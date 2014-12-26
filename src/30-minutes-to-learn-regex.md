<p>推荐几个正则表达式编辑器</p>
<ul>
<li>Debuggex ：<a href="https://www.debuggex.com/" target="_blank">https://www.debuggex.com/</a></li>
<li>PyRegex：<a href="http://www.pyregex.com/" target="_blank">http://www.pyregex.com/</a></li>
<li>Regexper：<a href="http://www.regexper.com/" target="_blank">http://www.regexper.com/</a></li>
</ul>
<p>正则表达式是一种查找以及字符串替换操作。正则表达式在文本编辑器中广泛使用，比如正则表达式被用于：</p>
<ul>
<li><span style="font-family: Simsun; font-size: medium;">检查文本中是否含有指定的特征词</span></li>
<li><span style="font-family: Simsun; font-size: medium;">找出文中匹配特征词的位置</span></li>
<li><span style="font-family: Simsun; font-size: medium;">从文本中提取信息，比如：字符串的子串</span></li>
<li><span style="font-family: Simsun; font-size: medium;">修改文本</span></li>
</ul>
<p><span style="font-family: Simsun; font-size: medium;">与文本编辑器相似，几乎所有的高级编程语言都支持正则表达式。在这样的语境下，“文本”也就是一个字符串，可以执行的操作都是类似的。一些编程语言（比如Perl，JavaScript）会检查正则表达式的语法。</span></p>
<p><strong><span style="font-family: Simsun; font-size: medium;">正则表达式是什么？</span></strong></p>
<p><span style="font-family: Simsun; font-size: medium;">正则表达式只是一个字符串。没有长度限制，但是，这样的正则表达式长度往往较短。如下所示是一些正则表达式的例子：</span></p>
<ul>
<li><code><span style="font-family: Simsun; font-size: medium;">I had a \S+ day today</span></code></li>
<li><code><span style="font-family: Simsun; font-size: medium;">[A-Za-z0-9\-_]{3,16}</span></code></li>
<li><code><span style="font-family: Simsun; font-size: medium;">\d\d\d\d-\d\d-\d\d</span></code></li>
<li><code><span style="font-family: Simsun; font-size: medium;">v(\d+)(\.\d+)*</span></code></li>
<li><code><span style="font-family: Simsun; font-size: medium;">TotalMessages="(.*?)"</span></code></li>
<li><code><span style="font-family: Simsun; font-size: medium;">&lt;[^&lt;&gt;]&gt;</span></code></li>
</ul>
<div><span style="font-family: Simsun; font-size: medium;">这些字符串实际上都是微型计算机程序。正则表达式的语法，实际上是一种轻量级、简洁、适用于特定领域的编程语言。记住这一点，那么你就很容易理解下面的事情：</span></div>
<div>
<ul>
<li><span style="font-family: Simsun; font-size: medium;">每一个正则表达式，都可以分解为一个指令序列，比如“先找到这样的字符，再找到那样的字符，再从中找到一个字符。。。”<br />
</span></li>
<li><span style="font-family: Simsun; font-size: medium;">每一个正则表达式都有输入（文本）和输出（匹配规则的输出，有时是修改后的文本）</span></li>
<li><span style="font-family: Simsun; font-size: medium;">正则表达式有可能出现语法错误——不是所有的字符串都是正则表达式</span></li>
<li><span style="font-family: Simsun; font-size: medium;">正则表达式语法很有个性，也可以说很恐怖</span></li>
<li><span style="font-family: Simsun; font-size: medium;">有时可以通过编译，使得正则表达式执行更快</span></li>
</ul>
<p><span style="font-family: Simsun; font-size: medium;">在实现中，正则表达式还有其他的特点。本文将重点讨论正则表达式的核心语法，在几乎所有的正则表达式中都可以见到这些规则。</span></p>
<p><span style="font-family: Simsun; font-size: medium;">特别提示：正则表达式与文件通配语法无关，比如 *.xml</span></p>
<h2><span style="font-family: Simsun; font-size: medium;">正则表达式的基础语法</span></h2>
<h3><span style="font-family: Simsun;">字符</span><span style="font-family: Simsun; font-size: medium;"> </span></h3>
<p><span style="font-family: Simsun; font-size: medium;">正则表达式中包含了一系列的字符，这些字符只能匹配它们本身。有一些被称为“元字符”的特殊字符，可以匹配一些特殊规则。</span></p>
<p><span style="font-family: Simsun; font-size: medium;">如下所示的例子中，我用红色标出了元字符。</span></p>
<ul>
<li><code><span style="font-family: Simsun; font-size: medium;">I had a <span style="color: #ff0000;">\S+</span> day today</span></code></li>
<li><span style="color: #ff0000;"><code>[A-Za-z0-9\-_]{3,16}</code></span></li>
<li><code><span style="font-family: Simsun; font-size: medium;"><span style="color: #ff0000;">\d\d\d\d</span>-<span style="color: #ff0000;">\d\d</span>-<span style="color: #ff0000;">\d\d</span></span></code></li>
<li><code><span style="font-family: Simsun; font-size: medium;">v<span style="color: #ff0000;">(\d+)(\.\d+)*</span></span></code></li>
<li><code><span style="font-family: Simsun; font-size: medium;">TotalMessages="<span style="color: #ff0000;">(.*?)</span>"</span></code></li>
<li><code><span style="font-family: Simsun; font-size: medium;">&lt;<span style="color: #ff0000;">[^&lt;&gt;]*</span>&gt;</span></code><span style="font-family: Simsun; font-size: medium;"> </span></li>
</ul>
<p><span style="font-family: Simsun; font-size: medium;">大部分的字符，包括所有的字母和数字字符，是普通字符。也就意味着，它们只能匹配它们自己，如下所示的正则表达式：</span></p>
<p>cat</p>
<p><span style="font-family: Simsun; font-size: medium;">意味着，只能匹配一个字符串，以“c”开头，然后是字符“a”，紧跟着是字符“t”的字符串。</span></p>
<p><span style="font-family: Simsun; font-size: medium;">到目前为止，正则表达式的功能类似于</span></p>
<ul>
<li><span style="font-family: Simsun; font-size: medium;">常规的Find功能</span></li>
<li><span style="font-family: Simsun; font-size: medium;">Java中的 <code>String.indexOf()</code> 函数</span></li>
<li><span style="font-family: Simsun; font-size: medium;">PHP中的 <code>strpos()</code>函数</span></li>
<li><span style="font-family: Simsun; font-size: medium;">等等</span></li>
</ul>
</div>
<div><span style="font-family: Simsun; font-size: medium;">注意：不做特殊说明，正则表达式中是区分大小写的。但是，几乎所有正则表达式的实现，都会提供一个Flag用来控制是否区分大小写。</span></div>
<h3></h3>
<div>
<h3><span style="font-family: Simsun;">点“.”</span></h3>
<p><span style="font-family: Simsun; font-size: medium;">我们第一个要讲解的元字符是“.”。这个符号意味着可以匹配任意一个字符。如下所示的正则表达式：</span></p>
<p>c.t</p>
<p><span style="font-family: Simsun; font-size: medium;">意味着匹配“以c开头,之后是任意一个字符，紧跟着是字母t”的字符串。</span></p>
<p><span style="font-family: Simsun; font-size: medium;">在一段文本中，这样的正则表达式可以用来找出</span><span style="color: #262626; font-family: Simsun; font-size: medium;"><code>cat</code>, <code>cot</code>, <code>czt这样的字符串，甚至可以找出c.t这样的组合，但是不能找到ct或者是coot这样的字符串。</code></span></p>
<p><span style="font-family: Simsun; font-size: medium;">使用反斜杠“\”可以忽略元字符，使得元字符的功能与普通字符一样。所以，正则表达式</span></p>
<p>c\.t</p>
<p><span style="font-family: Simsun; font-size: medium;">表示“找到字母c,然后是一个句号（“.”），紧跟着字母t”</span></p>
<p><span style="font-family: Simsun; font-size: medium;">反斜杠本身也是一个元字符，这意味着反斜杠本身也可以通过相似的方法变回到普通字符的用途。因此，正则表达式</span></p>
<p>c\\t</p>
<p><span style="font-family: Simsun; font-size: medium;">表示匹配“以字符c开头,然后是一个反斜杠，紧跟着是字母t”的字符串。</span></p>
<div>
<p><span style="font-family: Simsun; font-size: medium;">注意！在正则表达式的实现中，.是不能用于匹配换行符的。”换行符“的表示方法在不同实现中也不同。实际编程时，请参考相关文档。在本文中，我认为.是可以匹配任意字符的。实现环境通常会提供一个Flag标志位，来控制这一点。</span></p>
<h2>字符类</h2>
<p>字符类是一组在方括号内的字符，表示可以匹配其中的任何一个字符。</p>
<ul>
<li>正则表达式c[aeiou]t，表示可以匹配的字符串是&#8221;以c开头，接着是aeiou中的任何一个字符，最后以t结尾&#8221;。在文本的实际应用中，这样的正则表达式可以匹配：cat,cet,cit,cot,cut五种字符串。</li>
<li>正则表达式[0123456789]表示匹配任意一个整数。</li>
<li>正则表达式[a]表示匹配单字符a。</li>
</ul>
<p>包含忽略字符的例子</p>
<ul>
<li><p style='text-align:center;'><span class='MathJax_Preview'>\[a\]</span><script type='math/tex;  mode=display'>a</script></p>表示匹配字符串[a]</li>
<li>[<code class='tex2jax_ignore'>\[\]</code>\ab]表示匹配的字符为&#8221;["或者'']&#8221;或者&#8221;a&#8221;,或者&#8221;b&#8221;</li>
<li>[\\<code class='tex2jax_ignore'>\[\]</code>]表示匹配的字符为&#8221;\&#8221;或者 “[”或者"]&#8220;</li>
</ul>
<p>在字符类中，字符的重复和出现顺序并不重要。[dabaaabcc]与[abc]是相同的</p>
<div>
<p>重要提示：字符类中和字符类外的规则有时不同，一些字符在字符类中是元字符，在字符类外是普通字符。一些字符正好相反。还有一些字符在字符类中和字符类外都是元字符，这要视情况而定！</p>
<p>比如，.表示匹配任意一个字符，而[.]表示匹配一个全角句号。这不是一回事！</p>
</div>
<h3>字符类的范围</h3>
<p>在字符集中，你可以通过使用短横线来表示匹配字母或数字的范围。</p>
<ul>
<li>[b-f]与[b,c,d,e,f]相同，都是匹配一个字符&#8221;b&#8221;或&#8221;c&#8221;或&#8221;d&#8221;或&#8221;e&#8221;或&#8221;f&#8221;</li>
<li>[A-Z]与[ABCDEFGHIJKLMNOPQRSTUVWXYZ]相同，都是匹配任意一个大写字母。</li>
<li>[1-9]与[123456789]相同，都是匹配任意一个非零数字。</li>
</ul>
<h4><strong>练习</strong></h4>
<p>使用目前我们已经讲解的正则表达式相关知识，在字典中匹配找到含有最多连续元音的单词，同时找到含有最多连续辅音的单词。</p>
<h4><strong>答案</strong></h4>
<p><code>[aeiou][aeiou][aeiou][aeiou][aeiou][aeiou]</code> 这样的正则表达式，可以匹配连续含有六个元音的单词，比如 <code>euouae</code> 和 <code>euouaes</code>。</p>
<p><code><span style="font-family: Tahoma; font-size: medium;">同样的，恐怖的正则表达式</span></code><code>[bcdfghjklmnpqrstvwxyz][bcdfghjklmnpqrstvwxyz][bcdfghjklmnpqrstvwxyz][bcdfghjklmnpqrstvwxyz][bcdfghjklmnpqrstvwxyz][bcdfghjklmnpqrstvwxyz][bcdfghjklmnpqrstvwxyz][bcdfghjklmnpqrstvwxyz][bcdfghjklmnpqrstvwxyz][bcdfghjklmnpqrstvwxyz]</code><span style="font-family: Tahoma; font-size: medium;"> 可以找到连续含有十个辅音的单词</span><code>sulphhydryls</code><span style="font-family: Tahoma; font-size: medium;">. </span></p>
<p><span style="font-family: Tahoma; font-size: medium;">下文中，我们会讲解，怎样有效缩短这样的正则表达式长度。</span></p>
<p>在字符类之外，短横线没有特殊含义。正则表达式a-z，表示匹配字符串“以a开头，然后是一个短横线，以z结尾”。</p>
<p>范围和单独的字符可能在一个字符类中同时出现：</p>
<ul>
<li>[0-9.,]表明匹配一个数字，或者一个全角句号，或者一个逗号</li>
<li>[0-9a-fA-F]意味着匹配一个十六进制数</li>
<li>[a-zA-Z0-9\-]意味着匹配一个字母、数字或者一个短横线</li>
</ul>
<h4><strong>练习</strong></h4>
<p>使用已经介绍过的正则表达式知识，匹配YYYY-MM-DD格式的日期。</p>
<h4><strong>答案</strong></h4>
<p><code>[0-9][0-9][0-9][0-9]-[0-9][0-9]-[0-9][0-9]</code>.</p>
<p>同样的，下文中，我们会介绍怎样有效减少这样的正则表达式长度。</p>
<p>虽然你可以尝试在正则表达式中使用一些非字母或数字作为范围的最后一个符号，比如abc[!-/]def，但是这并不是在每种实现中都合法。即使这样的语法是合法的，这样的语义也是模糊的。最好不要这样使用。</p>
<p>同时，你必须谨慎选择范围的边界值。即使[A-z]在你使用的实现中，是合法的，也可能会产生无法预料的运行结果。（注意，在z到a之间，是有字符存在的）</p>
<div>
<p><b>注意：</b>范围的字符值代表的是字符而已，并不能代表数值范围，比如[1-31]表示匹配一个数字，是1或者2或者3，而不是匹配一个数值在1到31之间的数。</p>
<div>
<h3>字符类的反义</h3>
<p>你可以在字符类的起始位放一个反义符。</p>
<ul>
<li>[^a]表示匹配任何不是“a”的字符</li>
<li>[^a-zA-Z0-9]表示匹配任何不是字母也不是数字的字符</li>
<li>[\^abc]匹配一个为“^”或者a或者b或者c的字符</li>
<li>[^\^]表示匹配任何不为“^”的字符</li>
</ul>
<div>
<h4><strong>练习</strong></h4>
<p>在字典中，找到一个不满足“在e之前有i，但是没有c”的例子。</p>
<h4><strong>答案</strong></h4>
</div>
</div>
<p>cie和[^c]ei都要可以找到很多这样的例子，比如ancient,science,viel,weigh</p>
<h3></h3>
<h3>转义字符类</h3>
<p>\d这个正则表达式与[0-9]作用相同，都是匹配任何一个数字。（要匹配\d,应该使用正则表达式\\d）</p>
<p>\w与[0-9A-Za-z]相同，都表示匹配一个数字或字母字符</p>
<p>\s意味着匹配一个空字符（空格，制表符，回车或者换行）</p>
<p>另外</p>
<ul>
<li>\D与[^0-9]相同，表示匹配一个非数字字符。</li>
<li>\W与[^0-9A-Za-z]相同，表示匹配一个非数字同时不是字母的字符。</li>
<li>\S表示匹配一个非空字符。</li>
</ul>
<p>这些是你必须掌握的字符。你可能已经注意到了，一个全角句号“.”也是一个字符类，可以匹配任意一个字符。</p>
<p>很多正则表达式的实现中，提供了更多的字符类，或者是标志位在ASCII码的基础上，扩展现有的字符类。</p>
<p>特别提示：统一字符集中包含除了0至9之外的更多数字字符，同样的，也包含更多的空字符和字母字符。实际使用正则表达式时，请仔细查看相关文档。</p>
<div>
<h4><strong>练习</strong></h4>
<p>简化正则表达式 <code>[0-9][0-9][0-9][0-9]-[0-9][0-9]-[0-9][0-9]</code>.</p>
<h4><strong>答案</strong></h4>
<p><code>\d\d\d\d-\d\d-\d\d</code>.</p>
<h3></h3>
<h3>重复</h3>
<p>在字符或字符集之后，你可以使用{ }大括号来表示重复</p>
<ul>
<li>正则表达式a{1}与a意思相同，都表示匹配字母a</li>
<li>a{3}表示匹配字符串“aaa”</li>
<li>a{0}表示匹配空字符串。从这个正则表达式本身来看，它毫无意义。如果你对任何文本执行这样的正则表达式，你可以定位到搜索的起始位置，即使文本为空。</li>
<li>a\{2\}表示匹配字符串“a{2}”</li>
<li>在字符类中，大括号没有特殊含义。[{}]表示匹配一个左边的大括号，或者一个右边的大括号</li>
</ul>
<div>
<h4><strong>练习</strong></h4>
<p>简化下面的正则表达式</p>
<ul>
<li><code>z.......z</code></li>
<li><code>\d\d\d\d-\d\d-\d\d</code></li>
<li><code>[aeiou][aeiou][aeiou][aeiou][aeiou][aeiou]</code></li>
<li><code>[bcdfghjklmnpqrstvwxyz][bcdfghjklmnpqrstvwxyz][bcdfghjklmnpqrstvwxyz][bcdfghjklmnpqrstvwxyz][bcdfghjklmnpqrstvwxyz][bcdfghjklmnpqrstvwxyz][bcdfghjklmnpqrstvwxyz][bcdfghjklmnpqrstvwxyz][bcdfghjklmnpqrstvwxyz][bcdfghjklmnpqrstvwxyz]</code></li>
</ul>
<h4><strong>答案</strong></h4>
<ul>
<li><code>z.{7}z</code></li>
<li><code>\d{4}-\d{2}-\d{2}</code></li>
<li><code>[aeiou]{6}</code></li>
<li><code>[bcdfghjklmnpqrstvwxyz]{10}</code></li>
</ul>
</div>
<div>
<p>注意：重复字符是没有记忆性的，比如[abc]{2}表示先匹配&#8221;a或者b或者c&#8221;，再匹配&#8221;a或者b或者c&#8221;，与匹配”aa或者ab或者ac或者ba或者bb或者bc或者ca或者cb或者cc“一样。[abc]{2}并不能表示匹配”aa或者bb或者cc“</p>
</div>
</div>
<div>
<h3></h3>
<h3>指定重复次数范围</h3>
<p>重复次数是可以指定范围的</p>
<ul>
<li>x{4,4}与x{4}相同</li>
<li>colou{0,1}r表示匹配colour或者color</li>
<li>a{3,5}表示匹配aaaaa或者aaaa或者aaa</li>
</ul>
<p><span style="font-family: Tahoma; font-size: medium;">注意这样的正则表达式会优先匹配最长字符串，比如输入 </span><code>I had an aaaaawful day</code><code><span style="font-family: Tahoma;">会匹配单词aaaaawful中的aaaaa，而不会匹配其中的aaa。</span></code></p>
<p><span style="font-family: Tahoma; font-size: medium;">重复次数是可以有范围的，但是有时候这样的方法也不能找到最佳答案。如果你的输入文本是</span>I had an aaawful daaaaay<span style="font-family: Tahoma; font-size: medium;">那么在第一次匹配时，只能找到aaawful，只有再次执行匹配时才能找到daaaaay中的aaaaa.</span></p>
<p>重复次数的范围可以是开区间</p>
<ul>
<li>a{1，}表示匹配一个或一个以上的连续字符a。依然是匹配最长字符串。当找到第一个a之后，正则表达式会尝试匹配尽量多个的连续字母a。</li>
<li>.{0,}表示匹配任意内容。无论你输入的文本是什么，即使是一个空字符串，这个正则表达式都会成功匹配全文并返回结果。</li>
</ul>
<div>
<h4><strong>练习</strong></h4>
<p>使用正则表达式找到双引号。要求输入字符串可能包含任意个字符。</p>
<p>调整你的正则表达式使得在一对双引号中间不再包含其他的双引号。</p>
<h4><strong>答案</strong></h4>
<p><code> ".{0,}"</code>, 然后 <code>"[^"]{0,}"</code>.</p>
</div>
</div>
<div>
<h3></h3>
<h3>关于重复的转义字符</h3>
<p>？与{0,1}相同，比如，colou?r表示匹配colour或者color</p>
<p>*与{0,}相同。比如，.*表示匹配任意内容</p>
<p>+与{1，}相同。比如,\w+表示匹配一个词。其中&#8221;一个词&#8221;表示由一个或一个以上的字符组成的字符串，比如_var或者AccountName1.</p>
<p>这些是你必须知道的常用转义字符，除此之外还有:</p>
<ul>
<li>\?\*\+ 表示匹配字符串&#8221;?*+&#8221;</li>
<li>[?*+]表示匹配一个问号，或者一个*号，或者一个加号</li>
</ul>
<div>
<h4><strong>练习</strong></h4>
<p>简化下列的正则表达式:</p>
<ul>
<li><code>".{0,}"</code> and <code>"[^"]{0,}"</code></li>
<li><code>x?x?x?</code></li>
<li><code>y*y*</code></li>
<li><code>z+z+z+z+</code></li>
</ul>
<h4><span style="font-family: Tahoma; font-size: medium;"><b>答案</b></span></h4>
<ul>
<li><code>".*"</code> and <code>"[^"]*"</code></li>
<li><code>x{0,3}</code></li>
<li><code>y*</code></li>
<li><code>z{4,</code>}</li>
</ul>
<h4></h4>
<h4><strong>练习</strong></h4>
<p>写出正则表达式，寻找由非字母字符分隔的两个单词。如果是三个呢？六个呢？</p>
<p><code>\w+\W+\w+</code>, <code>\w+\W+\w+\W+\w+</code>, <code>\w+\W+\w+\W+\w+\W+\w+\W+\w+\W+\w+</code>.</p>
<p>下文中，我们将简化这个正则表达式。</p>
</div>
</div>
</div>
</div>
</div>
<h3></h3>
<h3> 非贪婪匹配</h3>
<p>正则表达式 &#8220;.*&#8221; 表示匹配双引号，之后是任意内容，之后再匹配一个双引号。注意，其中匹配任意内容也可以是双引号。通常情况下，这并不是很有用。通过在句尾加上一个问号，可以使得字符串重复不再匹配最长字符。</p>
<ul>
<li>\d{4,5}?表示匹配\d\d\d\d或者\d\d\d\d\d。也就是和\d{4}一样</li>
<li>colou??r与colou{0,1}r相同，表示找到color或者colour。这与colou?r一样。</li>
<li>&#8220;.*?&#8221;表示先匹配一个双引号，然后匹配最少的字符，然后是一个双引号，与上面两个例子不同，这很有用。</li>
</ul>
<h3>选择匹配</h3>
<p>你可以使用|来分隔可以匹配的不同选择:</p>
<ul>
<li>cat|dog表示匹配&#8221;cat&#8221;或者&#8221;dog&#8221;</li>
<li>red|blue|以及red||blue以及|red|blue都表示匹配red或者blue或者一个空字符串</li>
<li>a|b|c与[abc]相同</li>
<li>cat|dog|\|表示匹配&#8221;cat&#8221;或者&#8221;dog&#8221;或者一个分隔符”|“</li>
<li>[cat|dog]表示匹配a或者c或者d或者g或者o或者t或者一个分隔符“|”</li>
</ul>
<div>
<h4><strong>练习</strong></h4>
<p>简化下列正则表达式:</p>
<ul>
<li><code>s|t|u|v|w</code></li>
<li><code>aa|ab|ba|bb</code></li>
<li><code>[abc]|[^abc]</code></li>
<li><code>[^ab]|[^bc]</code></li>
<li><code>[ab][ab][ab]?[ab]?</code></li>
</ul>
<p><span style="font-family: 'Ubuntu Mono', monospace;"><strong><span style="font-size: medium;">答案</span></strong><br />
</span></p>
<ul>
<li><code>[s-w]</code></li>
<li><code>[ab]{2}</code></li>
<li><code>.</code></li>
<li><code>[^b]</code></li>
<li><code>[ab]{2,4}</code></li>
</ul>
</div>
<div>
<h4><strong>练习</strong></h4>
<p>使用正则表达式匹配1到31之间的整数，[1-31]不是正确答案！</p>
<p>这样的正则表达式不唯一. <code>[1-9]|[12][0-9]|3[01]</code> 是其中之一。</p>
<h3></h3>
<h3>分组</h3>
<p>你可以使用括号表示分组:</p>
<ul>
<li><span style="font-family: Tahoma;">通过使用 </span>Mon|Tues|Wednes|Thurs|Fri|Satur|Sun)day <span style="font-family: Tahoma; font-size: medium;">匹配一周中的某一天</span></li>
<li>(\w*)ility <span style="font-family: Tahoma; font-size: medium;"><span style="color: #0000ff;"> </span>与</span> \w*ility <span style="font-family: Tahoma; font-size: medium;">相同。都是匹配一个由&#8221;ility&#8221;结尾的单词。稍后我们会讲解，为何第一种方法更加有用。</span></li>
<li><span class='MathJax_Preview'>\(\)</span><script type='math/tex'></script>表示匹配一对括号。</li>
<li>[()]表示匹配任意一个左括号或者一个右括号</li>
</ul>
<div>
<h4><strong>练习</strong></h4>
<p>在《时间机器中》找到一对括号中的内容，然后通过修改正则表达式，找到不含括号的内容。</p>
<h4><strong>答案</strong></h4>
<p><code><span class='MathJax_Preview'>\(.*\)</span><script type='math/tex'>.*</script></code>. 然后是, <code><span class='MathJax_Preview'>\([^()]*\)</span><script type='math/tex'>[^()]*</script></code>.</p>
</div>
<p>分组可以包括空字符串：</p>
<ul>
<li>(red|blue)表示匹配red或者blue或者是一个空字符串</li>
<li>abc()def与abcdef相同</li>
</ul>
<p>你也可以在分组的基础上使用重复：</p>
<ul>
<li>(red|blue)？与(red|blue|)相同</li>
<li>\w+(\s+\w+)表示匹配一个或多个由空格分隔的单词</li>
</ul>
<h4><strong>练习</strong></h4>
<p>简化正则表达式 <code>\w+\W+\w+\W+\w+</code> 以及 <code>\w+\W+\w+\W+\w+\W+\w+\W+\w+\W+\w+</code>.</p>
<h4><strong>答案</strong></h4>
<p><code>\w+(\W+\w+){2}</code>, <code>\w+(\W+\w+){5}</code>.</p>
<p>&nbsp;</p>
<h3>单词分隔符</h3>
<p>在单词和非单词之间有单词分隔符。记住，一个单词\w是[0-9A-Za-z_]，而非单词字符是\W(大写)，表示[^0-9A-Za-z_].</p>
<p>在文本的开头和结尾通常也有单词分隔符。</p>
<p>在输入文本it&#8217;s a cat中，实际有八个单词分隔符。如果我们在cat之后在上一个空格，那就有九个单词分隔符。.</p>
<ul>
<li>\b表示匹配一个单词分隔符</li>
<li>\b\w\w\w\b表示匹配一个三字母单词</li>
<li>a\ba表示匹配两个a中间有一个单词分隔符。这个正则表达式永远不会有匹配的字符，无论输入怎样的文本。</li>
</ul>
<p>单词分隔符本身并不是字符。它们的宽度为0。下列正则表达式的作用不同</p>
<ul>
<li><code>(\bcat)\b</code></li>
<li><code>(\bcat\b)</code></li>
<li><code>\b(cat)\b</code></li>
<li><code>\b(cat\b)</code></li>
</ul>
<div>
<h4><strong>练习</strong></h4>
<p>在词典中找到最长的单词。</p>
<h4><strong>答案</strong></h4>
<p>在尝试之后发现，\b.{45,}\b可以在字典中找到最长单词<code><br />
</code></p>
</div>
<p>&nbsp;</p>
<h3>换行符</h3>
<p>一篇文本中可以有一行或多行，行与行之间由换行符分隔，比如：</p>
<ul>
<li>Line一行文字</li>
<li>Line break换行符</li>
<li>Line一行文字</li>
<li>Line break换行符</li>
<li>&#8230;</li>
<li>Line break换行符</li>
<li>Line一行文字</li>
</ul>
<p>注意，所有的文本都是以一行结束的，而不是以换行符结束。但是，任意一行都可能为空，包括最后一行。</p>
<p>行的起始位置，是在换行符和下一行首字符之间的空间。考虑到单词分隔符，文本的起始位置也可以当做是首行位置。</p>
<p>最后一行是最后一行的尾字符和换行符之间的空间。考虑到单词分隔符，文本的结束也可以认为是行的结束。</p>
<p>那么新的格式表示如下:</p>
<ul>
<li>Start-of-line, line, end-of-line</li>
<li>Line break</li>
<li>Start-of-line, line, end-of-line</li>
<li>Line break</li>
<li>&#8230;</li>
<li>Line break</li>
<li>Start-of-line, line, end-of-line</li>
</ul>
<p>基于上述概念:</p>
<ul>
<li>^表示匹配行的开始位置</li>
<li>$表示匹配行的结束位置</li>
<li>^&amp;表示一个空行</li>
<li><code>^.*&amp;</code><code><span style="font-family: Tahoma;"><span style="color: #0000ff;"> </span>表示匹配全文内容，因为行的开始符号也是一个字符，"."会匹配这个符号。找到单独的一行，可以使用</span></code> ^.*?$</li>
<li>\^\$表示匹配字符串“^$”</li>
<li>[$]表示匹配一个$。但是，[^]不是合法的正则表达式。记住在方括号中，字符有不同的特殊含义。要想在方括号内匹配^，必须用[\^]</li>
</ul>
<p>与字符分隔符一样，换行符也不是字符。它们宽度为0.如下所示的正则表达式作用不同：</p>
<ul>
<li><code>(^cat)$</code></li>
<li><code>(^cat$)</code></li>
<li><code>^(cat)$</code></li>
<li><code>^(cat$)</code></li>
</ul>
<div>
<h4><strong>练习</strong></h4>
<p>使用正则表达式在《时间机器》中找到最长的一行。</p>
<h4><b>答案</b></h4>
<p><b></b>使用正则表达式^.{73,}$可以匹配长度为73的一行</p>
<h3></h3>
<h3>文本分界</h3>
<p>在很多的正则表达式实现中，将^和$作为文本的开始符号和结束符号。</p>
<p>还有一些实现中，用\A和\z作为文本的开始和结束符号。</p>
<h2></h2>
<h2>捕捉和替换</h2>
<p>从这里开始，正则表达式真正体现出了它的强大。</p>
<h3>捕获组</h3>
<p>你已经知道了使用括号可以匹配一组符号。使用括号也可以捕获子串。假设正则表达式是一个小型计算机程序，那么捕获子串就是它输出的一部分。</p>
<p>正则表达式(\w*)ility表示匹配以ility结尾的词。第一个被捕获的部分是由\w*控制的。比如，输入的文本内容中有单词accessibility，那么首先被捕获的部分是accessib。如果输入的文本中有单独的ility，则首先被捕获的是一个空字符串。</p>
<p>你可能会有很多的捕获字符串，它们可能靠得很近。捕获组从左向右编号。也就是只需要对左括号计数。</p>
<p>假设有这样的正则表达式：(\w+) had a ((\w+) \w+)</p>
<p><span style="font-family: Tahoma;"><span style="font-size: medium;">输入的内容是：</span></span>I had a nice day <span style="font-family: Tahoma; font-size: medium;"><br />
</span></p>
<ul>
<li>捕获组1：I</li>
<li>捕获组2：nice day</li>
<li>捕获组3:nice</li>
</ul>
<p>在一些正则表达式的实现中，你可以从零开始编号，编号零表示匹配整句话：<code>I had a nice day</code>.</p>
<p>在其他的实现中，如果没有制定捕获组，那么捕获组1会自动地填入捕获组0的信息。</p>
<p>是的，这也意味着会有很多的括号。有一些正则表达式的实现中，提供了“非捕获组”的语法，但是这样的语法并不是标准语法，因此我们不会介绍。</p>
<div>
<p>从一个成功的匹配中返回的捕获组个数，与使用原来的正则表达式获得的捕获组个数相同。记住这一点，你可以解释一些奇怪的现象。.</p>
<p>正则表达式（（cat）|dog）表示匹配cat或者dog。这里有两个捕获组，如果输入文本是dog，那么捕获组1是dog,捕获组2为空。</p>
<p>正则表达式a(\w)*表示匹配一个以a开头的单词。这里只有一个捕获组</p>
<ul>
<li>如果输入文本为a,捕获组1为空。</li>
<li>如果输入文本为ad,捕获组为d</li>
<li>如果输入文本为avocado，捕获组1为v。但是捕获组0表示整个单词avocado.</li>
</ul>
</div>
<h3></h3>
<h3>替换</h3>
<p>假如你使用了一个正则表达式去匹配字符串，你可以描述另外一个字符串来替换其中的匹配字符。用来替换的字符串称为替换表达式。它的功能类似于</p>
<ul>
<li>常规的Replace会话</li>
<li>Java中的String.replace()函数</li>
<li>PHP的str_replace()函数</li>
<li>等等</li>
</ul>
<div>
<h4><strong>练习</strong></h4>
<p>将《时间机器》中所有的元音字母替换为r。</p>
<h4><strong>答案</strong></h4>
<p>使用正则表达式[aeiou]以及[AEIOU]，对应的替换字符串分别为r,R.</p>
</div>
<p>但是，你可以在替换表达式中引用捕获组。这是在替换表达式中，你可以唯一操作的地方。这也是非常有效的，因为这样你就不用重构你找到的字符串。</p>
<p>假设你正在尝试将美国风格的日期表示MM/DD/YY替换为ISO 8601日期表示YYYY-MM-DD</p>
<ul>
<li>从正则表达式<span style="color: #0000ff;">(\d\d)/(\d\d)/(\d\d)</span><span style="font-family: Tahoma; font-size: medium;">开始。注意，这其中有三个捕获组：月份，日期和两位的年份。</span></li>
</ul>
<ul>
<li>.捕获组的内容和捕获组编号之间用反斜杠分隔，因此你的替换表达式应该是<code>20\3-\1-\2</code>.</li>
<li>如果我们输入的文本中包含03/04/05表示2005年3月4日那么：
<ul>
<li>捕获组1:03</li>
<li>捕获组2：04</li>
<li>捕获组3：05</li>
<li>替换字符串<code style="font-style: inherit; font-weight: inherit;">2005-03-04</code>.</li>
</ul>
</li>
</ul>
<p>在替换表达式中，你可以多次使用捕获组</p>
<ul>
<li>对于双元音，正则表达式为([aeiou])，替换表达式为\l\l</li>
<li><span style="font-family: Tahoma;">在替换表达式中不能使用反斜杠。</span><span style="color: #888888; font-family: 'Ubuntu Mono', monospace; font-size: medium;">比如，你</span><span style="font-family: Tahoma;">在计算机程序中希望使用字符串中使用部分文本。那么，你必须在每个双引号或者反斜杠之前加上反斜杠。</span></li>
</ul>
<ul>
<li>你的正则表达式可以是<span style="font-family: 'Ubuntu Mono', monospace;"><span style="color: #0000ff;">([\\"])</span><span style="color: #262626;">。</span><span style="color: #262626; font-size: medium;">捕获组1是双引号或者反斜杠</span></span></li>
<li><span style="color: #262626; font-family: 'Ubuntu Mono', monospace; font-size: medium;">你的替换表达式应该是\\\l</span></li>
</ul>
<div>
<p>在某些实现中，采用美元符号$代替\</p>
</div>
<div>
<h4><strong>练习</strong></h4>
<p>使用正则表达式和替换表达式，将23h59这样的时间戳转化为23:59.</p>
<h4><strong>答案</strong></h4>
<p>正则表达式finds the timestamps, 替换表达式<code>\1:\2</code></p>
</div>
<h3></h3>
<h3>反向引用</h3>
<p>在一个正则表达式中，你也可以引用捕获组。这称作：反向引用</p>
<p>比如，[abc]{2}表示匹配aa或者ab或者ac或者ba或者bb或者bc或者ca或者cb或者cc.但是{[abc]}\1表示只匹配aa或者bb或者cc.</p>
<div>
<h4><strong>练习</strong></h4>
<p>在字典中，找到包含两次重复子串的最长单词，比如<code>papa</code>, <code>coco</code></p>
<p><code>\b(.{6,})\1\b</code> 匹配 <code>chiquichiqui</code>.</p>
<p><span style="font-family: Tahoma; font-size: medium;">如果我们不在乎单词的完整性，我们可以忽略单词的分解，使用正则表达式 </span><code>(.{7,})\1</code><code><span style="font-family: Tahoma;">匹配</span></code><code>countercountermeasure</code><span style="font-family: Tahoma; font-size: medium;"> 以及 </span><code>countercountermeasures</code><span style="font-family: Tahoma; font-size: medium;">.</span></p>
<h2>使用正则表达式编程</h2>
<p>特别提醒：</p>
<h3>过度使用的反斜杠</h3>
<p>在一些编程语言，比如Java中，对于包含正则表达式的字符串没有特殊标记。字符串有着自己的过滤规则，这是优先于正则表达式规则的，这是频繁使用反斜杠的原因。</p>
<p>比如在Java中</p>
<ul>
<li>匹配一个数字，使用的正则表达式从\d变为代码中的String re= &#8220;\\d&#8221;</li>
<li>匹配双引号字符串的正则表达式从<code style="font-style: inherit; font-weight: inherit;">"[^"]*"</code> 变为String re = &#8220;\&#8221;[^\"]*\&#8221;"</li>
<li><span style="font-family: Tahoma;">匹配反斜杠或者是左边方括号，或者右边方括号的正则表达式从</span><span style="color: #0000ff;">[\\<code class='tex2jax_ignore'>\[\]</code>] </span><span style="font-family: Tahoma; font-size: medium;">变为</span>String re = &#8220;[\\\\\<p style='text-align:center;'><span class='MathJax_Preview'>\[\\]</span><script type='math/tex;  mode=display'>\</script></p>]&#8221;;</li>
<li><code style="font-style: inherit; font-weight: inherit;">String re = "\\s";</code> 和<code style="font-style: inherit; font-weight: inherit;">String re = "[ \t\r\n]";</code> 是等价的. 注意它们实际执行调用时的层次不同。</li>
</ul>
<p>在其他的编程语言中，正则表达式是由特殊标明的，比如使用/。下面是JavaScript的例子：</p>
<ul>
<li>匹配一个数字，\d会简单写成 <code style="font-style: inherit; font-weight: inherit;">var regExp = /\d/;</code>.</li>
<li>匹配一个反斜杠或者一个左边的方括号或者一个右边的方括号， <code style="font-style: inherit; font-weight: inherit;">var regExp = /[\\<code class='tex2jax_ignore'>\[\]</code>]/;</code></li>
<li><code style="font-style: inherit; font-weight: inherit;">var regExp = /\s/;</code> 和 <code style="font-style: inherit; font-weight: inherit;">var regExp = /[ \t\r\n]/;</code> 是等价的</li>
<li>当然，这意味着在使用/时必须重复两次。比如找到URL必须使用<code style="font-style: inherit; font-weight: inherit;">var regExp = /https?:\/\//;</code>.</li>
</ul>
<p>我希望现在你能明白，我为什么让你特别注意反斜杠。</p>
<h3></h3>
<h3>动态正则表达式</h3>
<p>当你动态创建一个正则表达式的时候请特别小心。如果你使用的字符串不够完善的花，可能会有意想不到的匹配结果。这可能导致语法错误，更糟糕的是，你的正则表达式语法正确，但是结果无法预料。</p>
<p>错误的Java代码：</p>
<p>String sep = System.getProperty(&#8220;file.separator&#8221;); String[] directories = filePath.split(sep);</p>
<p><span style="font-family: Tahoma;">Bug:</span><span style="color: #0000ff;">String.split() </span><span style="font-family: Tahoma; font-size: medium;">认为sep是一个正则表达式。但是，在Windows中，Sep是表示匹配一个反斜杠，也就是与正则表达式&#8221;\\&#8221;相同。这个正则表达式是正确的，但是会返回一个异常：</span><code>PatternSyntaxException</code>.</p>
<p>任何好的编程语言都会提供一种良好的机制来跳过字符串中所有的元字符。在Java中，你可以这样实现：</p>
<p>String sep = System.getProperty(&#8220;file.separator&#8221;);</p>
<p>String[] directories = filePath.split(Pattern.quote(sep));</p>
<h3></h3>
<h3>循环中的正则表达式</h3>
<p>将正则表达式字符串加入反复运行的程序中，是一种开销很大的操作。如果你可以在循环中避免使用正则表达式，你可以大大提高效率。</p>
<h2></h2>
<h2>其他建议</h2>
<h3>输入验证</h3>
<div>正则表达式可以用来进行输入验证。但是严格的输入验证会使得用户体验较差。比如：</div>
<div></div>
<h4>信用卡号</h4>
<p><span style="font-family: Tahoma; font-size: medium;">在一个网站上，我输入了我的卡号比如 </span><code>1234 5678 8765 4321 </code><code><span style="font-family: Tahoma; font-size: medium;">网站拒绝接收。因为它使用了正则表达式\d{16}。</span></code></p>
<p><code><span style="font-family: Tahoma; font-size: medium;">正则表达式应该考虑到用户输入的空格和短横线。</span></code></p>
<p>实际上，为什么不先过滤掉所有的非数字字符，然后再进行有效性验证呢？这样做，可以先使用\D以及空的替换表达式。</p>
<div>
<h4><strong>练习</strong></h4>
<p>在不先过滤掉所有的非数字字符的情况下，使用正则表达式验证卡号的正确性。</p>
<h4><b>答案</b></h4>
<p><code>\D*(\d\D*){16}</code> is one of several variations which would accomplish this.</p>
</div>
<h4></h4>
<h4>名字</h4>
<p>不要使用正则表达式来验证姓名。实际上，即使可以，也不要企图验证姓名。</p>
<p>程序员对名字的错误看法:</p>
<ul>
<li>名字中不含空格</li>
<li>名字中没有连接符号</li>
<li>名字中只会使用ASCII码字符</li>
<li>名字中出现的字都在特殊字符集中</li>
<li>名字至少要有M个字的长度</li>
<li>名字不会超过N个字的长度</li>
<li>人们只有一个名</li>
<li>人们只有一个中间名</li>
<li>人们只有一个姓（最后三条是从英语的人名考虑）</li>
</ul>
<h4></h4>
<h4>电子邮件地址</h4>
<p>不要使用正则表达式验证邮箱地址的正确性。</p>
<p>首先，这样的验证很难是精确的。电子邮件地址是可以用正则表达式验证的，但是表达式会非常的长并且复杂。</p>
<p>短的正则表达式会导致错误。（你知道吗？电子邮箱地址中会有一些注释）</p>
<p>第二，即使一个电子邮件地址可以成功匹配正则表达式，也不代表这个邮箱实际存在。邮箱的唯一验证方法，是发送验证邮件。</p>
<h3></h3>
<h3>注意</h3>
<p>在严格的应用场景中，不要使用正则表达式来解析HTML或者XML。解析HTML或者XML：</p>
<ol>
<li>使用简单的正则表达式不能完成</li>
<li>总体来说非常困难</li>
<li>已经有其他的方法解决</li>
</ol>
<p>找到一个已经有的解析库来完成这个工作</p>
<h2>这就是55分钟的全部内容</h2>
<p>总结：</p>
<ul>
<li>字符: <code>a</code> <code>b</code> <code>c</code> <code>d</code> <code>1</code> <code>2</code> <code>3</code> <code>4</code> etc.</li>
<li>字符类: <code>.</code> <code>[abc]</code> <code>[a-z]</code> <code>\d</code> <code>\w</code> <code>\s</code>
<ul>
<li><code>.</code> 代表任何字符</li>
<li><code>\d </code><code><span style="color: #262626; font-family: Tahoma; font-size: medium;">表示</span></code><span style="font-family: Tahoma;">&#8220;数字&#8221;</span></li>
<li><code>\w</code>   表示&#8221;字母&#8221;, <code>[0-9A-Za-z_]</code></li>
<li><code>\s  </code> 表示 &#8220;空格, 制表符,回车或换行符&#8221;</li>
<li>否定字符类: <code>[^abc]</code> <code>\D</code> <code>\W</code> <code>\S</code></li>
</ul>
</li>
<li>重复: <code>{4}</code> <code>{3,16}</code> <code>{1,}</code> <code>?</code> <code>*</code> <code>+</code>
<ul>
<li><code>?</code> 表示 &#8220;零次或一次&#8221;</li>
<li><code>*</code> 表示 &#8220;大于零次&#8221;</li>
<li><code>+</code> 表示 &#8220;一次或一次以上&#8221;</li>
<li>如果不加上？，所有的重复都是最长匹配的（贪婪）</li>
</ul>
</li>
<li>分组: <code>(Septem|Octo|Novem|Decem)ber</code></li>
<li>词，行以及文本的分隔: <code>\b</code> <code>^</code> <code>$</code> <code>\A</code> <code>\z</code></li>
<li>转义字符: <code>\1</code> <code>\2</code> <code>\3</code> etc. (在匹配表达式和替换表达式中都可用)</li>
</ul>
<ul>
<li>元字符: <code>.</code> <code>\</code> <code>[</code> <code>]</code> <code>{</code> <code>}</code> <code>?</code> <code>*</code> <code>+</code> <code>|</code> <code>(</code> <code>)</code> <code>^</code> <code>$</code></li>
<li>在字符类中使用元字符: <code>[</code> <code>]</code> <code>\</code> <code>-</code> <code>^</code></li>
<li>使用反斜杠可以忽略元字符: <code>\</code></li>
</ul>
<h3> 致谢</h3>
<p>正则表达式非常常用而且非常有用。每个人在编辑文本或是编写程序时都必须了解怎样使用正则表达式。</p>
<h4><strong>练习</strong></h4>
<h4>选择正则表达式的某种实现，阅读相关文档。我保证，你会学到更多。</h4>