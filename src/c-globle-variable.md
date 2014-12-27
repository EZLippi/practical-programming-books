<p>作为一名程序员，如果说沉迷一门编程语言算作一种乐趣的话，那么与此同时反过来去黑一门编程语言就是这种乐趣的升华。今天我们就来黑一把C语言，好好展示一下这门经典语言令人抓狂的一面。</p>
<p>我们知道，全局变量是C语言语法和语义中一个很重要的知识点，首先它的存在意义需要从三个不同角度去理解：对于程序员来说，它是一个记录内容的<strong>变量(variable)</strong>；对于编译/链接器来说，它是一个需要解析的<strong>符号(symbol)</strong>；对于计算机来说，它可能是具有地址的一块<strong>内存(memory)</strong>。其次是语法/语义：从作用域上看，带static关键字的全局变量范围只能限定在文件里，否则会外联到整个模块和项目中；从生存期来看，它是静态的，贯穿整个程序或模块运行期间（<span style="color: #ff0000;"><strong>注意，正是跨单元访问和持续生存周期这两个特点使得全局变量往往成为一段受攻击代码的突破口，了解这一点十分重要</strong></span>）；从空间分配上看，定义且初始化的全局变量在编译时在数据段(.data)分配空间，定义但未初始化的全局变量<strong>暂存(tentative definition)</strong>在.bss段，编译时自动清零，而仅仅是声明的全局变量只能算个符号，寄存在编译器的符号表内，不会分配空间，直到链接或者运行时再重定向到相应的地址上。</p>
<p>我们将向您展现一下，<strong>非static限定全局变量</strong>在编译/链接以及程序运行时会发生哪些有趣的事情，顺便可以对C编译器/链接器的解析原理管中窥豹。以下示例对ANSI C和GNU C标准都有效，笔者的编译环境是Ubuntu下的GCC-4.4.3。</p>
<p><span id="more-10115"></span></p>
<h4>第一个例子</h4>
<pre class="brush: cpp; title: ; notranslate" title="">/* t.h */
#ifndef _H_
#define _H_
int a;
#endif

/* foo.c */
#include &lt;stdio.h&gt;
#include &quot;t.h&quot;

struct {
   char a;
   int b;
} b = { 2, 4 };

int main();

void foo()
{
    printf(&quot;foo:\t(&amp;a)=0x%08x\n\t(&amp;b)=0x%08x\n
        \tsizeof(b)=%d\n\tb.a=%d\n\tb.b=%d\n\tmain:0x%08x\n&quot;,
        &amp;a, &amp;b, sizeof b, b.a, b.b, main);
}

/* main.c */
#include &lt;stdio.h&gt;
#include &quot;t.h&quot;

int b;
int c;

int main()
{
    foo();
    printf(&quot;main:\t(&amp;a)=0x%08x\n\t(&amp;b)=0x%08x\n
        \t(&amp;c)=0x%08x\n\tsize(b)=%d\n\tb=%d\n\tc=%d\n&quot;,
        &amp;a, &amp;b, &amp;c, sizeof b, b, c);
	return 0;
}
</pre>
<p>Makefile如下：</p>
<pre class="brush: bash; title: ; notranslate" title="">
test: main.o foo.o
	gcc -o test main.o foo.o

main.o: main.c
foo.o: foo.c

clean:
	rm *.o test
</pre>
<p>运行情况：</p>
<pre class="brush: bash; title: ; notranslate" title="">
foo:	(&amp;a)=0x0804a024
	(&amp;b)=0x0804a014
	sizeof(b)=8
	b.a=2
	b.b=4
	main:0x080483e4
main:	(&amp;a)=0x0804a024
	(&amp;b)=0x0804a014
	(&amp;c)=0x0804a028
	size(b)=4
	b=2
	c=0
</pre>
<p>这个项目里我们定义了四个全局变量，t.h头文件定义了一个整型a，main.c里定义了两个整型b和c并且未初始化，foo.c里定义了一个初始化了的结构体，还定义了一个main的函数指针变量。由于C语言每个源文件单独编译，所以t.h分别包含了两次，所以int a就被定义了两次。两个源文件里变量b和函数指针变量main被重复定义了，实际上可以看做代码段的地址。但编译器并未报错，只给出一条警告：</p>
<pre class="brush: bash; title: ; notranslate" title="">/usr/bin/ld: Warning: size of symbol 'b' changed from 4 in main.o to 8 in foo.o</pre>
<p>运行程序发现，main.c打印中b大小是4个字节，而foo.c是8个字节，因为sizeof关键字是编译时决议，而源文件中对b类型定义不一样。但令人惊奇的是无论是在main.c还是foo.c中，a和b都是相同的地址，也就是说，a和b被定义了两次，b还是不同类型，但内存映像中只有一份拷贝。我们还看到，main.c中b的值居然就是foo.c中结构体第一个成员变量b.a的值，这证实了前面的推断——<strong>即便存在多次定义，内存中只有一份初始化的拷贝。</strong>另外在这里c是置身事外的一个独立变量。</p>
<p>为何会这样呢？这涉及到<strong>C编译器对多重定义的全局符号的解析和链接。</strong>在编译阶段，编译器将全局符号信息隐含地编码在可重定位目标文件的符号表里。这里有个<strong>“强符号(strong)”</strong>和<strong>“弱符号(weak)”</strong>的概念——前者指的是定义并且初始化了的变量，比如foo.c里的结构体b，后者指的是未定义或者定义但未初始化的变量，比如main.c里的整型b和c，还有两个源文件都包含头文件里的a。当符号被多重定义时，GNU链接器(ld)使用以下规则决议：</p>
<ul>
<li>不允许出现多个相同强符号。</li>
</ul>
<ul>
<li>如果有一个强符号和多个弱符号，则选择强符号。</li>
</ul>
<ul>
<li>如果有多个弱符号，那么先决议到size最大的那个，如果同样大小，则按照链接顺序选择第一个。</li>
</ul>
<p>像上面这个例子中，全局变量a和b存在重复定义。如果我们将main.c中的b初始化赋值，那么就存在两个强符号而违反了规则一，编译器报错。如果满足规则二，则仅仅提出警告，实际运行时决议的是foo.c中的强符号。而变量a都是弱符号，所以只选择一个（按照目标文件链接时的顺序）。</p>
<p>事实上，这种规则是C语言里的一个大坑，编译器对这种全局变量多重定义的“纵容”很可能会无端修改某个变量，导致程序不确定行为。如果你还没有意识到事态严重性，我再举个例子。</p>
<h4>第二个例子</h4>
<pre class="brush: cpp; title: ; notranslate" title="">/* foo.c */
#include &lt;stdio.h&gt;;

struct {
    int a;
    int b;
} b = { 2, 4 };

int main();

void foo()
{
    printf(&quot;foo:\t(&amp;b)=0x%08x\n\tsizeof(b)=%d\n
        \tb.a=%d\n\tb.b=%d\n\tmain:0x%08x\n&quot;,
        &amp;b, sizeof b, b.a, b.b, main);
}

/* main.c */
#include &lt;stdio.h&gt;

int b;
int c;

int main()
{
    if (0 == fork()) {
        sleep(1);
        b = 1;
        printf(&quot;child:\tsleep(1)\n\t(&amp;b):0x%08x\n
            \t(&amp;c)=0x%08x\n\tsizeof(b)=%d\n\tset b=%d\n\tc=%d\n&quot;,
            &amp;b, &amp;c, sizeof b, b, c);
        foo();
    } else {
        foo();
        printf(&quot;parent:\t(&amp;b)=0x%08x\n\t(&amp;c)=0x%08x\n
            \tsizeof(b)=%d\n\tb=%d\n\tc=%d\n\twait child...\n&quot;,
            &amp;b, &amp;c, sizeof b, b, c);
        wait(-1);
        printf(&quot;parent:\tchild over\n\t(&amp;b)=0x%08x\n
            \t(&amp;c)=0x%08x\n\tsizeof(b)=%d\n\tb=%d\n\tc=%d\n&quot;,
            &amp;b, &amp;c, sizeof b, b, c);
    }
    return 0;
}</pre>
<p>运行情况如下：</p>
<pre class="brush: bash; title: ; notranslate" title="">
foo:	(&amp;b)=0x0804a020
	sizeof(b)=8
	b.a=2
	b.b=4
	main:0x080484c8
parent:	(&amp;b)=0x0804a020
	(&amp;c)=0x0804a034
	sizeof(b)=4
	b=2
	c=0
	wait child...
child:	sleep(1)
	(&amp;b):0x0804a020
	(&amp;c)=0x0804a034
	sizeof(b)=4
	set b=1
	c=0
foo:	(&amp;b)=0x0804a020
	sizeof(b)=8
	b.a=1
	b.b=4
	main:0x080484c8
parent:	child over
	(&amp;b)=0x0804a020
	(&amp;c)=0x0804a034
	sizeof(b)=4
	b=2
	c=0
</pre>
<p>（说明一点，运行情况是直接输出到stdout的打印，笔者曾经将./test输出重定向到log中，结果发现打印的执行序列不一致，所以采用默认输出。）</p>
<p>这是一个<strong>多进程环境</strong>，首先我们看到无论父进程还是子进程，main.c还是foo.c，全局变量b和c的地址仍然是一致的（当然只是个<strong>逻辑地址</strong>），而且对b的大小不同模块仍然有不同的决议。这里值得注意的是，我们在子进程中对变量b进行赋值动作，从此子进程本身包括foo()调用中，整型b以及结构体成员b.a的值都是1，而父进程中整型b和结构体成员b.a的值仍是2，但它们显示的逻辑地址仍是一致的。</p>
<p>个人认为可以这样解释，fork创建新进程时，子进程获得了父进程上下文“镜像”（自然包括全局变量），虚拟地址相同但属于不同的进程空间，而且此时真正映射的物理地址中只有一份拷贝，所以b的值是相同的（都是2）。随后子进程对b改写，触发了操作系统的<strong>写时拷贝(copy on write)</strong>机制，这时物理内存中才产生真正的两份拷贝，分别映射到不同进程空间的虚拟地址上，但虚拟地址的值本身仍然不变，这对于应用程序来说是透明的，具有隐瞒性。</p>
<p>还有一点值得注意，这个示例编译时没有出现第一个示例的警告，即对变量b的sizeof决议，笔者也不知道为什么，或许是GCC的一个bug？</p>
<h4>第三个例子</h4>
<p>这个例子代码同上一个一致，只不过我们将foo.c做成一个静态链接库libfoo.a进行链接，这里只给出Makefile的改动。</p>
<pre class="brush: bash; title: ; notranslate" title="">
test: main.o foo.o
	ar rcs libfoo.a foo.o
	gcc -static -o test main.o libfoo.a

main.o: main.c
foo.o: foo.c

clean:
	rm -f *.o test
</pre>
<p>运行情况如下：</p>
<pre class="brush: bash; title: ; notranslate" title="">
foo:	(&amp;b)=0x080ca008
	sizeof(b)=8
	b.a=2
	b.b=4
	main:0x08048250
parent:	(&amp;b)=0x080ca008
	(&amp;c)=0x080cc084
	sizeof(b)=4
	b=2
	c=0
	wait child...
child:	sleep(1)
	(&amp;b):0x080ca008
	(&amp;c)=0x080cc084
	sizeof(b)=4
	set b=1
	c=0
foo:	(&amp;b)=0x080ca008
	sizeof(b)=8
	b.a=1
	b.b=4
	main:0x08048250
parent:	child over
	(&amp;b)=0x080ca008
	(&amp;c)=0x080cc084
	sizeof(b)=4
	b=2
	c=0
</pre>
<p>从这个例子看不出有啥差别，只不过使用<strong>静态链接</strong>后，全局变量加载的地址有所改变，b和c的地址之间似乎相隔更远了些。不过这次编译器倒是给出了变量b的sizeof决议警告。</p>
<p>到此为止，有些人可能会对上面的例子嗤之以鼻，觉得这不过是列举了C语言的某些特性而已，算不上黑。有些人认为既然如此，对于一切全局变量要么用static限死，要么定义同时初始化，杜绝弱符号，以便在编译时报错检测出来。只要小心地使用，C语言还是很完美的嘛~对于抱这样想法的人，我只想说，请你在夜深人静的时候竖起耳朵仔细聆听，你很可能听到Dennis Richie在九泉之下邪恶的笑声——不，与其说是嘲笑，不如说是诅咒……</p>
<h4>第四个例子</h4>
<pre class="brush: cpp; title: ; notranslate" title="">/* foo.c */
#include &lt;stdio.h&gt;

const struct {
    int a;
    int b;
} b = { 3, 3 };

int main();

void foo()
{
    b.a = 4;
    b.b = 4;
    printf(&quot;foo:\t(&amp;b)=0x%08x\n\tsizeof(b)=%d\n
        \tb.a=%d\n\tb.b=%d\n\tmain:0x%08x\n&quot;,
        &amp;b, sizeof b, b.a, b.b, main);
}

/* t1.c */
#include &lt;stdio.h&gt;

int b = 1;
int c = 1;

int main()
{
    int count = 5;
    while (count-- &gt; 0) {
        t2();
        foo();
        printf(&quot;t1:\t(&amp;b)=0x%08x\n\t(&amp;c)=0x%08x\n
            \tsizeof(b)=%d\n\tb=%d\n\tc=%d\n&quot;,
            &amp;b, &amp;c, sizeof b, b, c);
        sleep(1);
    }
    return 0;
}

/* t2.c */
#include &lt;stdio.h&gt;

int b;
int c;

int t2()
{
    printf(&quot;t2:\t(&amp;b)=0x%08x\n\t(&amp;c)=0x%08x\n
        \tsizeof(b)=%d\n\tb=%d\n\tc=%d\n&quot;,
        &amp;b, &amp;c, sizeof b, b, c);
    return 0;
}</pre>
<p>Makefile脚本：</p>
<pre class="brush: bash; title: ; notranslate" title="">export LD_LIBRARY_PATH:=.

all: test
	./test

test: t1.o t2.o
	gcc -shared -fPIC -o libfoo.so foo.c
	gcc -o test t1.o t2.o -L. -lfoo

t1.o: t1.c
t2.o: t2.c

.PHONY:clean
clean:
	rm -f *.o *.so test*
</pre>
<p>执行结果：</p>
<pre class="brush: bash; title: ; notranslate" title="">
./test
t2:	(&amp;b)=0x0804a01c
	(&amp;c)=0x0804a020
	sizeof(b)=4
	b=1
	c=1
foo:	(&amp;b)=0x0804a01c
	sizeof(b)=8
	b.a=4
	b.b=4
	main:0x08048564
t1:	(&amp;b)=0x0804a01c
	(&amp;c)=0x0804a020
	sizeof(b)=4
	b=4
	c=4
t2:	(&amp;b)=0x0804a01c
	(&amp;c)=0x0804a020
	sizeof(b)=4
	b=4
	c=4
foo:	(&amp;b)=0x0804a01c
	sizeof(b)=8
	b.a=4
	b.b=4
	main:0x08048564
t1:	(&amp;b)=0x0804a01c
	(&amp;c)=0x0804a020
	sizeof(b)=4
	b=4
	c=4
	...</pre>
<p>其实前面几个例子只是开胃小菜而已，真正的大坑终于出现了！而且这次编译器既没报错也没警告，但我们确实眼睁睁地看到作为main()中强符号的b被改写了，而且一旁的c也“躺枪”了。眼尖的读者发现，这次foo.c是作为动态链接库运行时加载的，当t1第一次调用t2时，libfoo.so还未加载，一旦调用了foo函数，b立马中弹，而且<strong>c的地址居然还相邻着b，这使得c一同中弹了。</strong>不过笔者有些无法解释这种行为的原因，有种说法是强符号的全局变量在数据段中是连续分布的（相应地弱符号暂存在.bss段或者符号表里），或许可以上报GNU的编译器开发小组。</p>
<p>另外笔者尝试过将t1.c中的b和c定义前面加上<strong>const限定词</strong>，编译器仍然默认通过，但程序在main()中第一次调用foo()时触发了Segment fault异常导致奔溃，在foo.c里使用指针改写它也一样。<strong>推断这是GCC对const常量所在地址启用了类似操作系统写保护机制，但我无法确定早期版本的GCC是否会让这个const常量被改写而程序不会奔溃。</strong></p>
<p>至于<strong>volatile关键词</strong>之于全局变量，自测似乎没有影响。</p>
<p>怎么样？看了最后一个例子是否有点“不明觉厉”呢？C语言在你心目中是否还是当初那个“纯洁”、“干净”、“行为一致”的姑娘呢？也许趁着你不注意的时候她会偷偷给你戴顶绿帽，这一切都是通过全局变量，特别在动态链接的环境下，就算全部定义成强符号仍然无法为编译器所察觉。而一些IT界“恐怖分子”也经常<strong>将恶意代码包装成全局变量注入到root权限下存在漏洞的操作序列中，</strong>就像著名的栈溢出攻击那样。某一天当你傻傻地看着一个程序出现未定义的行为却无法定位原因的时候，请不要忘记Richie大爷那来自九泉之下最深沉的“问候”~</p>
<p>或许有些人会偷换概念，把这一切归咎于编译器和链接器身上，认为这同语言无关，但我要提醒你，正是编译/链接器的行为支撑了整个语言的语法和语义。你可以反过来思考一下为何C的胞弟C++推出<strong>“命名空间(namespace)”</strong>的概念，或者你可以使用其它高级语言，对于重定义的全局变量是否能通过编译这一关。</p>
<p>所以请时刻谨记，<span style="color: #ff0000;"><strong>C是一门很恐怖的语言！</strong></span></p>
<p>P.S.题外话写在最后。我无意挑起语言之争，只是就事论事地去<strong>“黑(hack)</strong><strong>”</strong>一门语言而已，而且要黑就要黑得有理有力有层次，还要带点娱乐精神。其实黑一门语言并非什么尖端复杂的技术，个人觉得起码要做到两点：</p>
<ul>
<li><strong>亲自动手写测试程序。</strong>动手写测试程序是开发人员必备的基础技能，只有现成的代码才能让人心服口服，那些只会停留在口头上的争论只能算作cheap hack。</li>
</ul>
<ul>
<li><strong>测试程序不能依赖于不成熟的代码。</strong>软件开发99%以上的bug都是基于不合格(substandard)开发人员导致，这并不能怪罪于语言以及编译器本身。使用诸如#define TRUE FALSE或者#define NULL 1之类的trick来黑C语言只能证明此人很有娱乐精神而不是真正的&#8221;hack value&#8221;，拿老北京梨园行当里的一句话——“那是下三滥的玩意儿”。</li>
</ul>
<p>（全文完）