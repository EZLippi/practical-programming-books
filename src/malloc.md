  <p>任何一个用过或学过C的人对malloc都不会陌生。大家都知道malloc可以分配一段连续的内存空间，并且在不再使用时可以通过free释放掉。但是，许多程序员对malloc背后的事情并不熟悉，许多人甚至把malloc当做操作系统所提供的系统调用或C的关键字。实际上，malloc只是C的标准库中提供的一个普通函数，而且实现malloc的<strong>基本</strong>思想并不复杂，任何一个对C和操作系统有些许了解的程序员都可以很容易理解。</p>
<p>这篇文章通过实现一个简单的malloc来描述malloc背后的机制。当然与现有C的标准库实现（例如glibc）相比，我们实现的malloc并不是特别高效，但是这个实现比目前真实的malloc实现要简单很多，因此易于理解。重要的是，这个实现和真实实现在基本原理上是一致的。</p>
<p>这篇文章将首先介绍一些所需的基本知识，如操作系统对进程的内存管理以及相关的系统调用，然后逐步实现一个简单的malloc。为了简单起见，这篇文章将只考虑x86_64体系结构，操作系统为Linux。</p>
<!-- toc -->

<ul>
<li><a href="#1-什么是malloc">1 什么是malloc</a></li>
<li><a href="#2-预备知识">2 预备知识</a><ul>
<li><a href="#21-linux内存管理">2.1 Linux内存管理</a><ul>
<li><a href="#211-虚拟内存地址与物理内存地址">2.1.1 虚拟内存地址与物理内存地址</a></li>
<li><a href="#212-页与地址构成">2.1.2 页与地址构成</a></li>
<li><a href="#213-内存页与磁盘页">2.1.3 内存页与磁盘页</a></li>
</ul>
</li>
<li><a href="#22-linux进程级内存管理">2.2 Linux进程级内存管理</a><ul>
<li><a href="#221-内存排布">2.2.1 内存排布</a></li>
<li><a href="#222-heap内存模型">2.2.2 Heap内存模型</a></li>
<li><a href="#223-brk与sbrk">2.2.3 brk与sbrk</a></li>
<li><a href="#224-资源限制与rlimit">2.2.4 资源限制与rlimit</a></li>
</ul>
</li>
</ul>
</li>
<li><a href="#3-实现malloc">3 实现malloc</a><ul>
<li><a href="#31-玩具实现">3.1 玩具实现</a></li>
<li><a href="#32-正式实现">3.2 正式实现</a><ul>
<li><a href="#321-数据结构">3.2.1 数据结构</a></li>
<li><a href="#322-寻找合适的block">3.2.2 寻找合适的block</a></li>
<li><a href="#323-开辟新的block">3.2.3 开辟新的block</a></li>
<li><a href="#324-分裂block">3.2.4 分裂block</a></li>
<li><a href="#325-malloc的实现">3.2.5 malloc的实现</a></li>
<li><a href="#326-calloc的实现">3.2.6 calloc的实现</a></li>
<li><a href="#327-free的实现">3.2.7 free的实现</a></li>
<li><a href="#328-realloc的实现">3.2.8 realloc的实现</a></li>
</ul>
</li>
<li><a href="#33-遗留问题和优化">3.3 遗留问题和优化</a></li>
</ul>
</li>
<li><a href="#4-其它参考">4 其它参考</a></li>
</ul>
<!-- toc stop -->


<h1 id="1-什么是malloc">1 什么是malloc</h1>
<p>在实现malloc之前，先要相对正式地对malloc做一个定义。</p>
<p>根据标准C库函数的定义，malloc具有如下原型：</p>
<pre class="prettyprint linenums lang-c">void* malloc(size_t size);
</pre>
<p>这个函数要实现的功能是在系统中分配一段连续的可用的内存，具体有如下要求：</p>
<ul>
<li>malloc分配的内存大小<strong>至少</strong>为size参数所指定的字节数</li>
<li>malloc的返回值是一个指针，指向一段可用内存的起始地址</li>
<li>多次调用malloc所分配的地址不能有重叠部分，除非某次malloc所分配的地址被释放掉</li>
<li>malloc应该尽快完成内存分配并返回（不能使用<a href="http://en.wikipedia.org/wiki/NP-hard">NP-hard</a>的内存分配算法）</li>
<li>实现malloc时应同时实现内存大小调整和内存释放函数（即realloc和free）</li>
</ul>
<p>对于malloc更多的说明可以在命令行中键入以下命令查看：</p>
<pre class="prettyprint linenums lang-bash">man malloc
</pre>
<h1 id="2-预备知识">2 预备知识</h1>
<p>在实现malloc之前，需要先解释一些Linux系统内存相关的知识。</p>
<h2 id="21-linux内存管理">2.1 Linux内存管理</h2>
<h3 id="211-虚拟内存地址与物理内存地址">2.1.1 虚拟内存地址与物理内存地址</h3>
<p>为了简单，现代操作系统在处理内存地址时，普遍采用虚拟内存地址技术。即在汇编程序（或机器语言）层面，当涉及内存地址时，都是使用虚拟内存地址。采用这种技术时，每个进程仿佛自己独享一片$2^N$字节的内存，其中$N$是机器位数。例如在64位CPU和64位操作系统下，每个进程的虚拟地址空间为$2^{64}$Byte。</p>
<p>这种虚拟地址空间的作用主要是简化程序的编写及方便操作系统对进程间内存的隔离管理，真实中的进程不太可能（也用不到）如此大的内存空间，实际能用到的内存取决于物理内存大小。</p>
<p>由于在机器语言层面都是采用虚拟地址，当实际的机器码程序涉及到内存操作时，需要根据当前进程运行的实际上下文将虚拟地址转换为物理内存地址，才能实现对真实内存数据的操作。这个转换一般由一个叫<a href="http://en.wikipedia.org/wiki/Memory_management_unit">MMU</a>（Memory Management Unit）的硬件完成。</p>
<h3 id="212-页与地址构成">2.1.2 页与地址构成</h3>
<p>在现代操作系统中，不论是虚拟内存还是物理内存，都不是以字节为单位进行管理的，而是以页（Page）为单位。一个内存页是一段固定大小的连续内存地址的总称，具体到Linux中，典型的内存页大小为4096Byte（4K）。</p>
<p>所以内存地址可以分为页号和页内偏移量。下面以64位机器，4G物理内存，4K页大小为例，虚拟内存地址和物理内存地址的组成如下：</p>
<p><img src="http://blog-codinglabs-org.qiniudn.com/image/a-malloc-tutorial-01.png" alt="内存地址构成"></p>
<p>上面是虚拟内存地址，下面是物理内存地址。由于页大小都是4K，所以页内便宜都是用低12位表示，而剩下的高地址表示页号。</p>
<p>MMU映射单位并不是字节，而是页，这个映射通过查一个常驻内存的数据结构<a href="http://en.wikipedia.org/wiki/Page_table">页表</a>来实现。现在计算机具体的内存地址映射比较复杂，为了加快速度会引入一系列缓存和优化，例如<a href="http://en.wikipedia.org/wiki/Translation_lookaside_buffer">TLB</a>等机制。下面给出一个经过简化的内存地址翻译示意图，虽然经过了简化，但是基本原理与现代计算机真实的情况的一致的。</p>
<p><img src="http://blog-codinglabs-org.qiniudn.com/image/a-malloc-tutorial-02.png" alt="内存地址翻译"></p>
<h3 id="213-内存页与磁盘页">2.1.3 内存页与磁盘页</h3>
<p>我们知道一般将内存看做磁盘的的缓存，有时MMU在工作时，会发现页表表明某个内存页不在物理内存中，此时会触发一个缺页异常（Page Fault），此时系统会到磁盘中相应的地方将磁盘页载入到内存中，然后重新执行由于缺页而失败的机器指令。关于这部分，因为可以看做对malloc实现是透明的，所以不再详细讲述，有兴趣的可以参考《深入理解计算机系统》相关章节。</p>
<p>最后附上一张在维基百科找到的更加符合真实地址翻译的流程供大家参考，这张图加入了TLB和缺页异常的流程（<a href="http://en.wikipedia.org/wiki/Page_table">图片来源页</a>）。</p>
<p><img src="http://blog-codinglabs-org.qiniudn.com/image/a-malloc-tutorial-03.png" alt="较为完整的地址翻译流程"></p>
<h2 id="22-linux进程级内存管理">2.2 Linux进程级内存管理</h2>
<h3 id="221-内存排布">2.2.1 内存排布</h3>
<p>明白了虚拟内存和物理内存的关系及相关的映射机制，下面看一下具体在一个进程内是如何排布内存的。</p>
<p>以Linux 64位系统为例。理论上，64bit内存地址可用空间为0x0000000000000000 ~ 0xFFFFFFFFFFFFFFFF，这是个相当庞大的空间，Linux实际上只用了其中一小部分（256T）。</p>
<p>根据<a href="https://www.kernel.org/doc/Documentation/x86/x86_64/mm.txt">Linux内核相关文档</a>描述，Linux64位操作系统仅使用低47位，高17位做扩展（只能是全0或全1）。所以，实际用到的地址为空间为0x0000000000000000 ~ 0x00007FFFFFFFFFFF和0xFFFF800000000000 ~ 0xFFFFFFFFFFFFFFFF，其中前面为用户空间（User Space），后者为内核空间（Kernel Space）。图示如下：</p>
<p><img src="http://blog-codinglabs-org.qiniudn.com/image/a-malloc-tutorial-04.png" alt="Linux进程地址排布"></p>
<p>对用户来说，主要关注的空间是User Space。将User Space放大后，可以看到里面主要分为如下几段：</p>
<ul>
<li>Code：这是整个用户空间的最低地址部分，存放的是指令（也就是程序所编译成的可执行机器码）</li>
<li>Data：这里存放的是初始化过的全局变量</li>
<li>BSS：这里存放的是未初始化的全局变量</li>
<li>Heap：堆，这是我们本文重点关注的地方，堆自低地址向高地址增长，后面要讲到的brk相关的系统调用就是从这里分配内存</li>
<li>Mapping Area：这里是与mmap系统调用相关的区域。大多数实际的malloc实现会考虑通过mmap分配较大块的内存区域，本文不讨论这种情况。这个区域自高地址向低地址增长</li>
<li>Stack：这是栈区域，自高地址向低地址增长</li>
</ul>
<p>下面我们主要关注Heap区域的操作。对整个Linux内存排布有兴趣的同学可以参考其它资料。</p>
<h3 id="222-heap内存模型">2.2.2 Heap内存模型</h3>
<p>一般来说，malloc所申请的内存主要从Heap区域分配（本文不考虑通过mmap申请大块内存的情况）。</p>
<p>由上文知道，进程所面对的虚拟内存地址空间，只有按页映射到物理内存地址，才能真正使用。受物理存储容量限制，整个堆虚拟内存空间不可能全部映射到实际的物理内存。Linux对堆的管理示意如下：</p>
<p><img src="http://blog-codinglabs-org.qiniudn.com/image/a-malloc-tutorial-05.png" alt="Linux进程堆管理"></p>
<p>Linux维护一个break指针，这个指针指向堆空间的某个地址。从堆起始地址到break之间的地址空间为映射好的，可以供进程访问；而从break往上，是未映射的地址空间，如果访问这段空间则程序会报错。</p>
<h3 id="223-brk与sbrk">2.2.3 brk与sbrk</h3>
<p>由上文知道，要增加一个进程实际的可用堆大小，就需要将break指针向高地址移动。Linux通过brk和sbrk系统调用操作break指针。两个系统调用的原型如下：</p>
<pre class="prettyprint linenums lang-c">int brk(void *addr);
void *sbrk(intptr_t increment);
</pre>
<p>brk将break指针直接设置为某个地址，而sbrk将break从当前位置移动increment所指定的增量。brk在执行成功时返回0，否则返回-1并设置errno为ENOMEM；sbrk成功时返回break移动之前所指向的地址，否则返回(void *)-1。</p>
<p>一个小技巧是，如果将increment设置为0，则可以获得当前break的地址。</p>
<p>另外需要注意的是，由于Linux是按页进行内存映射的，所以如果break被设置为没有按页大小对齐，则系统实际上会在最后映射一个完整的页，从而实际已映射的内存空间比break指向的地方要大一些。但是使用break之后的地址是很危险的（尽管也许break之后确实有一小块可用内存地址）。</p>
<h3 id="224-资源限制与rlimit">2.2.4 资源限制与rlimit</h3>
<p>系统对每一个进程所分配的资源不是无限的，包括可映射的内存空间，因此每个进程有一个rlimit表示当前进程可用的资源上限。这个限制可以通过getrlimit系统调用得到，下面代码获取当前进程虚拟内存空间的rlimit：</p>
<pre class="prettyprint linenums lang-c">int main() {
    struct rlimit *limit = (struct rlimit *)malloc(sizeof(struct rlimit));
    getrlimit(RLIMIT_AS, limit);
    printf(&quot;soft limit: %ld, hard limit: %ld\n&quot;, limit-&gt;rlim_cur, limit-&gt;rlim_max);
}
</pre>
<p>其中rlimit是一个结构体：</p>
<pre class="prettyprint linenums lang-c">struct rlimit {
    rlim_t rlim_cur;  /* Soft limit */
    rlim_t rlim_max;  /* Hard limit (ceiling for rlim_cur) */
};
</pre>
<p>每种资源有软限制和硬限制，并且可以通过setrlimit对rlimit进行有条件设置。其中硬限制作为软限制的上限，非特权进程只能设置软限制，且不能超过硬限制。</p>
<h1 id="3-实现malloc">3 实现malloc</h1>
<h2 id="31-玩具实现">3.1 玩具实现</h2>
<p>在正式开始讨论malloc的实现前，我们可以利用上述知识实现一个简单但几乎没法用于真实的玩具malloc，权当对上面知识的复习：</p>
<pre class="prettyprint linenums lang-c">/* 一个玩具malloc */
#include &lt;sys/types.h&gt;
#include &lt;unistd.h&gt;
void *malloc(size_t size)
{
    void *p;
    p = sbrk(0);
    if (sbrk(size) == (void *)-1)
        return NULL;
    return p;
}
</pre>
<p>这个malloc每次都在当前break的基础上增加size所指定的字节数，并将之前break的地址返回。这个malloc由于对所分配的内存缺乏记录，不便于内存释放，所以无法用于真实场景。</p>
<h2 id="32-正式实现">3.2 正式实现</h2>
<p>下面严肃点讨论malloc的实现方案。</p>
<h3 id="321-数据结构">3.2.1 数据结构</h3>
<p>首先我们要确定所采用的数据结构。一个简单可行方案是将堆内存空间以块（Block）的形式组织起来，每个块由meta区和数据区组成，meta区记录数据块的元信息（数据区大小、空闲标志位、指针等等），数据区是真实分配的内存区域，并且数据区的第一个字节地址即为malloc返回的地址。</p>
<p>可以用如下结构体定义一个block：</p>
<pre class="prettyprint linenums lang-c">typedef struct s_block *t_block;
struct s_block {
    size_t size;  /* 数据区大小 */
    t_block next; /* 指向下个块的指针 */
    int free;     /* 是否是空闲块 */
    int padding;  /* 填充4字节，保证meta块长度为8的倍数 */
    char data[1]  /* 这是一个虚拟字段，表示数据块的第一个字节，长度不应计入meta */
};
</pre>
<p>由于我们只考虑64位机器，为了方便，我们在结构体最后填充一个int，使得结构体本身的长度为8的倍数，以便内存对齐。示意图如下：</p>
<p><img src="http://blog-codinglabs-org.qiniudn.com/image/a-malloc-tutorial-06.png" alt="Block结构"></p>
<h3 id="322-寻找合适的block">3.2.2 寻找合适的block</h3>
<p>现在考虑如何在block链中查找合适的block。一般来说有两种查找算法：</p>
<ul>
<li><strong>First fit</strong>：从头开始，使用第一个数据区大小大于要求size的块所谓此次分配的块</li>
<li><strong>Best fit</strong>：从头开始，遍历所有块，使用数据区大小大于size且差值最小的块作为此次分配的块</li>
</ul>
<p>两种方法各有千秋，best fit具有较高的内存使用率（payload较高），而first fit具有更好的运行效率。这里我们采用first fit算法。</p>
<pre class="prettyprint linenums lang-c">/* First fit */
t_block find_block(t_block *last, size_t size) {
    t_block b = first_block;
    while(b &amp;&amp; !(b-&gt;free &amp;&amp; b-&gt;size &gt;= size)) {
        *last = b;
        b = b-&gt;next;
    }
    return b;
}
</pre>
<p>find_block从frist_block开始，查找第一个符合要求的block并返回block起始地址，如果找不到这返回NULL。这里在遍历时会更新一个叫last的指针，这个指针始终指向当前遍历的block。这是为了如果找不到合适的block而开辟新block使用的，具体会在接下来的一节用到。</p>
<h3 id="323-开辟新的block">3.2.3 开辟新的block</h3>
<p>如果现有block都不能满足size的要求，则需要在链表最后开辟一个新的block。这里关键是如何只使用sbrk创建一个struct：</p>
<pre class="prettyprint linenums lang-c">#define BLOCK_SIZE 24 /* 由于存在虚拟的data字段，sizeof不能正确计算meta长度，这里手工设置 */

t_block extend_heap(t_block last, size_t s) {
    t_block b;
    b = sbrk(0);
    if(sbrk(BLOCK_SIZE + s) == (void *)-1)
        return NULL;
    b-&gt;size = s;
    b-&gt;next = NULL;
    if(last)
        last-&gt;next = b;
    b-&gt;free = 0;
    return b;
}
</pre>
<h3 id="324-分裂block">3.2.4 分裂block</h3>
<p>First fit有一个比较致命的缺点，就是可能会让很小的size占据很大的一块block，此时，为了提高payload，应该在剩余数据区足够大的情况下，将其分裂为一个新的block，示意如下：</p>
<p><img src="http://blog-codinglabs-org.qiniudn.com/image/a-malloc-tutorial-07.png" alt="分裂block"></p>
<p>实现代码：</p>
<pre class="prettyprint linenums lang-c">void split_block(t_block b, size_t s) {
    t_block new;
    new = b-&gt;data + s;
    new-&gt;size = b-&gt;size - s - BLOCK_SIZE ;
    new-&gt;next = b-&gt;next;
    new-&gt;free = 1;
    b-&gt;size = s;
    b-&gt;next = new;
}
</pre>
<h3 id="325-malloc的实现">3.2.5 malloc的实现</h3>
<p>有了上面的代码，我们可以利用它们整合成一个简单但初步可用的malloc。注意首先我们要定义个block链表的头first_block，初始化为NULL；另外，我们需要剩余空间至少有BLOCK_SIZE + 8才执行分裂操作。</p>
<p>由于我们希望malloc分配的数据区是按8字节对齐，所以在size不为8的倍数时，我们需要将size调整为大于size的最小的8的倍数：</p>
<pre class="prettyprint linenums lang-c">size_t align8(size_t s) {
    if(s &amp; 0x7 == 0)
        return s;
    return ((s &gt;&gt; 3) + 1) &lt;&lt; 3;
}
</pre>
<pre class="prettyprint linenums lang-c">#define BLOCK_SIZE 24
void *first_block=NULL;

/* other functions... */

void *malloc(size_t size) {
    t_block b, last;
    size_t s;
    /* 对齐地址 */
    s = align8(size);
    if(first_block) {
        /* 查找合适的block */
        last = first_block;
        b = find_block(&amp;last, s);
        if(b) {
            /* 如果可以，则分裂 */
            if ((b-&gt;size - s) &gt;= ( BLOCK_SIZE + 8))
                split_block(b, s);
            b-&gt;free = 0;
        } else {
            /* 没有合适的block，开辟一个新的 */
            b = extend_heap(last, s);
            if(!b)
                return NULL;
        }
    } else {
        b = extend_heap(NULL, s);
        if(!b)
            return NULL;
        first_block = b;
    }
    return b-&gt;data;
}
</pre>
<h3 id="326-calloc的实现">3.2.6 calloc的实现</h3>
<p>有了malloc，实现calloc只要两步：</p>
<ol>
<li>malloc一段内存</li>
<li>将数据区内容置为0</li>
</ol>
<p>由于我们的数据区是按8字节对齐的，所以为了提高效率，我们可以每8字节一组置0，而不是一个一个字节设置。我们可以通过新建一个size_t指针，将内存区域强制看做size_t类型来实现。</p>
<pre class="prettyprint linenums lang-c">void *calloc(size_t number, size_t size) {
    size_t *new;
    size_t s8, i;
    new = malloc(number * size);
    if(new) {
        s8 = align8(number * size) &gt;&gt; 3;
        for(i = 0; i &lt; s8; i++)
            new[i] = 0;
    }
    return new;
}
</pre>
<h3 id="327-free的实现">3.2.7 free的实现</h3>
<p>free的实现并不像看上去那么简单，这里我们要解决两个关键问题：</p>
<ol>
<li>如何验证所传入的地址是有效地址，即确实是通过malloc方式分配的数据区首地址</li>
<li>如何解决碎片问题</li>
</ol>
<p>首先我们要保证传入free的地址是有效的，这个有效包括两方面：</p>
<ul>
<li>地址应该在之前malloc所分配的区域内，即在first_block和当前break指针范围内</li>
<li>这个地址确实是之前通过我们自己的malloc分配的</li>
</ul>
<p>第一个问题比较好解决，只要进行地址比较就可以了，关键是第二个问题。这里有两种解决方案：一是在结构体内埋一个magic number字段，free之前通过相对偏移检查特定位置的值是否为我们设置的magic number，另一种方法是在结构体内增加一个magic pointer，这个指针指向数据区的第一个字节（也就是在合法时free时传入的地址），我们在free前检查magic pointer是否指向参数所指地址。这里我们采用第二种方案：</p>
<p>首先我们在结构体中增加magic pointer（同时要修改BLOCK_SIZE）：</p>
<pre class="prettyprint linenums lang-c">typedef struct s_block *t_block;
struct s_block {
    size_t size;  /* 数据区大小 */
    t_block next; /* 指向下个块的指针 */
    int free;     /* 是否是空闲块 */
    int padding;  /* 填充4字节，保证meta块长度为8的倍数 */
    void *ptr;    /* Magic pointer，指向data */
    char data[1]  /* 这是一个虚拟字段，表示数据块的第一个字节，长度不应计入meta */
};
</pre>
<p>然后我们定义检查地址合法性的函数：</p>
<pre class="prettyprint linenums lang-c">t_block get_block(void *p) {
    char *tmp;  
    tmp = p;
    return (p = tmp -= BLOCK_SIZE);
}

int valid_addr(void *p) {
    if(first_block) {
        if(p &gt; first_block &amp;&amp; p &lt; sbrk(0)) {
            return p == (get_block(p))-&gt;ptr;
        }
    }
    return 0;
}
</pre>
<p>当多次malloc和free后，整个内存池可能会产生很多碎片block，这些block很小，经常无法使用，甚至出现许多碎片连在一起，虽然总体能满足某此malloc要求，但是由于分割成了多个小block而无法fit，这就是碎片问题。</p>
<p>一个简单的解决方式时当free某个block时，如果发现它相邻的block也是free的，则将block和相邻block合并。为了满足这个实现，需要将s_block改为双向链表。修改后的block结构如下：</p>
<pre class="prettyprint linenums lang-c">typedef struct s_block *t_block;
struct s_block {
    size_t size;  /* 数据区大小 */
    t_block prev; /* 指向上个块的指针 */
    t_block next; /* 指向下个块的指针 */
    int free;     /* 是否是空闲块 */
    int padding;  /* 填充4字节，保证meta块长度为8的倍数 */
    void *ptr;    /* Magic pointer，指向data */
    char data[1]  /* 这是一个虚拟字段，表示数据块的第一个字节，长度不应计入meta */
};
</pre>
<p>合并方法如下：</p>
<pre class="prettyprint linenums lang-c">t_block fusion(t_block b) {
    if (b-&gt;next &amp;&amp; b-&gt;next-&gt;free) {
        b-&gt;size += BLOCK_SIZE + b-&gt;next-&gt;size;
        b-&gt;next = b-&gt;next-&gt;next;
        if(b-&gt;next)
            b-&gt;next-&gt;prev = b;
    }
    return b;
}
</pre>
<p>有了上述方法，free的实现思路就比较清晰了：首先检查参数地址的合法性，如果不合法则不做任何事；否则，将此block的free标为1，并且在可以的情况下与后面的block进行合并。如果当前是最后一个block，则回退break指针释放进程内存，如果当前block是最后一个block，则回退break指针并设置first_block为NULL。实现如下：</p>
<pre class="prettyprint linenums lang-c">void free(void *p) {
    t_block b;
    if(valid_addr(p)) {
        b = get_block(p);
        b-&gt;free = 1;
        if(b-&gt;prev &amp;&amp; b-&gt;prev-&gt;free)
            b = fusion(b-&gt;prev);
        if(b-&gt;next)
            fusion(b);
        else {
            if(b-&gt;prev)
                b-&gt;prev-&gt;prev = NULL;
            else
                first_block = NULL;
            brk(b);
        }
    }
}
</pre>
<h3 id="328-realloc的实现">3.2.8 realloc的实现</h3>
<p>为了实现realloc，我们首先要实现一个内存复制方法。如同calloc一样，为了效率，我们以8字节为单位进行复制：</p>
<pre class="prettyprint linenums lang-c">void copy_block(t_block src, t_block dst) {
    size_t *sdata, *ddata;
    size_t i;
    sdata = src-&gt;ptr;
    ddata = dst-&gt;ptr;
    for(i = 0; (i * 8) &lt; src-&gt;size &amp;&amp; (i * 8) &lt; dst-&gt;size; i++)
        ddata[i] = sdata[i];
}
</pre>
<p>然后我们开始实现realloc。一个简单（但是低效）的方法是malloc一段内存，然后将数据复制过去。但是我们可以做的更高效，具体可以考虑以下几个方面：</p>
<ul>
<li>如果当前block的数据区大于等于realloc所要求的size，则不做任何操作</li>
<li>如果新的size变小了，考虑split</li>
<li>如果当前block的数据区不能满足size，但是其后继block是free的，并且合并后可以满足，则考虑做合并</li>
</ul>
<p>下面是realloc的实现：</p>
<pre class="prettyprint linenums lang-c">void *realloc(void *p, size_t size) {
    size_t s;
    t_block b, new;
    void *newp;
    if (!p)
        /* 根据标准库文档，当p传入NULL时，相当于调用malloc */
        return malloc(size);
    if(valid_addr(p)) {
        s = align8(size);
        b = get_block(p);
        if(b-&gt;size &gt;= s) {
            if(b-&gt;size - s &gt;= (BLOCK_SIZE + 8))
                split_block(b,s);
        } else {
            /* 看是否可进行合并 */
            if(b-&gt;next &amp;&amp; b-&gt;next-&gt;free
                    &amp;&amp; (b-&gt;size + BLOCK_SIZE + b-&gt;next-&gt;size) &gt;= s) {
                fusion(b);
                if(b-&gt;size - s &gt;= (BLOCK_SIZE + 8))
                    split_block(b, s);
            } else {
                /* 新malloc */
                newp = malloc (s);
                if (!newp)
                    return NULL;
                new = get_block(newp);
                copy_block(b, new);
                free(p);
                return(newp);
            }
        }
        return (p);
    }
    return NULL;
}
</pre>
<h2 id="33-遗留问题和优化">3.3 遗留问题和优化</h2>
<p>以上是一个较为简陋，但是初步可用的malloc实现。还有很多遗留的可能优化点，例如：</p>
<ul>
<li>同时兼容32位和64位系统</li>
<li>在分配较大快内存时，考虑使用mmap而非sbrk，这通常更高效</li>
<li>可以考虑维护多个链表而非单个，每个链表中的block大小均为一个范围内，例如8字节链表、16字节链表、24-32字节链表等等。此时可以根据size到对应链表中做分配，可以有效减少碎片，并提高查询block的速度</li>
<li>可以考虑链表中只存放free的block，而不存放已分配的block，可以减少查找block的次数，提高效率</li>
</ul>
<p>还有很多可能的优化，这里不一一赘述。下面附上一些参考文献，有兴趣的同学可以更深入研究。</p>
<h1 id="4-其它参考">4 其它参考</h1>
<ol>
<li>这篇文章大量参考了<a href="http://www.inf.udec.cl/~leo/Malloc_tutorial.pdf">A malloc Tutorial</a>，其中一些图片和代码直接引用了文中的内容，这里特别指出</li>
<li><a href="http://csapp.cs.cmu.edu/">Computer Systems: A Programmer&#39;s Perspective, 2/E</a>一书有许多值得参考的地方</li>
<li>关于Linux的虚拟内存模型，<a href="http://duartes.org/gustavo/blog/post/anatomy-of-a-program-in-memory/">Anatomy of a Program in Memory</a>是很好的参考资料，另外作者还有一篇<a href="http://duartes.org/gustavo/blog/post/how-the-kernel-manages-your-memory/">How the Kernel Manages Your Memory</a>对于Linux内核中虚拟内存管理的部分有很好的讲解</li>
<li>对于真实世界的malloc实现，可以参考<a href="http://repo.or.cz/w/glibc.git/blob/HEAD:/malloc/malloc.c">glibc的实现</a></li>
<li>本文写作过程中大量参考了<a href="http://www.wikipedia.org/">维基百科</a>，再次感谢这个伟大的网站，并且呼吁大家在手头允许的情况下可以适当捐助维基百科，帮助这个造福人类的系统运行下去</li>
</ol>