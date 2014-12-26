<p>最近在阅读《<a href="http://shop.oreilly.com/product/9781565922259.do">sed &amp; awk（第二版）</a>》，这本书是sed和awk相关书籍中比较经典的一本。我在读书的时候有一个习惯，就是会作一些笔记，如果有条件我会放到博客中。写博客不仅是给别人看的，更是写给自己看的，同时因为写给别人看，所以必然会在一些细节的地方写得很清楚明了，可以加深自己对原书的理解，同时以后回头看的时候，我自己也能快速的回忆起来。</p>
<p>另外一方面，我会选择英文原版来阅读而非中文翻译版，主要是出于英文版的内容更加准确、容易领会作者的本意这个方面的原因。毕竟翻译的内容一方面因为翻译的时候会丢失一些原版的意思，同时因为不同的人有不同的理解，在翻译中可能会夹杂着自己个人的理解。就好比这一系列的文章，许多内容都是出自原书，我只不过是翻译了些内容加了点注解而已，所心也只能称之为笔记。</p>
<p>文中对一些术语的翻译只是按本人自己的喜好而定，请见谅。</p>
<p>本系列包含两部分的内容：sed篇和awk篇。</p>
<p>sed篇总共分成6章：</p>
<p>awk篇暂时还未计划。</p>
<p>&nbsp;</p>
<p><strong>Sed是什么</strong></p>
<p>《sed and awk》一书中（1.2 A Stream Editor）是这样解释的：</p>
<blockquote><p><span style="color: #888888;">Sed is a &#8220;non-interactive&#8221; stream-oriented editor. It is stream-oriented because, like many UNIX</span><br />
<span style="color: #888888;">programs, input flows through the program and is directed to standard output.</span></p></blockquote>
<p>Sed本质上是一个编辑器，但是它是非交互式的，这点与VIM不同；同时它又是面向字符流的，输入的字符流经过Sed的处理后输出。这两个特性使得Sed成为命令行下面非常有用的一个处理工具。</p>
<p>如果对sed的历史有兴趣，可以看书中2.1节&#8221;Awk, by Sed and Grep, out of Ed&#8221;，这里就不多介绍。</p>
<p><strong>基本概念</strong></p>
<p>sed命令的语法如下所示：</p>
<pre class="brush: bash; gutter: true">sed [options] script filename</pre>
<p>同大多数Linux命令一样，sed也是从stdin中读取输入，并且将输出写到stdout，但是当filename被指定时，则会从指定的文件中获取输入，输出可以重定向到文件中，但是需要注意的是，该文件绝对不能与输入的文件相同。</p>
<p>options是指sed的命令行参数，这一块并不是重点，参数也不多。</p>
<p>script是指需要对输入执行的一个或者多个操作指令(instruction)，sed会依次读取输入文件的每一行到缓存中并应用script中指定的操作指令，因此而带来的变化并不会影响最初的文件（<strong>注：</strong>如果使用sed时指定-i参数则会影响最初的文件）。如果操作指令很多，为了不影响可读性，可以将其写到文件中，并通过-f参数指定scriptfile:</p>
<pre class="brush: bash; gutter: true">sed -f scriptfile filename</pre>
<p>这里有一个建议，在命令行中指定的操作指令最好用单引号引起来，这样可以避免shell对特殊字符的处理（如空格、$等，关于这一点可以参考<a title="简洁的Bash编程技巧续篇" href="http://kodango.me/simple-bash-programming-skills-2">简洁的Bash编程技巧续篇 &#8211; 9. 引号之间的区别</a>)。这个建议同样适用grep/awk等命令，当然如果有时候确实不适合使用单引号时，记得对特殊字符转义。</p>
<p>无论是将操作指令通过命令行指定，还是写入到文件中作为一个sed脚本，必须包含至少一个指令，否则用sed就没有意义了。一般会同时指定多个操作指令，这时候指令之间的顺序就显得非常重要。而你的脑海中必须有这么一个概念，即每个指令应用后，当前输入的行会变成什么样子。要做到这一点首先必须要了解sed的工作原理，要做到“知其然，且知其所以然”。</p>
<p>每条操作指令由pattern和procedure两部分组成，pattern一般是用&#8217;/'分隔的正则表达式（在sed中也有可能是行号，具体参见<a title="Sed命令地址匹配问题总结" href="http://kodango.me/sed-address-matching-summary">Sed命令地址匹配问题总结</a>），而procedure则是一连串编辑命令(action)。</p>
<p>sed的处理流程，简化后是这样的：</p>
<p>1.读入新的一行内容到缓存空间；</p>
<p>2.从指定的操作指令中取出第一条指令，判断是否匹配pattern；</p>
<p>3.如果不匹配，则忽略后续的编辑命令，回到第2步继续取出下一条指令；</p>
<p>4.如果匹配，则针对缓存的行执行后续的编辑命令；完成后，回到第2步继续取出下一条指令；</p>
<p>5.当所有指令都应用之后，输出缓存行的内容；回到第1步继续读入下一行内容；</p>
<p>6.当所有行都处理完之后，结束；</p>
<p style="text-align: center;">具体流程见下图：<br />
<a href="http://jbcdn2.b0.upaiyun.com/2012/12/simple_sed_process_flow_chart1.png" rel="lightbox[31026]" title="《sed &amp; awk》笔记之 sed 篇"><img class="aligncenter  wp-image-31040" title="《sed &amp; awk》笔记之 sed 篇" src="http://jbcdn2.b0.upaiyun.com/2012/12/simple_sed_process_flow_chart1.png" alt="《sed &amp; awk》笔记之 sed 篇" width="453" height="378" /></a></p>
<p><strong>简单例子</strong></p>
<p>概念如果脱离实际的例子就会显得非常枯燥无趣，这本书在这一点上处理得很好，都是配上实际的例子来讲解的。不过有点遗憾的是，书中的例子大多是与troff macro有关的，我们很难有条件来实际测试例子，在笔记中我会尽量使用更加浅显易懂的例子来说明。</p>
<p>下面是摘自书中的一个例子，假设有个文件list：</p>
<pre class="brush: bash; gutter: true">John Daggett, 341 King Road, Plymouth MA
Alice Ford, 22 East Broadway, Richmond VA
Orville Thomas, 11345 Oak Bridge Road, Tulsa OK
Terry Kalkas, 402 Lans Road, Beaver Falls PA
Eric Adams, 20 Post Road, Sudbury MA
Hubert Sims, 328A Brook Road, Roanoke VA
Amy Wilde, 334 Bayshore Pkwy, Mountain View CA
Sal Carpenter, 73 6th Street, Boston MA</pre>
<p>这个例子中用到的知识点都会在后文中介绍，你可以先知道有这个东西就行。</p>
<p>假如，现在打算将MA替换成Massachusetts，可以使用替换命令s：</p>
<pre class="brush: bash; gutter: true">$ sed -e &#039;s/MA/Massachusetts/&#039; list
John Daggett, 341 King Road, Plymouth Massachusetts
Alice Ford, 22 East Broadway, Richmond VA
Orville Thomas, 11345 Oak Bridge Road, Tulsa OK
Terry Kalkas, 402 Lans Road, Beaver Falls PA
Eric Adams, 20 Post Road, Sudbury Massachusetts
Hubert Sims, 328A Brook Road, Roanoke VA
Amy Wilde, 334 Bayshore Pkwy, Mountain View CA
Sal Carpenter, 73 6th Street, Boston Massachusetts</pre>
<p>这里面的-e选项是可选的，这个参数只是在命令行中同时指定多个操作指令时才需要用到，如：</p>
<pre class="brush: bash; gutter: true">$ sed -e &#039;s/ MA/, Massachusetts/&#039; -e &#039;s/ PA/, Pennsylvania/&#039; list 
John Daggett, 341 King Road, Plymouth, Massachusetts
Alice Ford, 22 East Broadway, Richmond VA
Orville Thomas, 11345 Oak Bridge Road, Tulsa OK
Terry Kalkas, 402 Lans Road, Beaver Falls, Pennsylvania
Eric Adams, 20 Post Road, Sudbury, Massachusetts
Hubert Sims, 328A Brook Road, Roanoke VA
Amy Wilde, 334 Bayshore Pkwy, Mountain View CA
Sal Carpenter, 73 6th Street, Boston, Massachusetts</pre>
<p>即使在多个操作指令的情况下，-e参数也不是必需的，我一般不会加-e参数，比如上面的例子可以换成下面的写法：</p>
<pre class="brush: bash; gutter: true">$ sed &#039;s/ MA/, Massachusetts/;s/ PA/, Pennsylvania/&#039; list</pre>
<p>操作指令之间可以用逗号分隔，这点和shell命令可以用逗号分隔是一样的。</p>
<p>这里提前说一点使用s替换命令需要小心的地方，不要忘记结尾的斜杠，如果你遇到下面的错误说明你犯了这个错误:</p>
<pre class="brush: bash; gutter: true">$ sed -e &#039;s/ MA/, Massachusetts&#039; list
sed: -e expression #1, char 21: unterminated `s&#039; command</pre>
<p>假如这时，我只想输出替换命令影响的行，而不想输出文件的所有内容，需要怎么办？这个时候可以利用替换命令s的p选项，它会将替换后的内容打印到标准输出。但是仅仅这样是不够的，否则输出的结果并非我们所想要的：</p>
<pre class="brush: bash; gutter: true">$ sed -e &#039;s/ MA/, Massachusetts/p&#039; list
John Daggett, 341 King Road, Plymouth, Massachusetts
John Daggett, 341 King Road, Plymouth, Massachusetts
Alice Ford, 22 East Broadway, Richmond VA
Orville Thomas, 11345 Oak Bridge Road, Tulsa OK
Terry Kalkas, 402 Lans Road, Beaver Falls PA
Eric Adams, 20 Post Road, Sudbury, Massachusetts
Eric Adams, 20 Post Road, Sudbury, Massachusetts
Hubert Sims, 328A Brook Road, Roanoke VA
Amy Wilde, 334 Bayshore Pkwy, Mountain View CA
Sal Carpenter, 73 6th Street, Boston, Massachusetts
Sal Carpenter, 73 6th Street, Boston, Massachusetts</pre>
<p>从上面可以看出，文件的所有行都被打印到标准输出，并且被替换命令修改的行输出了两遍。造成这个问题的原因是，sed命令应用完所有指定之后默认会将当前行打印输出，所以就有了上面的结果。解决方法是在使用sed命令是指定-n参数，该参数会抑制sed默认的输出：</p>
<pre class="brush: bash; gutter: true">$ sed -n &#039;s/ MA/, Massachusetts/p&#039; list
John Daggett, 341 King Road, Plymouth, Massachusetts
Eric Adams, 20 Post Road, Sudbury, Massachusetts
Sal Carpenter, 73 6th Street, Boston, Massachusetts</pre>
<p id="aeaoofnhgocdbnbeljkmbjdmhbcokfdb-mousedown"><strong>正则表达式</strong></p>
<p>书中的第3章的内容都是介绍正则表达式，这是因为sed的很多地方都需要用到正则表达式，比如操作指令的pattern部分以及替换命令中用到的匹配部分。关于正则这部分可以看<a href="http://www.infoq.com/cn/news/2011/07/regular-expressions-6-POSIX">Linux/Unix工具与正则表达式的POSIX规范</a>，文章中已经讲得非常清楚了，我自已在忘记一些内容的时候也是去查阅这篇文章。</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p id="aeaoofnhgocdbnbeljkmbjdmhbcokfdb-mousedown"><span style="color: #ff0000;"><strong>Sed&amp;awk笔记之sed篇：模式空间与地址匹配</strong></span></p>
<p><strong>模式空间</strong></p>
<p>在上一篇Sed&amp;awk笔记之sed篇：简单介绍中，我们曾经介绍过简单的sed处理流程，这里首先回顾下：</p>
<p>1.读入新的一行内容到缓存空间；</p>
<p>2.从指定的操作指令中取出第一条指令，判断是否匹配pattern；</p>
<p>3.如果不匹配，则忽略后续的编辑命令，回到第2步继续取出下一条指令；</p>
<p>4.如果匹配，则针对缓存的行执行后续的编辑命令；完成后，回到第2步继续取出下一条指令；</p>
<p>5.当所有指令都应用之后，输出缓存行的内容；回到第1步继续读入下一行内容；</p>
<p>6.当所有行都处理完之后，结束；</p>
<p>由此可见，sed并非是将一个编辑命令分别应用到每一行，然后再取下一个编辑命令。恰恰相反，sed是以行的方式来处理的。另外一方面，每一行都是被读入到一块缓存空间，该空间名为模式空间(pattern space)，这是一个很重要的概念，在后文中会多次被提及。因此sed操作的都是最初行的拷贝，同时后续的编辑命令都是应用到前面的命令编辑后输出的结果，所以编辑命令之间的顺序就显得格外重要。</p>
<p><strong>简单例子</strong></p>
<p>让我们来看一个非常简单的例子，将一段文本中的pig替换成cow，并且将cow替换成horse：</p>
<pre class="brush: bash; gutter: true">$ sed &#039;s/pig/cow/;s/cow/hores/&#039; input</pre>
<p>初看起来好像没有问题，但是实际上是错误的，原因是第一个替换命令将所有的pig都替换成cow，紧接着的替换命令是基于前一个结果处理的，将所有的cow都替换成horse，造成的结果是全部的pig/cow都被替换成了horse，这样违背了我们的初衷。</p>
<p>在这种情况下，只需要调换下两个编辑命令的顺序：</p>
<pre class="brush: bash; gutter: true">$ sed &#039;s/cow/hores/;s/pig/cow/&#039; input</pre>
<p><strong>模式空间的转换</strong></p>
<p>sed只会缓存一行的内容在模式空间，这样的好处是sed可以处理大文件而不会有任何问题，不像一些编辑器因为要一次性载入文件的一大块内容到缓存中而导致内存不足。下面用一个简单的例子来讲解模式空间的转换过程，如下图所示：</p>
<p style="text-align: center;"><a href="http://jbcdn2.b0.upaiyun.com/2012/12/sed_pattern_space2.png" rel="lightbox[31026]" title="《sed &amp; awk》笔记之 sed 篇"><img class="aligncenter  wp-image-31042" title="《sed &amp; awk》笔记之 sed 篇" src="http://jbcdn2.b0.upaiyun.com/2012/12/sed_pattern_space2.png" alt="《sed &amp; awk》笔记之 sed 篇" width="510" height="305" /></a></p>
<p>现在要把一段文本中的Unix System与UNIX System都要统一替换成The UNIX Operating System，因此我们用两句替换命令来完成这个目的：</p>
<pre class="brush: bash; gutter: true">s/Unix /UNIX /
s/UNIX System/UNIX Operating System/</pre>
<p>对应上图，过程如下：</p>
<p>1.首先一行内容The Unix System被读入模式空间；</p>
<p>2.应用第一条替换命令将Unix替换成UNIX；</p>
<p>3.现在模式空间的内容变成The UNIX System；</p>
<p>4.应用第二条替换命令将UNIX System替换成UNIX Operating System；</p>
<p>5.现在模式空间的内容变成The UNIX Operating System；</p>
<p>6.所有编辑命令执行完毕，默认输出模式空间中的行；</p>
<p><strong>地址匹配</strong></p>
<p>地址匹配在sed中是非常重要的一块内容，因为它限制sed的编辑命令到底应用在哪些行上。默认情况下，sed是全局匹配的，即对所有输入行都应用指定的编辑命令，这是因为sed依次读入每一行，每一行都会成为当前行并被处理，所以<code>s/CA/California/g</code>会将所有输入行的CA替换成California。这一点跟vi/vim是不一样的，众所周知，vim的替换命令默认是替换当前行的内容，除非你指定%s才会作全局替换。</p>
<p>可以通过指定地址来限制sed的处理范围，例如只想将替换包含Sebastopol的行：</p>
<pre class="brush: bash; gutter: true">Sebastopol/s/CA/California/</pre>
<p>/Sebastopol/是一个正则表达式匹配包含Sebastopol的行，因此像行“San Francisco, CA”则不会被替换。</p>
<p>sed命令中可以包含0个、1个或者2个地址（地址对），地址可以为正则表达式（如/Sebastopol/），行号或者特殊的行符号（如$表示最后一行）：</p>
<p>● 如果没有指定地址，默认将编辑命令应用到所有行；</p>
<p>●如果指定一个地址，只将编辑命令应用到匹配该地址的行；</p>
<p>●如果指定一个地址对（addr1,addr2)，则将编辑命令应用到地址对中的所有行（包括起始和结束）；</p>
<p>●如果地址后面有一个感叹号（！），则将编辑命令应用到不匹配该地址的所有行；</p>
<p>为了方便理解上述内容，我们以删除命令（d）为例，默认不指定地址将会删除所有行：</p>
<pre class="brush: bash; gutter: true">$ sed &#039;d&#039; list</pre>
<p>指定地址则删除匹配的行，如删除第一行：</p>
<pre class="brush: bash; gutter: true">$ sed &#039;1d&#039; list 
Alice Ford, 22 East Broadway, Richmond VA
Orville Thomas, 11345 Oak Bridge Road, Tulsa OK
Terry Kalkas, 402 Lans Road, Beaver Falls PA
Eric Adams, 20 Post Road, Sudbury MA
Hubert Sims, 328A Brook Road, Roanoke VA
Amy Wilde, 334 Bayshore Pkwy, Mountain View CA
Sal Carpenter, 73 6th Street, Boston MA</pre>
<p>或者删除最后一行，$符号在这里表示最后一行，这点要下正则表达式中的含义区别开来：</p>
<pre class="brush: bash; gutter: true">$ sed &#039;$d&#039; list 
John Daggett, 341 King Road, Plymouth MA
Alice Ford, 22 East Broadway, Richmond VA
Orville Thomas, 11345 Oak Bridge Road, Tulsa OK
Terry Kalkas, 402 Lans Road, Beaver Falls PA
Eric Adams, 20 Post Road, Sudbury MA
Hubert Sims, 328A Brook Road, Roanoke VA
Amy Wilde, 334 Bayshore Pkwy, Mountain View CA</pre>
<p>这里通过指定行号删除，行号是sed命令内部维护的一个计数变量，该变量只有一个，并且在多个文件输入的情况下也不会被重置。</p>
<p>前面都是以行号来指定地址，也可以通过正则表达式来指定地址，如删除包含MA的行：</p>
<pre class="brush: bash; gutter: true">$ sed &#039;/MA/d&#039; list 
Alice Ford, 22 East Broadway, Richmond VA
Orville Thomas, 11345 Oak Bridge Road, Tulsa OK
Terry Kalkas, 402 Lans Road, Beaver Falls PA
Hubert Sims, 328A Brook Road, Roanoke VA
Amy Wilde, 334 Bayshore Pkwy, Mountain View CA</pre>
<p>通过指定地址对可以删除该范围内的所有行，例如删除第3行到最后一行：</p>
<pre class="brush: bash; gutter: true">$ sed &#039;2,$d&#039; list 
John Daggett, 341 King Road, Plymouth MA</pre>
<p>使用正则匹配，删除从包含Alice的行开始到包含Hubert的行结束的所有行：</p>
<pre class="brush: bash; gutter: true">$ sed &#039;/Alice/,/Hubert/d&#039; list 
John Daggett, 341 King Road, Plymouth MA
Amy Wilde, 334 Bayshore Pkwy, Mountain View CA
Sal Carpenter, 73 6th Street, Boston MA</pre>
<p>当然，行号和地址对是可以混用的：</p>
<pre class="brush: bash; gutter: true">$ sed &#039;2,/Hubert/d&#039; list 
John Daggett, 341 King Road, Plymouth MA
Amy Wilde, 334 Bayshore Pkwy, Mountain View CA
Sal Carpenter, 73 6th Street, Boston MA</pre>
<p>如果在地址后面指定感叹号（！），则会将命令应用到不匹配该地址的行：</p>
<pre class="brush: bash; gutter: true">$ sed &#039;2,/Hubert/!d&#039; list 
Alice Ford, 22 East Broadway, Richmond VA
Orville Thomas, 11345 Oak Bridge Road, Tulsa OK
Terry Kalkas, 402 Lans Road, Beaver Falls PA
Eric Adams, 20 Post Road, Sudbury MA
Hubert Sims, 328A Brook Road, Roanoke VA</pre>
<p>以上介绍的都是最基本的地址匹配形式，GNU Sed（也是我们用的sed版本）基于此添加了几个扩展的形式，具体可以看man手册，或者可以看我之前写的<a title="Sed命令地址匹配问题总结" href="http://kodango.me/sed-address-matching-summary">Sed命令地址匹配问题总结</a>一文。</p>
<p>上面说的内容都是对匹配的地址执行单个命令，如果要执行多个编辑命令要怎么办？sed中可以用{}来组合命令，就好比编程语言中的语句块，例如：</p>
<pre class="brush: bash; gutter: true">$ sed -n &#039;1,4{s/ MA/, Massachusetts/;s/ PA/, Pennsylvania/;p}&#039; list 
John Daggett, 341 King Road, Plymouth, Massachusetts
Alice Ford, 22 East Broadway, Richmond VA
Orville Thomas, 11345 Oak Bridge Road, Tulsa OK
Terry Kalkas, 402 Lans Road, Beaver Falls, Pennsylvania</pre>
<p>&nbsp;</p>
<p><span style="color: #ff0000;"><strong>Sed&amp;awk笔记之sed篇：基础命令</strong></span></p>
<p id="">在开始之前，首先回顾上一篇的重点内容：地址匹配。上一篇中介绍过，地址可以指定0个，1个或者2个。地址的形式可以为斜杠分隔的正则表达式(例如/test/)，行号（例如3,5)或者特殊符号(例如$)。如果没有指定地址，说明sed应用的编辑命令是全局的；如果是1个地址，编辑命令只是应用到匹配的那一行；如果是一对地址，编辑命令则应用到该地址对匹配的行范围。关于地址匹配的内容具体可以看<a title="Sed命令地址匹配问题总结" href="http://kodango.me/sed-address-matching-summary">Sed命令地址匹配问题总结</a>。</p>
<p>书中说，对于sed编辑命令的语法有两种约定，分别是</p>
<pre class="brush: bash; gutter: true">[address]command               # 第一种
[line-address]command          # 第二种</pre>
<p>第一种[address]是指可以为任意地址包括地址对，第二种[line-address]是指只能为单个地址。文中指出，例如i（后方插入命令）、a（前方追加命令）以及=（打印行号命令）等都属于第二种命令，但是实际上我在ArchLinux上测试，指定地址对也没有报错。不过为了兼容性，建议按作者所说的做。</p>
<p>以下是要介绍的全部基础命令：</p>
<table>
<thead>
<tr>
<th>名称</th>
<th>命令</th>
<th>语法</th>
<th>说明</th>
</tr>
</thead>
<tbody>
<tr>
<td>替换</td>
<td>s</td>
<td>[address]s/pattern/replacement/flags</td>
<td>替换匹配的内容</td>
</tr>
<tr>
<td>删除</td>
<td>d</td>
<td>[address]d</td>
<td>删除匹配的行</td>
</tr>
<tr>
<td>插入</td>
<td>i</td>
<td>[line-address]i\text</td>
<td>在匹配行的前方插入文本</td>
</tr>
<tr>
<td>追加</td>
<td>a</td>
<td>[line-address]a\text</td>
<td>在匹配行的后方插入文本</td>
</tr>
<tr>
<td>行替换</td>
<td>c</td>
<td>[address]c\text</td>
<td>将匹配的行替换成文本text</td>
</tr>
<tr>
<td>打印行</td>
<td>p</td>
<td>[address]p</td>
<td>打印在模式空间中的行</td>
</tr>
<tr>
<td>打印行号</td>
<td>=</td>
<td>[address]=</td>
<td>打印当前行行号</td>
</tr>
<tr>
<td>打印行</td>
<td>l</td>
<td>[address]l</td>
<td>打印在模式空间中的行，同时显示控制字符</td>
</tr>
<tr>
<td>转换字符</td>
<td>y</td>
<td>[address]y/SET1/SET2/</td>
<td>将SET1中出现的字符替换成SET2中对应位置的字符</td>
</tr>
<tr>
<td>读取下一行</td>
<td>n</td>
<td>[address]n</td>
<td>将下一行的内容读取到模式空间</td>
</tr>
<tr>
<td>读文件</td>
<td>r</td>
<td>[line-address]r file</td>
<td>将指定的文件读取到匹配行之后</td>
</tr>
<tr>
<td>写文件</td>
<td>w</td>
<td>[address]w file</td>
<td>将匹配地址的所有行输出到指定的文件中</td>
</tr>
<tr>
<td>退出</td>
<td>q</td>
<td>[line-address]q</td>
<td>读取到匹配的行之后即退出</td>
</tr>
</tbody>
</table>
<h3>替换命令: s</h3>
<p>替换命令的语法是：</p>
<pre class="brush: bash; gutter: true">[address]s/pattern/replacement/flags</pre>
<p>其中[address]是指地址，pattern是替换命令的匹配表达式，replacement则是对应的替换内容，flags是指替换的标志位，它可以包含以下一个或者多个值：</p>
<p>● n: 一个数字（取值范围1-512），表明仅替换前n个被pattern匹配的内容；</p>
<p>● g: 表示全局替换，替换所有被pattern匹配的内容；</p>
<p>● p: 仅当行被pattern匹配时，打印模式空间的内容；</p>
<p>● w file：仅当行被pattern匹配时，将模式空间的内容输出到文件file中；</p>
<p>如果flags为空，则默认替换第一次匹配：</p>
<pre class="brush: bash; gutter: true">$ echo &quot;column1 column2 column3 column4&quot; | sed &#039;s/ /;/&#039;
column1;column2 column3 column4</pre>
<p>如果flags中包含g，则表示全局匹配：</p>
<pre class="brush: bash; gutter: true">$ echo &quot;column1 column2 column3 column4&quot; | sed &#039;s/ /;/g&#039;
column1;column2;column3;column4</pre>
<p>如果flags中明确指定替换第n次的匹配，例如n=2：</p>
<pre class="brush: bash; gutter: true">$ echo &quot;column1 column2 column3 column4&quot; | sed &#039;s/ /;/2&#039;
column1 column2;column3 column4</pre>
<p>当替换命令的pattern与地址部分是一样的时候，比如<code>/regexp/s/regexp/replacement/</code>可以省略替换命令中的pattern部分，这在单个编辑命令的情况下没多大用处，但是在组合命令的场景下还是能省不少功夫的，例如下面是摘自文中的一个段落：</p>
<p><span style="color: #888888;">The substitute command is applied to the lines matching the address. If no </span><span style="color: #888888;">address is specified, it is applied to all lines that match the pattern, a </span><span style="color: #888888;">regular expression. If a regular expression is supplied as an address, and no </span><span style="color: #888888;">pattern is specified, the substitute command matches what is matched by the </span><span style="color: #888888;">address. This can be useful when the substitute command is one of multiple </span><span style="color: #888888;">commands applied at the same address. For an example, see the section &#8220;Checking </span><span style="color: #888888;">Out Reference Pages&#8221; later in this chapter.</span></p>
<p>现在要在substitute command后面增加(&#8220;s&#8221;)，同时在被修改的行前面增加+号，以下是使用的sed命令：</p>
<pre class="brush: bash; gutter: true">$ sed &#039;/substitute command/{s//&amp;(&quot;s&quot;)/;s/^/+ /}&#039; paragraph.txt</pre>
<p>这里我们用到了组合命令，并且地址匹配的部分和第一个替换命令的匹配部分是一样的，所以后者我们省略了，在replacement部分用到了&amp;这个元字符，它代表之前匹配的内容，这点我们在后面介绍。执行后的结果为：</p>
<pre class="brush: bash; gutter: true">+ The substitute command(&quot;s&quot;) is applied to the lines matching the address. If no
address is specified, it is applied to all lines that match the pattern, a
regular expression. If a regular expression is supplied as an address, and no
+ pattern is specified, the substitute command(&quot;s&quot;) matches what is matched by the
+ address.  This can be useful when the substitute command(&quot;s&quot;) is one of multiple
commands applied at the same address. For an example, see the section &quot;Checking
Out Reference Pages&quot; later in this chapter.</pre>
<p>替换命令的一个技巧是中间的分隔符是可以更改的（这个技巧我们在<a title="简洁的Bash编程技巧" href="http://blog.jobbole.com/29973/" target="_blank">简洁的Bash编程技巧续篇 &#8211; 5. 你知道sed的这个特性吗？</a>中也曾经介绍过），这个技巧在有些地方非常有用，比如路径替换，下面是采用默认的分隔符和使用感叹号作为分隔符的对比：</p>
<pre class="brush: bash; gutter: true">$ find /usr/local -maxdepth 2 -type d | sed &#039;s//usr/local/man//usr/share/man/&#039;
$ find /usr/local -maxdepth 2 -type d | sed &#039;s!/usr/local/man!/usr/share/man!&#039;</pre>
<p>替换命令中还有一个很重要的部分——replacement（替换内容），即将匹配的部分替换成replacement。在replacemnt部分中也有几个特殊的元字符，它们分别是：</p>
<p>● &amp;: 被pattern匹配的内容；</p>
<p>●\num: 被pattern匹配的第num个分组（正则表达式中的概念，<span class='MathJax_Preview'>\(..\)</span><script type='math/tex'>..</script>括起来的部分称为分组；</p>
<p>●\: 转义符号，用来转义&amp;，\, 回车等符号</p>
<p>&amp;元字符我们已经在上面的例子中介绍过，这里举个例子介绍下\num的用法。在Linux系统中，默认都开了几个tty可以让你切换（ctrl+alt+1, ctrl+alt+2 &#8230;)，例如CentOS 6.0+的系统默认是6个：</p>
<pre class="brush: bash; gutter: true">[root@localhost ~]# grep -B 1 ACTIVE_CONSOLES /etc/sysconfig/init 
# What ttys should gettys be started on?
ACTIVE_CONSOLES=/dev/tty[1-6]</pre>
<p>可以通过修改上面的配置来减少开机启动的时候创建的tty个数，比如我们只想要2个：</p>
<pre class="brush: bash; gutter: true">[root@localhost ~]# sed -r -i &#039;s!(ACTIVE_CONSOLES=/dev/tty[1-)6]!12]!&#039; /etc/sysconfig/init 
ACTIVE_CONSOLES=/dev/tty[1-2]</pre>
<p>其中-i参数表示直接修改原文件，-r参数是指使用扩展的正则表达式（ERE），扩展的正则表达式中分组的括号不需要用反斜杠转义&#8221;(ACTIVE_CONSOLES=/dev/tty<p style='text-align:center;'><span class='MathJax_Preview'>\[1-)"???[???????????????????????????????\1?????????????1????????????????</p>
<p><strong>????: d</strong></p>
<p>?????????</p>
<pre class="brush: bash; gutter: true">[address]d</pre>
<p>?????????????????<code>1,3d</code>???1?3???????????????????????????????????????????????????????????????</p>
<pre class="brush: bash; gutter: true">$ sed '2,${d;=}' list
John Daggett, 341 King Road, Plymouth MA</pre>
<p>??????????2???????????????(=)???????????????</p>
<p><strong>???/???/?????: i/a/c</strong></p>
<p>?????????????</p>
<pre class="brush: bash; gutter: true"># Append ??
[line-address]a\
text
# Insert ??
line-address]i\
text
# Change ??? 
[address]c\
text</pre>
<p>?????????????c)???????????????????????<strong>??</strong>?ArchLinux?????????????????????sed???<strong>GNU sed version 4.2.1</strong>?</p>
<p>?????????????????text?????????????????????text????????????????????text???text?????????????????????????????????????????????????-n????????????????</p>
<p>???????????????????????????????list?</p>
<pre class="brush: bash; gutter: true">$ cat list
John Daggett, 341 King Road, Plymouth MA
Alice Ford, 22 East Broadway, Richmond VA
Orville Thomas, 11345 Oak Bridge Road, Tulsa OK
Terry Kalkas, 402 Lans Road, Beaver Falls PA
Eric Adams, 20 Post Road, Sudbury MA
Hubert Sims, 328A Brook Road, Roanoke VA
Amy Wilde, 334 Bayshore Pkwy, Mountain View CA
Sal Carpenter, 73 6th Street, Boston MA</pre>
<p>????????2?????'------'?</p>
<pre class="brush: bash; gutter: true">$ sed '2a
--------------------------------------
' list
John Daggett, 341 King Road, Plymouth MA
Alice Ford, 22 East Broadway, Richmond VA
--------------------------------------
Orville Thomas, 11345 Oak Bridge Road, Tulsa OK
Terry Kalkas, 402 Lans Road, Beaver Falls PA
Eric Adams, 20 Post Road, Sudbury MA
Hubert Sims, 328A Brook Road, Roanoke VA
Amy Wilde, 334 Bayshore Pkwy, Mountain View CA
Sal Carpenter, 73 6th Street, Boston MA</pre>
<p>??????3??????</p>
<pre class="brush: bash; gutter: true">$ sed '3i
--------------------------------------
' list
John Daggett, 341 King Road, Plymouth MA
Alice Ford, 22 East Broadway, Richmond VA
--------------------------------------
Orville Thomas, 11345 Oak Bridge Road, Tulsa OK
Terry Kalkas, 402 Lans Road, Beaver Falls PA
Eric Adams, 20 Post Road, Sudbury MA
Hubert Sims, 328A Brook Road, Roanoke VA
Amy Wilde, 334 Bayshore Pkwy, Mountain View CA
Sal Carpenter, 73 6th Street, Boston MA</pre>
<p>???????????????????????????????????????????</p>
<pre class="brush: bash; gutter: true">$ sed -n '2a
--------------------------------------
' list
--------------------------------------</pre>
<p>??-n???????????????????????????????????????????</p>
<p>?????????2??????????????'----'?</p>
<pre class="brush: bash; gutter: true">$ sed '2,$c
--------------------------------------
' list
John Daggett, 341 King Road, Plymouth MA
--------------------------------------</pre>
<p><strong>????: p/l/=</strong></p>
<p>?????????????p????????(l?=)?p?????????????????????????</p>
<p>??????????</p>
<pre class="brush: bash; gutter: true">[address]p
[address]=
[address]l</pre>
<p>p??????????????????list???????</p>
<pre class="brush: bash; gutter: true">$ sed -n '1p' list
John Daggett, 341 King Road, Plymouth MA</pre>
<p>l????p??????????????????vim?list????????</p>
<pre class="brush: bash; gutter: true">$ echo "column1 column2 column3^M" | sed -n 'l'
column1tcolumn2tcolumn3r$</pre>
<p>=?????????????</p>
<pre class="brush: bash; gutter: true">$ sed '=' list
1
John Daggett, 341 King Road, Plymouth MA
2
Alice Ford, 22 East Broadway, Richmond VA
3
Orville Thomas, 11345 Oak Bridge Road, Tulsa OK
4
Terry Kalkas, 402 Lans Road, Beaver Falls PA
5
Eric Adams, 20 Post Road, Sudbury MA
6
Hubert Sims, 328A Brook Road, Roanoke VA
7
Amy Wilde, 334 Bayshore Pkwy, Mountain View CA
8
Sal Carpenter, 73 6th Street, Boston MA</pre>
<p><strong>????: y</strong></p>
<p>?????????</p>
<pre class="brush: bash; gutter: true">[address]y/SET1/SET2/</pre>
<p>?????????????SET1?????????SET2???????????<code>1,3y/abc/xyz/</code>??1?3?????a???x?b???y?c???z????????????????????tr???????????y???????????????????????</p>
<pre class="brush: bash; gutter: true">$ echo "hello, world" | sed 'y/abcdefghijklmnopqrstuvwxyz/ABCDEFGHIJKLMNOPQRSTUVWXYZ/'
HELLO, WORLD</pre>
<p>??tr?????????</p>
<pre class="brush: bash; gutter: true">$ echo "hello, world" | tr a-z A-Z � � � � � � � � � � � � � � � � � � � � � � � � � �
HELLO, WORLD</pre>
<p><strong>??????: n</strong></p>
<p>???????????</p>
<pre class="brush: bash; gutter: true">[address]n</pre>
<p>n????????????????????????????????????????????????????????????n?????d??????sed??????</p>
<p>????????????n??????????????</p>
<pre class="brush: bash; gutter: true">$ cat text
.H1 "On Egypt"

Napoleon, pointing to the Pyramids, said to his troops:
"Soldiers, forty centuries have their eyes upon you."</pre>
<p>????.H1????????</p>
<pre class="brush: bash; gutter: true">$ sed '/.H1/{n;/^$/d}' text
.H1 "On Egypt"
Napoleon, pointing to the Pyramids, said to his troops:
"Soldiers, forty centuries have their eyes upon you."</pre>
<p><strong>??????: r/w</strong></p>
<p>???????????</p>
<pre class="brush: bash; gutter: true">[line-address]r file
[address]w file</pre>
<p>??????????????????????????????????(a)?????????????????????????text?</p>
<pre class="brush: bash; gutter: true">$ cat text
For service, contact any of the following companies:
[Company-list]
Thank you.</pre>
<p>??????????????????company.list?</p>
<pre class="brush: bash; gutter: true">$ cat company.list 
Allied
Mayflower
United</pre>
<p>??????company.list??????[Company-list]???</p>
<pre class="brush: bash; gutter: true">$ sed '/^[Company-list]/r company.list' text
For service, contact any of the following companies:
[Company-list]
Allied
Mayflower
United
Thank you.</pre>
<p>?????????????????[Company-list]??????????[Company-list]????</p>
<pre class="brush: bash; gutter: true">$ sed '/^[Company-list]/r company.list;/^[Company-list]/d;' text
For service, contact any of the following companies:
[Company-list]
Thank you.</pre>
<p>????????????[Company-list]???company.list????????????????</p>
<p>??????????????????r?????????????????<code>company.list;/^\]</span><script type='math/tex;  mode=display'>1-)"???[???????????????????????????????\1?????????????1????????????????</p>
<p><strong>????: d</strong></p>
<p>?????????</p>
<pre class="brush: bash; gutter: true">[address]d</pre>
<p>?????????????????<code>1,3d</code>???1?3???????????????????????????????????????????????????????????????</p>
<pre class="brush: bash; gutter: true">$ sed '2,${d;=}' list
John Daggett, 341 King Road, Plymouth MA</pre>
<p>??????????2???????????????(=)???????????????</p>
<p><strong>???/???/?????: i/a/c</strong></p>
<p>?????????????</p>
<pre class="brush: bash; gutter: true"># Append ??
[line-address]a\
text
# Insert ??
line-address]i\
text
# Change ??? 
[address]c\
text</pre>
<p>?????????????c)???????????????????????<strong>??</strong>?ArchLinux?????????????????????sed???<strong>GNU sed version 4.2.1</strong>?</p>
<p>?????????????????text?????????????????????text????????????????????text???text?????????????????????????????????????????????????-n????????????????</p>
<p>???????????????????????????????list?</p>
<pre class="brush: bash; gutter: true">$ cat list
John Daggett, 341 King Road, Plymouth MA
Alice Ford, 22 East Broadway, Richmond VA
Orville Thomas, 11345 Oak Bridge Road, Tulsa OK
Terry Kalkas, 402 Lans Road, Beaver Falls PA
Eric Adams, 20 Post Road, Sudbury MA
Hubert Sims, 328A Brook Road, Roanoke VA
Amy Wilde, 334 Bayshore Pkwy, Mountain View CA
Sal Carpenter, 73 6th Street, Boston MA</pre>
<p>????????2?????'------'?</p>
<pre class="brush: bash; gutter: true">$ sed '2a
--------------------------------------
' list
John Daggett, 341 King Road, Plymouth MA
Alice Ford, 22 East Broadway, Richmond VA
--------------------------------------
Orville Thomas, 11345 Oak Bridge Road, Tulsa OK
Terry Kalkas, 402 Lans Road, Beaver Falls PA
Eric Adams, 20 Post Road, Sudbury MA
Hubert Sims, 328A Brook Road, Roanoke VA
Amy Wilde, 334 Bayshore Pkwy, Mountain View CA
Sal Carpenter, 73 6th Street, Boston MA</pre>
<p>??????3??????</p>
<pre class="brush: bash; gutter: true">$ sed '3i
--------------------------------------
' list
John Daggett, 341 King Road, Plymouth MA
Alice Ford, 22 East Broadway, Richmond VA
--------------------------------------
Orville Thomas, 11345 Oak Bridge Road, Tulsa OK
Terry Kalkas, 402 Lans Road, Beaver Falls PA
Eric Adams, 20 Post Road, Sudbury MA
Hubert Sims, 328A Brook Road, Roanoke VA
Amy Wilde, 334 Bayshore Pkwy, Mountain View CA
Sal Carpenter, 73 6th Street, Boston MA</pre>
<p>???????????????????????????????????????????</p>
<pre class="brush: bash; gutter: true">$ sed -n '2a
--------------------------------------
' list
--------------------------------------</pre>
<p>??-n???????????????????????????????????????????</p>
<p>?????????2??????????????'----'?</p>
<pre class="brush: bash; gutter: true">$ sed '2,$c
--------------------------------------
' list
John Daggett, 341 King Road, Plymouth MA
--------------------------------------</pre>
<p><strong>????: p/l/=</strong></p>
<p>?????????????p????????(l?=)?p?????????????????????????</p>
<p>??????????</p>
<pre class="brush: bash; gutter: true">[address]p
[address]=
[address]l</pre>
<p>p??????????????????list???????</p>
<pre class="brush: bash; gutter: true">$ sed -n '1p' list
John Daggett, 341 King Road, Plymouth MA</pre>
<p>l????p??????????????????vim?list????????</p>
<pre class="brush: bash; gutter: true">$ echo "column1 column2 column3^M" | sed -n 'l'
column1tcolumn2tcolumn3r$</pre>
<p>=?????????????</p>
<pre class="brush: bash; gutter: true">$ sed '=' list
1
John Daggett, 341 King Road, Plymouth MA
2
Alice Ford, 22 East Broadway, Richmond VA
3
Orville Thomas, 11345 Oak Bridge Road, Tulsa OK
4
Terry Kalkas, 402 Lans Road, Beaver Falls PA
5
Eric Adams, 20 Post Road, Sudbury MA
6
Hubert Sims, 328A Brook Road, Roanoke VA
7
Amy Wilde, 334 Bayshore Pkwy, Mountain View CA
8
Sal Carpenter, 73 6th Street, Boston MA</pre>
<p><strong>????: y</strong></p>
<p>?????????</p>
<pre class="brush: bash; gutter: true">[address]y/SET1/SET2/</pre>
<p>?????????????SET1?????????SET2???????????<code>1,3y/abc/xyz/</code>??1?3?????a???x?b???y?c???z????????????????????tr???????????y???????????????????????</p>
<pre class="brush: bash; gutter: true">$ echo "hello, world" | sed 'y/abcdefghijklmnopqrstuvwxyz/ABCDEFGHIJKLMNOPQRSTUVWXYZ/'
HELLO, WORLD</pre>
<p>??tr?????????</p>
<pre class="brush: bash; gutter: true">$ echo "hello, world" | tr a-z A-Z � � � � � � � � � � � � � � � � � � � � � � � � � �
HELLO, WORLD</pre>
<p><strong>??????: n</strong></p>
<p>???????????</p>
<pre class="brush: bash; gutter: true">[address]n</pre>
<p>n????????????????????????????????????????????????????????????n?????d??????sed??????</p>
<p>????????????n??????????????</p>
<pre class="brush: bash; gutter: true">$ cat text
.H1 "On Egypt"

Napoleon, pointing to the Pyramids, said to his troops:
"Soldiers, forty centuries have their eyes upon you."</pre>
<p>????.H1????????</p>
<pre class="brush: bash; gutter: true">$ sed '/.H1/{n;/^$/d}' text
.H1 "On Egypt"
Napoleon, pointing to the Pyramids, said to his troops:
"Soldiers, forty centuries have their eyes upon you."</pre>
<p><strong>??????: r/w</strong></p>
<p>???????????</p>
<pre class="brush: bash; gutter: true">[line-address]r file
[address]w file</pre>
<p>??????????????????????????????????(a)?????????????????????????text?</p>
<pre class="brush: bash; gutter: true">$ cat text
For service, contact any of the following companies:
[Company-list]
Thank you.</pre>
<p>??????????????????company.list?</p>
<pre class="brush: bash; gutter: true">$ cat company.list 
Allied
Mayflower
United</pre>
<p>??????company.list??????[Company-list]???</p>
<pre class="brush: bash; gutter: true">$ sed '/^[Company-list]/r company.list' text
For service, contact any of the following companies:
[Company-list]
Allied
Mayflower
United
Thank you.</pre>
<p>?????????????????[Company-list]??????????[Company-list]????</p>
<pre class="brush: bash; gutter: true">$ sed '/^[Company-list]/r company.list;/^[Company-list]/d;' text
For service, contact any of the following companies:
[Company-list]
Thank you.</pre>
<p>????????????[Company-list]???company.list????????????????</p>
<p>??????????????????r?????????????????<code>company.list;/^</script></p>!Company-list]/d;</code>，然后读取命令的时候发现该文件不存在，但是sed命令读取不存在的文件是不会报错的。所以什么事都没干成。</p>
<p>我们用-e选项将两个命令分开就正常了：</p>
<pre class="brush: bash; gutter: true">$ sed -e &#039;/^[Company-list]/r company.list&#039; -e &#039;/^[Company-list]/d;&#039; text
For service, contact any of the following companies:
Allied
Mayflower
United
Thank you.</pre>
<p>写命令将匹配地址的所有行输出到指定的文件中。假设有一个文件内容如下，前半部分是人名，后半部分是区域名称：</p>
<pre class="brush: bash; gutter: true">$ cat text 
Adams, Henrietta Northeast
Banks, Freda South
Dennis, Jim Midwest
Garvey, Bill Northeast
Jeffries, Jane West
Madison, Sylvia Midwest
Sommes, Tom South</pre>
<p>现在我们要将不同区域的人名字写到不同的文件中：</p>
<pre class="brush: bash; gutter: true">$ sed &#039;/Northeast$/w region.northeast
&gt; /South$/w region.south
&gt; /Midwest$/w region.midwest
&gt; /West$/w region.west&#039; text
Adams, Henrietta Northeast
Banks, Freda South
Dennis, Jim Midwest
Garvey, Bill Northeast
Jeffries, Jane West
Madison, Sylvia Midwest
Sommes, Tom South</pre>
<p>查看输出的文件：</p>
<pre class="brush: bash; gutter: true">$ cat region.
region.midwest    region.northeast  region.south      region.west 
$ cat region.midwest     
Dennis, Jim Midwest
Madison, Sylvia Midwest</pre>
<p>我们也可以试试将所有的w命令放到一起用分号分隔，发现会出下面的错误，它把w空格后面的部分当作文件名，这样就验证了上面的猜测：</p>
<pre class="brush: bash; gutter: true">$ sed &#039;/Northeast$/w region.northeast;/south$/w region.south;&#039; text
sed: couldn&#039;t open file region.northeast;/south$/w region.south;: No such file or directory</pre>
<p><strong>退出命令: q</strong></p>
<p>退出命令的语法是</p>
<pre class="brush: bash; gutter: true">[line-address]q</pre>
<p>当sed读取到匹配的行之后即退出，不会再读入新的行，并且将当前模式空间的内容输出到屏幕。例如打印前3行内容：</p>
<pre class="brush: bash; gutter: true">$ sed &#039;3q&#039; list
John Daggett, 341 King Road, Plymouth MA
Alice Ford, 22 East Broadway, Richmond VA
Orville Thomas, 11345 Oak Bridge Road, Tulsa OK</pre>
<p>打印前3行也可以用p命令：</p>
<pre class="brush: bash; gutter: true">$ sed -n &#039;3p&#039; list
John Daggett, 341 King Road, Plymouth MA
Alice Ford, 22 East Broadway, Richmond VA
Orville Thomas, 11345 Oak Bridge Road, Tulsa OK</pre>
<p>但是对于大文件来说，前者比后者效率更高，因为前者读取到第N行之后就退出了。后者虽然打印了前N行，但是后续的行还是要继续读入，只不会不作处理。</p>
<p>到此为止，sed基础命令的部分就介绍完了。</p>
<p>下面是我整理的一份思维导图（附.xmind文件<a href="http://kodango.aliapp.com/upload/sed%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4.xmind">下载地址</a>）：</p>
<p style="text-align: center;"><a href="http://jbcdn2.b0.upaiyun.com/2012/12/sed_basic_command_副本.jpg" rel="lightbox[31026]" title="《sed &amp; awk》笔记之 sed 篇"><img class="aligncenter  wp-image-31062" title="《sed &amp; awk》笔记之 sed 篇" src="http://jbcdn2.b0.upaiyun.com/2012/12/sed_basic_command_副本.jpg" alt="《sed &amp; awk》笔记之 sed 篇" width="576" height="256" /></a></p>
<p><span style="color: #ff0000;"><strong>Sed&amp;awk笔记之sed篇：高级命令（一）</strong></span></p>
<p>在上一篇中介绍的基础命令都是面向行的，一般情况下，这种处理并没有什么问题，但是当匹配的内容是错开在两行时就会有问题，最明显的例子就是某些英文单词会被分成两行。</p>
<p>幸运地是，sed允许将多行内容读取到模式空间，这样你就可以匹配跨越多行的内容。本篇笔记主要介绍这些命令，它们能够创建多行模式空间并且处理之。其中，N/D/P这三个多行命令分别对应于小写的n/d/p命令，后者我们在上一篇已经介绍。它们的功能是类似的，区别在于命令影响的内容不同。例如D命令与d命令同样是删除模式空间的内容，只不过d命令会删除模式空间中所有的内容，而D命令仅会删除模式空间中的第一行。</p>
<p><strong>读下一行：N</strong></p>
<p>N命令将下一行的内容读取到当前模式空间，但是下n命令不一样的地方是N命令并没有直接输出当前模式空间中的行，而是把下一行追加到当前模式空间，两行之间用回车符\n连接，如下图所示：</p>
<p><a href="http://jbcdn2.b0.upaiyun.com/2012/12/N_read_newline.png" rel="lightbox[31026]" title="《sed &amp; awk》笔记之 sed 篇"><img class="aligncenter size-full wp-image-31051" title="《sed &amp; awk》笔记之 sed 篇" src="http://jbcdn2.b0.upaiyun.com/2012/12/N_read_newline.png" alt="《sed &amp; awk》笔记之 sed 篇" width="392" height="361" /></a></p>
<p>模式空间包含多行之后，正则表达式的^/$符号的意思就变了，^是匹配模式空间的最开始而非行首，$是匹配模式空间的最后位置而非行尾。</p>
<p>书中给的第一例子，是替换以下文本中的"Owner and Operator Guide"为"Installation Guide"，正如我们在本篇开头说的Owner and Operator Guide跨越在两行：</p>
<pre class="brush: bash; gutter: true">Consult Section 3.1 in the Owner and Operator
Guide for a description of the tape drives
available on your system.</pre>
<p>我们用N命令试下手：</p>
<pre class="brush: bash; gutter: true">$ sed &#039;/Operator$/{N;s/Owner and OperatornGuide/Installation Guide/}&#039; text
Consult Section 3.1 in the Installation Guide for a description of the tape drives
available on your system.</pre>
<p>不过这个例子有两个局限：</p>
<p>● 我们知道Owner and Operator Guide分割的位置；</p>
<p>● 执行替换命令后，前后两行拼接在一起，导致这行过长；</p>
<p>第二点，可以这样解决：</p>
<pre class="brush: bash; gutter: true">$ sed &#039;/Operator$/{N;s/Owner and OperatornGuide /Installation Guiden/}&#039; text
Consult Section 3.1 in the Installation Guide
for a description of the tape drives
available on your system.</pre>
<p>现在去掉第1个前提，我们引入更加复杂的测试文本，这段文本中Owner and Operator Guide有位于一行的，也有跨越多行的情况：</p>
<pre class="brush: bash; gutter: true">$ cat text
Consult Section 3.1 in the Owner and Operator
Guide for a description of the tape drives
available on your system.

Look in the Owner and Operator Guide shipped with your system.

Two manuals are provided including the Owner and
Operator Guide and the User Guide.

The Owner and Operator Guide is shipped with your system.</pre>
<p>相应地修改下之前执行的命令，如下所示：</p>
<pre class="brush: bash; gutter: true">$ sed &#039;s/Owner and Operator Guide/Installation Guide/
/Owner/{
N
s/ *n/ /
s/Owner and Operator Guide */Installation Guide
/
}&#039; text
Consult Section 3.1 in the Installation Guide
for a description of the tape drives
available on your system.

Look in the Installation Guide shipped with your system.

Two manuals are provided including the Installation Guide
and the User Guide.

The Installation Guide is shipped with your system.</pre>
<p>这里我们首先将在单行出现的Owner and Operator Guide替换为Installation Guide，然后再寻找匹配Owner的行，匹配后读取下一行的内容到模式空间，并且将中间的换行符替换成空格，最后再替换Owner and Operator Guide。</p>
<p>解释起来很简单，但是中间还是有些门道的。比如你可能觉得这里最前面的s/Owner and Operator Guide/Installation Guide/命令是多余的，假设你删除这一句：</p>
<pre class="brush: bash; gutter: true">$ sed &#039;/Owner/{
&gt; N
&gt; s/ *n/ /
&gt; s/Owner and Operator Guide */Installation Guide
&gt; /
&gt; }&#039; text
Consult Section 3.1 in the Installation Guide
for a description of the tape drives
available on your system.

Look in the Installation Guide
shipped with your system. 
Two manuals are provided including the Installation Guide
and the User Guide.

The Owner and Operator Guide is shipped with your system.</pre>
<p>最明显的问题是最后一行没有被替换，原因是当最后一行的被读入到模式空间后，匹配Owner，执行N命令读入下一行，但是因为当前已经是最后一行，所以N读取替换，告诉sed可以退出了，sed也不会继续执行接下来的替换命令（<strong>注：</strong>书中说最后一行不会被打印输出，我这边测试是会输出的）。所以这里需要在使用N的时候加一个判断，即当前行为最后一行时，不读取下一行的内容：$!N，这一点在很多场合都是有用的，更改后重新执行：</p>
<pre class="brush: bash; gutter: true">$ sed &#039;/Owner/{
&gt; $!N
&gt; s/ *n/ /
&gt; s/Owner and Operator Guide */Installation Guide
&gt; /
&gt; }&#039; text
Consult Section 3.1 in the Installation Guide
for a description of the tape drives
available on your system.

Look in the Installation Guide
shipped with your system. 
Two manuals are provided including the Installation Guide
and the User Guide.

The Installation Guide
is shipped with your system.</pre>
<p>上面只是为了用例子说明N的用法，可能解决方案未必是最好的。</p>
<p><strong>删除行：D</strong></p>
<p>该命令删除模式空间中第一行的内容，而它对应的小d命令删除模式空间的所有内容。D不会导致读入新行，相反它会回到最初的编辑命令，重要应用在模式空间剩余的内容上。</p>
<p>后面半句开始比较难以理解，用书中的一个例子来解释下。现在我们有一个文本文件，内容如下所示，行之间有空行：</p>
<pre class="brush: bash; gutter: true">$ cat text
This line is followed by 1 blank line.

This line is followed by 2 blank lines.

This line is followed by 3 blank lines.

This line is followed by 4 blank lines.

This is the end.</pre>
<p>现在我们要删除多余的空行，将多个空行缩减成一行。假如我们使用d命令来删除，很简单的逻辑：</p>
<pre class="brush: bash; gutter: true">$ sed &#039;/^$/{N;/^n$/d}&#039; text
This line is followed by 1 blank line.

This line is followed by 2 blank lines.
This line is followed by 3 blank lines.

This line is followed by 4 blank lines.
This is the end.</pre>
<p>我们会发现一个奇怪的结果，奇数个数的相连空行已经被合并成一行，但是偶数个数的却全部被删除了。造成这样的原因需要重新翻译下上面的命令，当匹配一个空行是，将下一行也读取到模式空间，然后若下一行也是空行，则模式空间中的内容应该是\n，因此匹配^\n$，从而执行d命令会将模式空间中的内容清空，结果就是相连的两个空行都被删除。这样就可以理解为什么相连奇数个空行的情况下是正常的，而偶数个数就有问题了。</p>
<p>这种情况下，我们就应该用D命令来处理，这样做就得到预期的结果了：</p>
<pre class="brush: bash; gutter: true">$ sed &#039;/^$/{N;/^n$/D}&#039; text
This line is followed by 1 blank line.

This line is followed by 2 blank lines.

This line is followed by 3 blank lines.

This line is followed by 4 blank lines.

This is the end.</pre>
<p>D命令只会删除模式空间的第一行，而且删除后会重新在模式空间的内容上执行编辑命令，类似形成一个循环，前提是相连的都是空行。当匹配一个空行时，N读取下一行内容，此时匹配^\n$导致模式空间中的第一行被删除。现在模式空间中的内容是空的，重新执行编辑命令，此时匹配/^$/。继续读取下一行，当下一行依然为空行时，重复之前的动作，否则输出当前模式空间的内容。造成的结果是连续多个空行，只有最后一个空行是保留输出的，其余的都被删除了。这样的结果才是我们最初希望得到的。</p>
<p><strong>打印行：P</strong></p>
<p>P命令与p命令一样是打印模式空间的内容，不同的是前者仅打印模式空间的第一行内容，而后者是打印所有的内容。因为编辑命令全部执行完之后，sed默认会输出模式空间的内容，所以一般情况下，p和P命令都是与-n选项一起使用的。但是有一种情况是例外的，即编辑命令的执行流程被改变的情况，例如N，D等。很多情况下，P命令都是用在N命令之后，D命令之前的。这三个命令合起来，可以形成一人输入输出的循环，并且每次只打印一行：读入一行后，N继续读下一行，P命令打印第一行，D命令删除第一行，执行流程回到最开始重复该过程。</p>
<pre class="brush: bash; gutter: true">$ echo -e &quot;line1nline2nline3&quot; | sed &#039;$!N;P;D&#039;</pre>
<p>不过多行命令用起来要格外小心，你要用自己的大脑去演算一遍执行的过程，要不然很容易出错，比如：</p>
<pre class="brush: bash; gutter: true">$ echo -e &quot;line1nline2nline3&quot; | sed -n &#039;N;1P&#039;</pre>
<p>你可能期望打印第一行的内容，事实上并没有输出。原因是当N继续读入第二行后，当前行号已经是2了，我们在<a title="Sed&amp;awk笔记之sed篇：模式空间与地址匹配" href="http://kodango.me/sed-and-awk-notes-part-2">第二篇</a>笔记中曾经说过，行号只是sed在内部维护的一个计数变量而已，每当读入新的一行，行号就加一：</p>
<pre class="brush: bash; gutter: true">$ echo -e &quot;line1nline2nline3&quot; | sed -n &#039;$!N;=&#039;
2
3</pre>
<p>我们依然用替换Unix System为Unix Operating System作为例子，介绍N/P/D三个命令是如何配合使用的。</p>
<p>示例文本如下所示，为了举例方便，这段文本仅有三行内容，刚好可以演示一个循环的处理过程：</p>
<pre class="brush: bash; gutter: true">$ cat text
The UNIX
System and UNIX
...</pre>
<p>执行的命令：</p>
<pre class="brush: bash; gutter: true">$ sed &#039;/UNIX$/{
&gt; N
&gt; s/nSystem/ Operating &amp;/
&gt; P
&gt; D
&gt; }&#039; text
The UNIX Operating 
System and UNIX
...</pre>
<p style="text-align: center;">执行过程如下图所示：<br />
<a href="http://jbcdn2.b0.upaiyun.com/2012/12/ndp_loop.png" rel="lightbox[31026]" title="《sed &amp; awk》笔记之 sed 篇"><img class="aligncenter  wp-image-31052" title="《sed &amp; awk》笔记之 sed 篇" src="http://jbcdn2.b0.upaiyun.com/2012/12/ndp_loop.png" alt="《sed &amp; awk》笔记之 sed 篇" width="586" height="254" /></a></p>
<p id="aeaoofnhgocdbnbeljkmbjdmhbcokfdb-mousedown">以上三个命令的用法与之前介绍的基础命令是截然不同的，有些同学可能都没有接触过。在下一篇中，我会介绍更多高级的命令，同时为引入一个新的概念：保持空间（Hold Space)。</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p id="aeaoofnhgocdbnbeljkmbjdmhbcokfdb-mousedown"><span style="color: #ff0000;"><strong>Sed&amp;awk笔记之sed篇：高级命令（二）</strong></span></p>
<p>上一篇中介绍了N/D/P三个命令，它们可以形成多行的模式空间，在一点程度上弥补了单行模式空间的不足，我们用sed编辑文本的能力又进了一步。模式空间是sed内部维护的一个缓存空间，它存放着读入的一行或者多行内容。但是模式空间的一个限制是无法保存模式空间中被处理的行，因此sed又引入了另外一个缓存空间——模式空间（Hold Space）。</p>
<p><strong>保持空间</strong></p>
<p>保持空间用于保存模式空间的内容，模式空间的内容可以复制到保持空间，同样地保持空间的内容可以复制回模式空间。sed提供了几组命令用来完成复制的工作，其它命令无法匹配也不能修改模式空间的内容。</p>
<p>操作保持空间的命令如下所示：</p>
<table class="aligncenter">
<thead>
<tr>
<th>名称</th>
<th>命令</th>
<th>说明</th>
</tr>
</thead>
<tbody>
<tr>
<td>保存（Hold)</td>
<td>h/H</td>
<td>将模式空间的内容复制或者追加到保持空间</td>
</tr>
<tr>
<td>取回（Get）</td>
<td>g/G</td>
<td>将保持空间的内容复制或者追加到模式空间</td>
</tr>
<tr>
<td>交换（Exchange）</td>
<td>x</td>
<td>交换模式空间和保持空间的内容</td>
</tr>
</tbody>
</table>
<p>这几组命令提供了保存、取回以及交换三个动作，交换命令比较容易理解，保存命令和取回命令都有大写和小写两种形式，这两种形式的区别是小写的是将会覆盖目的空间的内容，而大写的是将内容追加到目的空间，追加的内容和原有的内容是以\n分隔。</p>
<p><strong>基本使用</strong></p>
<p>我们随便试试这几个命令，假设有如下测试文本:</p>
<pre class="brush: bash; gutter: true">$ cat text
1
2
11
22
111
222</pre>
<p>1. 首先，仅使用h/H或者g/G命令：</p>
<p>使用h命令：</p>
<pre class="brush: bash; gutter: true">$ sed &#039;h&#039; text
1
2
11
22
111
222</pre>
<p>使用G命令：</p>
<pre class="brush: bash; gutter: true">$ sed &#039;G&#039; text
1

2

11

22

111

222</pre>
<p>前者返回的结果正常，因为复制到保持空间的内容并没有取回；后者每一行的后面都多了一个空行，原因是每行都会从保持空间取回一行，追加（大写的G）到模式空间的内容之后，以\n分隔。</p>
<p>2. 使用x命令交换空间</p>
<pre class="brush: bash; gutter: true">$ sed &#039;x&#039; text

1
2
11
22
111</pre>
<p>命令执行后，发现前面多了一个空行并且最后一行不见了。我在前面一直强调sed命令用好，要有用大脑回顾命令执行过程的能力：</p>
<p>* 当读入第一行的时候，模式空间中的内容是第一行的内容，而保持空间是空的，这个时候交换两个空间，导致模式空间为空，保持空间为第一行的内容，因此输出为空行；</p>
<p>* 当读入下一行之后，模式空间为第2行的内容，保持空间为第一行的内容，交换后输出第1行的内容；</p>
<p>* 依次读入每一行，输出上一行的内容；</p>
<p>* 直到最后一行被读入到模式空间，交换后输出倒数第二行的内容，而最后一行的内容并没有输出，此时命令执行结束。</p>
<p><strong>深入使用</strong></p>
<p>上面的例子简单地介绍了保持空间命令的基本使用方法，这些命令单个使用可能效果不大，但是组合起来的效果是非常好的。</p>
<p>1.第一个例子： 使用逗号拼接行</p>
<pre class="brush: bash; gutter: true">$ sed &#039;H;$!d;${x;s/^n//;s/n/,/g}&#039; text
1,11,2,11,22,111,222</pre>
<p>上面的命令执行过程是这样的，使用H将每一行都追加到保持空间，这里利用d命令打断常规的命令执行流程，让sed继续读入新的一行，直接到将最后一行都放到保持空间。这个时候使用x命令将保持空间的内容交换到模式空间，模式空间的内容现在是这样的：<code>\n1\n11\n2\n11\n22\n111\n222</code>。替换的步骤分成两个，首先去掉首个回车符，然后把剩余的回车符替换成逗号。</p>
<p>其实上面的过程可以包装成一个常用的函数：</p>
<pre class="brush: bash; gutter: true">$ function join_lines()
&gt; {
&gt;    sed &#039;H;$!d;${x;s/^n//;s/n/,/g}&#039; $1
&gt; }
$ join_lines text
1,11,2,11,22,111,222</pre>
<p>进一步，我们可以让分隔符可以通过参数设置，另外一方面删除文件名的参数，而改成常见的过滤器命令形式（即通过管道传递输入）：</p>
<pre class="brush: bash; gutter: true">$ function join_lines()
&gt; {
&gt;     local delim=${1:-,}
&gt;     sed &#039;H;$!d;${x;s/^n//;s/n/&#039;$delim&#039;/g}&#039;
&gt; }
$ cat text | join_lines &#039;;&#039;
1;11;2;11;22;111;222</pre>
<p>但是如果我们要用&amp;作为符号，就会出现问题:</p>
<pre class="brush: bash; gutter: true">$ cat text | join_lines &#039;&amp;&#039;
1
11
2
11
22
111
222</pre>
<p>上面并没有&amp;作为分隔符，这是因为&amp;是元字符，表示匹配的部分，这里刚好是回车符\n。因此我们需要对分隔符进行转义：</p>
<pre class="brush: bash; gutter: true">$ function join_lines()
&gt; {
&gt;      local delim=${1:-,}
&gt;      sed &#039;H;$!d;${x;s/^n//;s/n/&#039;$delim&#039;/g}&#039;
&gt; } 
$ cat text | join_lines &#039;&amp;&#039;
1&amp;11&amp;2&amp;11&amp;22&amp;111&amp;222</pre>
<p>2.第二个例子：将语句中的特定单词转换成大写</p>
<p>现在有这样的文本，有许多类似这样的find the Match statement语句，其中Match是语句的名称，但是这个单词的大小写不统一，有些地方是小写的，有些地方是首字符大写，现在我们要做的是把这个单词统一转换成大写。</p>
<p>容易联想到的是Sed&amp;awk笔记之sed篇：基础命令中介绍的y命令，利用y命令确实可以做到小写转换成大写，转换的命令是这样的：</p>
<pre class="brush: bash; gutter: true">y/abcdefghijklmnopqrstuvwxyz/ABCDEFGHIJKLMNOPQRSTUVWXYZ/</pre>
<p>但是y命令是会把模式空间的所有内容都转换，这样不能满足我们的需求。但是我们可以利用保持空间保存当前行，然后处理模式空间中的内容：</p>
<pre class="brush: bash; gutter: true">/the .* statement/{
h
s/.*the (.*) statement.*/1/
y/abcdefghijklmnopqrstuvwxyz/ABCDEFGHIJKLMNOPQRSTUVWXYZ/
G
s/(.*)n(.*the ).*( statement.*)/213/
}</pre>
<p>老规矩一条一条过上面的命令，为了方便说明，每个命令解释后我都会给出当前模式空间和保持空间的内容。</p>
<p>首先找到需要处理的行（假设当前行为find the Match statement）。</p>
<pre class="brush: bash; gutter: true">Pattern space: find the Match statement</pre>
<p>将当前行保存到保持空间：</p>
<pre class="brush: bash; gutter: true">Pattern space: find the Match statement
Hold space: find the Match statement</pre>
<p>然后利用替换命令获取需要处理的单词：</p>
<pre class="brush: bash; gutter: true">Pattern space: Match
Hold space: find the Match statement</pre>
<p>然后通过转换命令将其转换成大写：</p>
<pre class="brush: bash; gutter: true">Pattern space: MATCH
Hold space: find the Match statement</pre>
<p>现在再利用G命令将保持空间的内容追加到模式空间最后：</p>
<pre class="brush: bash; gutter: true">Pattern space: MATCH\nfind the Match statement
Hold space: find the Match statement</pre>
<p>最后再次利用替换命令处理下：</p>
<pre class="brush: bash; gutter: true">Pattern space: find the MATCH statement
Hold space: find the Match statement</pre>
<p><strong>流程控制</strong></p>
<p>一般情况下，sed是将编辑命令从上到下依次应用到读入的行上，但是像d/n/D/N命令都能够在一定程度上改变默认的执行流程，甚至利用N/D/P三个命令可以形成一个强大的循环处理流程。除此之外，其实sed还提供了分支命令(b)和测试(test)两个命令来控制流程，这两个命令可以跳转到指定的标签（label）位置继续执行命令。标签是以冒号开头的标记，如下例中的:top标签：</p>
<pre class="brush: bash; gutter: true">:top
command1
command2
/pattern/b top
command3</pre>
<p>当执行到<code>/pattern/b top</code>时，如果匹配pattern，则跳转到:top标签所在的位置，继续执行下一个命令command1。</p>
<p>如果没有指定标签，则将控制转移到脚本的结尾处。也许这只是一个默认的行为，但是有时候如果用得好也是非常有用的，例如:</p>
<pre class="brush: bash; gutter: true">/pattern/b
command 1
command 2
command 3</pre>
<p>当执行到<code>/pattern/b</code>时，如果匹配pattern，则跳转到最后。这种情况下匹配pattern的行可以避开执行后续的命令，被排除在外。</p>
<p>下一个例子中，我们利用分支命令的跳转效果达到类似if语句的效果：</p>
<pre class="brush: bash; gutter: true">command1
/pattern/b end
command2
:end
command3</pre>
<p>当执行到<code>/pattern/b end</code>时，如果匹配pattern，则跳转到:end标签所在的位置，跳过command2而不执行。</p>
<p>进一步地，利用两个分支命令可以达到if..else的分支效果：</p>
<pre class="brush: bash; gutter: true">command1
/pattern/b dothree
command2
b
:dothree
command3</pre>
<p>这个例子中，当执行到<code>/pattern/b dothree</code>时，若匹配pattern则中转到:dothree标签，此时执行command3；若不匹配，则执行command2，并且跳转到最后。</p>
<p>上面的例子都是用到了分支命令，分支命令的跳转是无条件的。而与之相对的是测试命令，测试命令的跳转是有条件的，当且仅当当前行发生成功的替换时才跳转。</p>
<p>为了说明测试命令的用法，我们用它来实现前文定义过的join_lines函数：</p>
<pre class="brush: bash; gutter: true">$ sed &#039;:a;$!N;s/n/,/;ta&#039; text
1,11,2,11,22,111,222</pre>
<p>在最前面我们添加了一个标签:a，然后在再最后面利用测试命令跳转到该标签。可能，你会觉得这里也可以用分支命令，但是事实上分支命令会导致死循环，因为在它里他没有结束的条件。</p>
<p>但是测试命令就不同了，这一点直到最后才体现出来。当最后一行被N命令读入之后，回车替换成逗号，此时ta继续跳转到最开头，因为所有行都已经被读入，所以$!不匹配，同时模式空间中的回车已经全部被替换成逗号，所以替换也不会发生。之前我们说过，当<strong>且仅当当前行发生成功的替换时测试命令才跳转。</strong>所以此时跳转不会发生，退出sed命令。</p>
<p>我们可以利用<a href="http://aurelio.net/sedsed/">sedsed</a>这个工具来验证上面的过程，sedsed可以用来调试sed脚本。</p>
<pre class="brush: bash; gutter: true">$ ./sedsed -d -e &#039;:a;$!N;s/n/,/;ta&#039; text
PATT:1$
HOLD:$
COMM::a
COMM:$ !N
PATT:1n11$
HOLD:$
COMM:s/n/,/
PATT:1,11$
HOLD:$
COMM:t a
COMM:$ !N
PATT:1,11n2$
HOLD:$
...
...
COMM:$ !N
PATT:1,11,2,11,22,111,222n1111$
HOLD:$
COMM:s/n/,/
PATT:1,11,2,11,22,111,222,1111$
HOLD:$
COMM:t a
COMM:$ !N
PATT:1,11,2,11,22,111,222,1111$
HOLD:$
COMM:s/n/,/
PATT:1,11,2,11,22,111,222,1111$
HOLD:$
COMM:t a
PATT:1,11,2,11,22,111,222,1111$
HOLD:$
1,11,2,11,22,111,222,1111</pre>
<p>看第27行替换命令发生的时候，此时模式空间的内容为<code>PATT:1,11,2,11,22,111,222,1111$</code>，因此替换失败，ta命令不会发生跳转，脚本执行退出。</p>
<p>而如果在这里把测试命令换成分支命令，整个执行过程就会陷入死循环了:</p>
<pre class="brush: bash; gutter: true">COMM:b a
COMM:$ !N
PATT:1,11,2,11,22,111,222,1111$
HOLD:$
COMM:s/n/,/
PATT:1,11,2,11,22,111,222,1111$
HOLD:$
COMM:b a
COMM:$ !N
PATT:1,11,2,11,22,111,222,1111$</pre>
<p><strong>高级命令总结</strong></p>
<p id="aeaoofnhgocdbnbeljkmbjdmhbcokfdb-mousedown">到此为止，所有高级命令的用法就已经介绍完了。最后一段内容由于时间的关系，写得比较仓促。高级命令的用法比起基础命令相对复杂一点，而且容易出错，需要十分小心，如果不确定可以用上面介绍的<a href="http://aurelio.net/sedsed/">sedsed</a>工具来调式下，而且便于加深各种命令行为的理解。</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p><span style="color: #ff0000;"><strong>Sed&amp;awk笔记之sed篇：实战</strong></span></p>
<p id="">相信大家肯定用过grep这个命令，它可以找出匹配某个正则表达式的行，例如查看包含"the word"的行：</p>
<pre class="brush: bash; gutter: true">$ grep &quot;the word&quot; filename</pre>
<p>但是grep是针对单行作匹配的，所以如果一个短句跨越了两行就无法匹配。这就给我们出了一个题目，如何用sed模仿grep的行为，并且支持跨行短句的匹配呢？</p>
<p>当单词仅出现在一行时很简单，用普通的正则就可以匹配，这里我们用到分支命令，当匹配时则跳转到最后：</p>
<pre class="brush: bash; gutter: true">/the word/b</pre>
<p>当单词跨越两行，容易想到用N命令将下一行读取到模式空间再做处理。例如我们同样要查找短句"the word"，现在的文本是这样的：</p>
<pre class="brush: bash; gutter: true">$ cat text
we want to find the phrase the
word, but it appears across two lines.</pre>
<p>当用N玲读入下一行时，模式空间的内容如下所示：</p>
<pre class="brush: bash; gutter: true">Pattern space: we want to find the phrase the\nword, but it appears across two lines.</pre>
<p>因此，需要将回车符号删除，然后再作匹配。</p>
<pre class="brush: bash; gutter: true">$!N
s/ *\n/ /
/the word/b</pre>
<p>可是这里会有一个问题，如果短句恰好在读入的第二行的话，虽然匹配，但是会打印出模式空间中的所有内容，这样不符合grep的行为，只显示包含短句的那一行。所以这里要再加一个处理，将模式空间的第一行内容删除，再在第二行中匹配。但是在此之前，首先要保存模式空间的内容，否则可没有后悔药可吃。</p>
<p>h命令可以将模式空间的内容保存到保持空间，然后利用替换命令将模式空间的第一行内容清除，然后再作匹配处理：</p>
<pre class="brush: bash; gutter: true">$!N
h
s/.*\n//
/the word/b</pre>
<p>如果不匹配，则短句可能确实是跨越两行，这时候我们首先用g命令将之前保存的内容从保持空间取回到模式空间，替换掉回车后再进行匹配：</p>
<pre class="brush: bash; gutter: true">g
s/ *\n/ /
/the word/b</pre>
<p>这里如果匹配则直接跳转到最后，并且输出的内容是替换后的内容。</p>
<p>但是我们要输出的是匹配的原文，而原文现在在保持空间还有一份拷贝，因此当匹配时，需要将原文从保持空间取回：</p>
<pre class="brush: bash; gutter: true">g
s/ *\n/ /
/the word/{
g
b
}</pre>
<p>同样地，我们要考虑不匹配的情况，不匹配的时候也要将会原文从保持空间取回，并且将模式空间的第一行删除，继续处理下一行：</p>
<pre class="brush: bash; gutter: true">g
s/ *\n/ /
/the word/{
g
b
}
g
D</pre>
<p>将所有的sed脚本合在一起，假设我们将以下内容保存到phrase.sed文件中：</p>
<pre class="brush: bash; gutter: true">/the word/b
$!N
h
s/.*\n//
/the word/b
g
s/ *\n//
/the word/{
g
b
}
g
D</pre>
<p>接下来，我们用一段文本来测试下以上的脚本是否正确：</p>
<pre class="brush: bash; gutter: true">$ cat text
We will use phrase.sed to print any line which contains
the word. Or if the word appears across two lines, like
below:

It will print this line, because the
word appears across two lines.

You can run sed -f phrase.sed text to test this.</pre>
<p>执行命令如下所示：</p>
<pre class="brush: bash; gutter: true">$ sed -f phrase.sed text
the word. Or if the word appears across two lines, like
It will print this line, because the
word appears across two lines.</pre>
<p>上面的命令中的"the word"其实可以是一个变量，这样我们就可以将这个功能写成一个脚本或者函数，用在更多地方：</p>
<pre class="brush: bash; gutter: true">$ cat phrase.sh 
#! /bin/sh
# phrase -- search for words across lines
# $1 = search string; remaining args = filenames

search=$1

for file in ${@:2}; do
    sed &quot;/$search/b
    $!N
    h
    s/.*n//
    /$search/b
    g
    s/ *n/ /
    /$search/{
    g
    b
    }
    g
    D&quot; $file
done

$ chmod +x phrase.sh
$ ./phrase.sh &#039;the word&#039; text
the word. Or if the word appears across two lines, like
It will print this line, because the
word appears across two lines.</pre>
<p id="aeaoofnhgocdbnbeljkmbjdmhbcokfdb-mousedown">这只是一个开头，或者你也可以在此基础上扩展更多的功能。sed的命令从单个看来并没是很复杂，但是要用得好就有点难度了，所以需要多多实践，多多思考，这一点跟正则表达式的学习是一样的。如果觉得没有现成的学习环境，<a title="sed1line 笔记" href="http://kodango.me/sed1line-notes">sed1line</a>是一个不错的开始。</p>
<p>&nbsp;</p>
<p>&nbsp;</p>

        