<p><strong>Awk是什么</strong></p>
<p>Awk、sed与grep，俗称Linux下的三剑客，它们之前有很多相似点，但是同样也各有各的特色，相似的地方是它们都可以匹配文本，其中sed和awk还可以用于文本编辑，而grep则不具备这个功用。sed是一种非交互式且面向字符流的编辑器（a &#8220;non-interactive&#8221; stream-oriented editor），而awk则是一门模式匹配的编程语言，因为它的主要功能是用于匹配文本并处理，同时它有一些编程语言才有的语法，例如函数、分支循环语句、变量等等，当然比起我们常见的编程语言，Awk相对比较简单。</p>
<p>使用Awk，我们可以做以下事情：</p>
<p>● 将文本文件视为由字段和记录组成的文本数据库；</p>
<p>● 在操作文本数据库的过程中能够使用变量；</p>
<p>● 能够使用数学运算和字符串操作；</p>
<p>● 能够使用常见的编程结构，例如条件分支与循环；</p>
<p>● 能够格式化输出；</p>
<p>● 能够自定义函数；</p>
<p>● 能够在awk脚本中执行UNIX命令；</p>
<p>● 能够处理UNIX命令的输出结果；</p>
<p>装备以上功能，awk能够做得事情非常多。但千里之行，始于足下，我们首先从最基本的命令行语法开始，一步一步得走入awk的编程世界。</p>
<p>&nbsp;</p>
<p><strong>命令行语法</strong></p>
<p>同sed一样，awk的命令行语法也有两种形式：</p>
<pre class="brush: shell; gutter: true">awk [-F ERE] [-v assignment] ... program [argument ...]
awk [-F ERE] -f progfile ...  [-v assignment] ...[argument ...]</pre>
<p>这里的program类似sed中的script，因为我们一直强调awk是一门编程语言，所以将awk的脚本视为一段代码。而awk的脚本同样可以写到一个文件中，并通过-f参数指定，这一点和sed是一样的。program一般多个pattern和action序列组成，当读入的记录匹配pattern时，才会执行相应的action命令。这里有一点要注意，在第一种形式中，除去命令行选项外，program参数一定要位于第一个位置。</p>
<p>Awk的输入被解析成多个记录（Record），默认情况下，记录的分隔符是\n，因此可以认为一行就是一个记录，记录的分隔符可以通过内置变量<code>RS</code>更改。当记录匹配某个pattern时，才会执行后续的action命令。</p>
<p>而每个记录由进一步地被分隔成多个字段（Field），默认情况下字段的分隔符是空白符，例如空格、制表符等等，也可以通过<code>-F ERE</code>选项或者内置变量<code>FS</code>更改。在awk中，可以通过$1，$2&#8230;来访问对应位置的字段，同时$0存放整个记录，这一点有点类似shell下的命令行位置参数。关于这些内容，我们会在下面详细介绍，这里你只要知道有这些东西就好。</p>
<p>标准的awk命令行参数主要由以下三个：</p>
<p>● <code>-F ERE</code>：定义字段分隔符，该选项的值可以是扩展的正则表达式（ERE）；</p>
<p>● <code>-f progfile</code>：指定awk脚本，可以同时指定多个脚本，它们会按照在命令行中出现的顺序连接在一起；</p>
<p>● <code>-v assignment</code>：定义awk变量，形式同awk中的变量赋值，即name=value，赋值发生在awk处理文本之前；</p>
<p>为了便于理解，这里举几个简单的例子。通过-F参数设置冒号:为分隔符，并打印各个字段：</p>
<pre class="brush: shell; gutter: true">[kodango@devops ~]$ echo &quot;1:2:3&quot; | awk -F: &#039;{print $1 &quot; and &quot; $2 &quot; and &quot; $3}&#039;
1 and 2 and 3</pre>
<p>在awk的脚本中访问通过-v选项设置的变量：</p>
<pre class="brush: shell; gutter: true">[kodango@devops ~]$ echo | awk -v a=1 &#039;BEGIN {print a}&#039;
1</pre>
<p>从上面可以看到，通过-v选项设置的变量在<code>BEGIN</code>的位置就可以访问了。<code>BEGIN</code>是一个特殊的pattern，它在awk处理输入之前就会执行，可以认为是一个初始化语句，与此对应的还有<code>END</code>。</p>
<p>好像还没介绍如何指定处理的文件，是不是最后的argument就是指定的文件？在看我这本书之前，我也是这样认为的，但是实际上arguemnt有两种形式，它们分别是输入文件（file）和变量赋值（assignment）。</p>
<p>awk可以同时指定多个输入文件，如果输入文件的文件名为&#8217;-'，表示从标准输入读取内容。</p>
<p>变量赋值类似-v选项，它的形式为name=value。awk中的变量名同一般的编程语言无太多区别，但是不能同awk的保留关键字重名，可以查看awk的man手册查询哪些是保留关键字。而变量值只有两种形式：字符串和数值。变量赋值必须位于脚本参数的后面，与文件名参数无先后顺序的要求，但是位于不同位置的赋值它的执行时机是不同的。</p>
<p>我们用实际的例子来解释这个区别，假设有两个文件：a和b，它们的内容分别如下所示：</p>
<pre class="brush: shell; gutter: true">[kodango@devops awk_temp]$ cat a
file a
[kodango@devops awk_temp]$ cat b
file b</pre>
<p>为了说明赋值操作发生的时机，我们在BEGIN，正常处理，END三个地方都打印变量的值。</p>
<p>第一种情况： 变量赋值位于所有文件名参数之前</p>
<pre class="brush: shell; gutter: true">[kodango@devops awk_temp]$ awk &#039;BEGIN {print &quot;BEGIN: &quot; var} {print &quot;PROCESS: &quot; var} \
END {print &quot;END: &quot; var }&#039; var=1 a
BEGIN: 
PROCESS: 1
END: 1</pre>
<p>结果：赋值操作发生在正常处理之前，<code>BEGIN</code>动作之后。</p>
<p>第二种情况：变量赋值位于所有文件名之后：</p>
<pre class="brush: shell; gutter: true">[kodango@devops awk_temp]$ awk &#039;BEGIN {print &quot;BEGIN: &quot; var} {print &quot;PROCESS: &quot; var} \
END {print &quot;END: &quot; var }&#039; a var=1  
BEGIN: 
PROCESS: 
END: 1</pre>
<p>结果：赋值操作发生在正常处理之后，<code>END</code>动作之前。</p>
<p>第三种情况：变量赋值位于文件名之间：</p>
<pre class="brush: shell; gutter: true">[kodango@devops awk_temp]$ awk &#039;BEGIN {print &quot;BEGIN: &quot; var} {print &quot;PROCESS: &quot; var} \
END {print &quot;END: &quot; var }&#039; a var=1 b
BEGIN: 
PROCESS: 
PROCESS: 1
END: 1</pre>
<p>结果：赋值操作发生在处理前面的文件之后，并且位于处理后面的文件之前；</p>
<p>总结如下：<br />
1. 如果变量赋值在第一个文件参数之前，在<code>BEGIN</code>动作之后执行，影响到正常处理和<code>END</code>动作；</p>
<p>2. 如果变量赋值在最后一个文件参数之后，在END动作之前执行，仅影响<code>END</code>动作；</p>
<p>3. 如果文件参数不存在，情况同1所述；</p>
<p>4. 如果变量赋值位于多个文件参数之间，在变量赋值前面的文件被处理后执行，影响到后续文件的处理和<code>END</code>动作；</p>
<p>所以变量赋值一定要考虑清楚用途，否则比较容易出错，不过一般情况下也不会用到变量赋值。</p>
<p>自然地大家会将变量赋值与-v assignment选项进行比较，赋值的形式是一致的，但是-v选项的执行时机比变量赋值要早：</p>
<pre class="brush: shell; gutter: true">[kodango@devops awk_temp]$ echo 1 | awk -v var=a &#039;BEGIN {print &quot;BEGIN: &quot; var}&#039;
BEGIN: a</pre>
<p>可见，-v选项的赋值操作在<code>BEGIN</code>动作之前就执行了。</p>
<p>变量赋值一定要小心不要与保留关键字重名，否则会报错：</p>
<pre class="brush: shell; gutter: true">[kodango@devops awk_temp]$ echo 1 | awk -v BEGIN=1 &#039;BEGIN {print &quot;BEGIN: &quot; BEGIN}&#039;
awk: fatal: cannot use gawk builtin `BEGIN&#039; as variable name</pre>
<p>&nbsp;</p>
<p><strong>记录（Record）与字段（Field)</strong></p>
<p>对于数据库来说，一个数据库表是由多条记录组成的，每一行表示一条记录（Record）。每条记录由多列组成，每一列表示一个字段（Field)。Awk将一个文本文件视为一个文本数据库，因此它也有记录和字段的概念。默认情况下，记录的分隔符是回车，字段的分隔符是空白符，所以文本文件的每一行表示一个记录，而每一行中的内容被空白分隔成多个字段。利用字段和记录，awk就可以非常灵活地处理文件的内容。</p>
<p>可以通过-F选项来修改默认的字段分隔符，例如/etc/passwd的每一行都是由冒号分隔成多个字段的，所以这里就需要将分隔符设置成冒号：</p>
<pre class="brush: shell; gutter: true">[kodango@devops awk_temp]$ awk -F: &#039;{print $1}&#039; /etc/passwd | head -3
root
bin
daemon</pre>
<p>这里通过$1引用第一人字段，类似地$2表示第二个字段，$3表示第三个字段&#8230;. $0则表示整个记录。内置变量NF记录着字段的个数，所以$NF表示最后一个字段：</p>
<pre class="brush: shell; gutter: true">[kodango@devops awk_temp]$ awk -F: &#039;{print $NF}&#039; /etc/passwd | head -3
/bin/bash
/bin/false
/bin/false</pre>
<p>当然，$(NF-1)表示倒数第二个。</p>
<p>内置变量FS也可以用于更改字段分隔符，它记录着当前的字段分隔符：</p>
<pre class="brush: shell; gutter: true">[kodango@devops awk_temp]$ awk -F: &#039;{print FS}&#039; /etc/passwd | head -1
:
[kodango@devops awk_temp]$ awk -v FS=: &#039;{print $1}&#039; /etc/passwd | head -1
root</pre>
<p>记录的分隔符可以通过内置变量RS更改：</p>
<pre class="brush: shell; gutter: true">[kodango@devops awk_temp]$ awk -v RS=: &#039;{print $0}&#039; /etc/passwd | head -1
root</pre>
<p>如果将RS设置成空，行为有就一点怪异了，它会将连续不为空行的所有行（一个段落）当作一个记录，而且强制回车为字段分隔符：</p>
<pre class="brush: shell; gutter: true">[kodango@devops awk_temp]$ cat awk_man.txt 

The awk utility shall execute programs written in the awk programming language,
which is specialized for textual data manipulation. An awk program is a sequence
of patterns and corresponding actions.  When  input  is  read  that matches a
pattern, the action associated with that pattern is carried out.

Input shall be interpreted as a sequence of records. By default, a record is a line,
less its terminating &lt;newline&gt;, but this can be changed by using the RS built-in
variable. Each record of input shall be matched in turn against each pattern in the
program. For each pattern matched, the associated action shall be executed.

[kodango@devops awk_temp]$ awk &#039;BEGIN {RS=&quot;&quot;;FS=&quot;:&quot;} {print &quot;First line: &quot; $1}&#039; awk_man.txt 
First line: The awk utility shall execute programs written in the awk programming language,
First line: Input shall be interpreted as a sequence of records. By default, a record is a line,</pre>
<p>这里，我们将变量赋值放到<code>BEGIN</code>动作中执行，因为<code>BEGIN</code>动作是在文件处理之前执行的，专门用于放初始化的语句。FS的赋值在这里是无效的，awk依然使用回车符来分隔字段。</p>
<p>&nbsp;</p>
<p><strong>脚本（Script）组成</strong></p>
<p>命令行中的program部分，可以称为awk代码,也可以称为awk脚本。一段awk脚本是由多个&#8217;<code>pattern { action }</code>&#8216;序列组成的。action是一个或者多个语句，它在输入行匹配pattern的时候被执行。如果pattern为空，表明这个action会在每一行处理时都会被执行。下面的例子简单地打印文件的每一行，这里不带任何参数的print语句打印的是整个记录，类似&#8217;<code>print $0</code>&#8216;：</p>
<pre class="brush: shell; gutter: true">[kodango@devops awk_temp]$ echo -e &#039;line1\nline2&#039; | awk &#039;{print}&#039; 
line1
line2</pre>
<p>除了<code><code>pattern { action }</code></code>，还可以在脚本中定义自定义的函数，函数定义格式如下所示：</p>
<pre class="brush: shell; gutter: true">function name(parameter list) { statements }</pre>
<p>函数的参数列表用逗号分隔，参数默认是局部变量，无法在函数之外访问，而在函数中定义的变量为全局变量，可以在函数之外访问，如：</p>
<pre class="brush: shell; gutter: true">[kodango@devops awk_temp]$ echo line1 | awk &#039;
function t(a) {
    b=a;
    print a;
} 

{
    print b;
    t(&quot;kodango.me&quot;); 
    print b;
}&#039;

kodango.me
kodango.me</pre>
<p>Awk脚本中的语句使用空行或者分号分隔，使用分号可以放在同一行，不过有时候会影响可读性，尤其是分支或循环结构中，很容易出错。</p>
<p>如果Awk中的一个语句太长，要分成多行，可以在行为使用反斜杠&#8217;\'：</p>
<pre class="brush: shell; gutter: true">[kodango@devops awk_temp]$ cat test.awk 

function t(a)
{
    b=a
    print &quot;This is a very long line, so use backslash to escape the newline \
then we will print the variable a: a=&quot; a
} 

{ print b; t(&quot;kodango.me&quot;); print b;}
[kodango@devops awk_temp]$ echo 1 | awk -f test.awk 

This is a very long line, so use backslash to escape the newline then we will print the variable a: a=kodango.me
kodango.me</pre>
<p>这里我们将脚本写到文件中，并通过-f参数来指定。但是，在一些特殊符号之后，是可以直接换行的，例如&#8221;, { &amp;&amp; ||&#8221;。</p>
<p>&nbsp;</p>
<p><strong>模式（Pattern）</strong></p>
<p>模式是awk中比较重要的一部分，它有以下几种情况：</p>
<p>● <code>/regular expression/</code>： 扩展的正则表达式（Extended Regular Expression）， 关于ERE可以参考<a href="http://www.infoq.com/cn/news/2011/07/regular-expressions-6-POSIX" target="_blank">这篇文章</a>；</p>
<p>● <code>relational expression</code>： 关系表达式，例如大于、小于、等于，关系表达式结果为true表示匹配；</p>
<p>● <code>BEGIN</code>： 特殊的模式，在第一个记录处理之前被执行，常用于初始化语句的执行；</p>
<p>● <code>END</code>： 特殊的模式，在最后一个记录处理之前被执行，常用于输出汇总信息；</p>
<p>● <code>pattern, pattern</code>：模式对，匹配两者之间的所有记录，类似sed的地址对；</p>
<p>例如查找匹配数字3的行：</p>
<pre class="brush: shell; gutter: true">[kodango@devops awk_temp]$ seq 1 20 | awk &#039;/3/ {print}&#039;
3
13</pre>
<p>相反地，可以在在正则表达式之前加上&#8217;!'表示不匹配：</p>
<pre class="brush: shell; gutter: true">[kodango@devops awk_temp]$ seq 1 5 | awk &#039;!/3/ {print}&#039;
1
2
4
5</pre>
<p>除了<code>BEGIN</code>和<code>END</code>这两个特殊的模式外，其余的模式都可以使用&#8217;&amp;&amp;&#8217;或者&#8217;||&#8217;运算符组合，前者表示逻辑与，后者表示逻辑或：</p>
<pre class="brush: shell; gutter: true">[kodango@devops awk_temp]$ seq 1 50 | awk &#039;/3/ &amp;&amp; /1/ {print}&#039;
13
31</pre>
<p>前面的正则都是整行匹配，有时候仅仅需要匹配某个字符，这样我们可以用表达式<code>$n ~ /ere/</code>：</p>
<pre class="brush: shell; gutter: true">[kodango@devops ~]$ awk &#039;$1 ~ /ko/ {print}&#039; /etc/passwd
kodango:x:1000:1000::/home/kodango:/bin/bash</pre>
<p>有时候我们只想显示特定和行，例如显示第一行：</p>
<pre class="brush: shell; gutter: true">[kodango@devops ~]$ seq 1 5 | awk &#039;NR==1 {print}&#039;
1</pre>
<p>&nbsp;</p>
<p><strong>正则表达式（Regular Expression）</strong></p>
<p>和<a href="http://blog.jobbole.com/31026/" target="_blank">sed篇</a>一样，这里我不会详细介绍正则表达式。因为正则表达式的内容介绍起来太麻烦，还是推荐同学阅读现有的文章（如<a href="http://www.infoq.com/cn/news/2011/07/regular-expressions-6-POSIX">Linux/Unix工具与正则表达式的POSIX规范</a>），里面对各个流派的正则表达式归纳地很清楚了。</p>
<p>&nbsp;</p>
<p><strong>表达式（Expressions）</strong></p>
<p>表达式可以由常量、变量、运算符和函数组成，常数和变量的值可以为字符串和数值。</p>
<p>Awk中的变量有三种类型：用户定义的变量，内置变量和字段变量。其中，内置变量名都是大写的。变量并不非一定要被声明或者被初始化，未初始化的字符串变量的值为&#8221;"，未初始化的数值变量的值为0。字段变量可以用$n来引用，n的取值范围为[0,NF]。n可以为一个变量，例如$NF代码最后一个字段，而$(NF-1)表示倒数第二个字段。</p>
<p>&nbsp;</p>
<p><strong>数组</strong></p>
<p>数组是一种特殊的变量，在awk中，比较特殊地是，数组的下标可以为数字或者字符串。数组的赋值很简单，下面将value赋值给数组下标为index的元素：</p>
<pre class="brush: shell; gutter: true">array[index]=value</pre>
<p>可以用for..in..语法遍历数组元素，其中item是数组元素对应的下标：</p>
<pre class="brush: shell; gutter: true">for (item in array)</pre>
<p>当然也可以在if分支判断中使用in操作符：</p>
<pre class="brush: shell; gutter: true">if (item in array)</pre>
<p>一个完整的例子如下所示：</p>
<pre class="brush: shell; gutter: true">[kodango@devops ~]$ echo &quot;1 2 3&quot; | awk &#039;{
for (i=0;i&lt;NF;i++)
  a[i]=i;
}

END {
print 3 in a
for (i in a)
   printf &quot;%s: %s\n&quot;, i, a[i];
}&#039;
0
0: 0
1: 1
2: 2</pre>
<p>&nbsp;</p>
<p><strong>内置变量</strong></p>
<p>Awk在内部维护了许多内置变量，或者称为系统变量，例如之前提到的<code>FS</code>、<code>RS</code>等等。常见的内置变量如下表所示</p>
<table border="1px">
<thead>
<tr>
<th>变量名</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>ARGC</td>
<td>命令行参数的各个，即ARGV数组的长度</td>
</tr>
<tr>
<td>ARGV</td>
<td>存放命令行参数</td>
</tr>
<tr>
<td>CONVFMT</td>
<td>定义awk内部数值转换成字符串的格式，默认值为&#8221;%.6g&#8221;</td>
</tr>
<tr>
<td>OFMT</td>
<td>定义输出时数值转换成字符串的格式，默认值为&#8221;%.6g&#8221;</td>
</tr>
<tr>
<td>ENVIRON</td>
<td>存放系统环境变量的关联数组</td>
</tr>
<tr>
<td>FILENAME</td>
<td>当前被处理的文件名</td>
</tr>
<tr>
<td>NR</td>
<td>记录的总个数</td>
</tr>
<tr>
<td>FNR</td>
<td>当前文件中的记录的总个数</td>
</tr>
<tr>
<td>FS</td>
<td>字段分隔符，默认为空白</td>
</tr>
<tr>
<td>NF</td>
<td>每个记录中字段的个数</td>
</tr>
<tr>
<td>RS</td>
<td>记录的分隔符，默认为回车</td>
</tr>
<tr>
<td>OFS</td>
<td>输出时字段的分隔符，默认为空白</td>
</tr>
<tr>
<td>ORS</td>
<td>输出时记录的分隔符，默认为回车</td>
</tr>
<tr>
<td>RLENGTH</td>
<td>被match函数匹配的子串长度</td>
</tr>
<tr>
<td>RSTART</td>
<td>被match函数匹配的子串位于目标字符串的起始下标</td>
</tr>
</tbody>
</table>
<p>&nbsp;</p>
<p>下面主要介绍几个比较难理解的内置变量：</p>
<p><strong>1. <code>ARGV</code>与<code>ARGC</code></strong></p>
<p><code>ARGV</code>与<code>ARGC</code>的意思比较好理解，就像C语言<code>main(int argc, char **argv)</code>。<code>ARGV</code>数组的下标从0开始到<code>ARGC</code>-1，它存放的是命令行参数，并且排除命令行选项（例如-v/-f）以及program部分。因此事实上<code>ARGV</code>只是存储argument的部分，即文件名（file）以及命令行变量赋值两部分的内容。</p>
<p>通过下面的例子可以大概了解ARGC与ARGV的用法：</p>
<pre class="brush: shell; gutter: true">[kodango@devops awk_temp]$  awk &#039;BEGIN {
&gt;     for (i = 0; i &lt; ARGC; i++)
&gt;         print ARGV[i]
&gt;  }&#039; inventory-shipped BBS-list
awk
inventory-shipped
BBS-list</pre>
<p><code>ARGV</code>的用法不仅限于此，它是可以修改的，可以更改数组元素的值，可以增加数组元素或者删除数组元素。</p>
<p>a. 更改<code>ARGV</code>元素的值</p>
<p>假设我们有a, b两个文件，它们各有一行内容：file a和file b。现在利用ARGV，我们可以做到偷梁换柱：</p>
<pre class="brush: shell; gutter: true">[kodango@devops awk_temp]$ awk &#039;BEGIN{ARGV[1]=&quot;b&quot;} {print}&#039; a
file b</pre>
<p>这里要注意<code>ARGV[1]="b"</code>的引号不能缺少，否则<code>ARGV[1]=b</code>会将变量b的值赋值给<code>ARGV[1]</code>。</p>
<p>当awk处理完一个文件之后，它会从<code>ARGV</code>的下一个元素获取参数，如果是一个文件则继续处理，如果是一个变量赋值则执行赋值操作：</p>
<pre class="brush: shell; gutter: true">[kodango@devops awk_temp]$ awk &#039;BEGIN{ARGV[1]=&quot;var=1&quot;} {print var}&#039; a b
1</pre>
<p>为什么这里只打印一次变量值呢？可以回头再看看<a title="Sed&amp;awk笔记之awk篇：快速了解Awk（一）" href="http://kodango.me/sed-and-awk-notes-part-7">上一篇</a>中介绍变量赋值的内容。</p>
<p>而当下一个元素为空时，则跳过不处理，这样可以避开处理某个文件：</p>
<pre class="brush: shell; gutter: true">[kodango@devops awk_temp]$ awk &#039;BEGIN{ARGV[1]=&quot;&quot;} {print}&#039; a b
file b</pre>
<p>上面的例子中a这个文件就被跳过了。</p>
<p>而当下一个元素的值为&#8221;-&#8221;时，表明从标准输入读取内容：</p>
<pre class="brush: shell; gutter: true">[kodango@devops awk_temp]$ awk &#039;BEGIN{ARGV[1]=&quot;-&quot;} {print}&#039; a b
a
a    # --&gt; 这里按下CTRL+D停止输入
file b</pre>
<p>b. 删除<code>ARGV</code>元素</p>
<p>删除<code>ARGV</code>元素和将元素的值赋值为空的效果是一样的，它们都会跳转对某个参数的处理：</p>
<pre class="brush: shell; gutter: true">[kodango@devops awk_temp]$ awk &#039;BEGIN{delete ARGV[1]} {print}&#039; a b
file b</pre>
<p>删除数组元素可以用<code>delete</code>语句。</p>
<p>c. 增加<code>ARGV</code>元素</p>
<p>我第一次看到<code>ARGV</code>变量的时候就在想，能不能利用<code>ARGV</code>变量避免提供命令行参数，就像这样:</p>
<pre class="brush: shell; gutter: true">awk &#039;BEGIN{ARGV[1]=&quot;a&quot;;} {print}&#039;</pre>
<p>但是事实上这样不行，awk会依然从标准输入中获取内容。下面的方法倒是可以，首先增加<code>ARGC</code>的值，再增加<code>ARGV</code>元素，我到现在也没搞懂这两者的区别：</p>
<pre class="brush: shell; gutter: true">[kodango@devops awk_temp]$ awk &#039;BEGIN{ARGC+=1;ARGV[1]=&quot;a&quot;} {print}&#039;
file a</pre>
<p><strong>2. <code>CONVFMT</code>与<code>OFMT</code></strong></p>
<p>Awk中允许数值到字符串相互转换，其中内置变量<code>CONVFMT</code>定义了awk内部数值到字符串转换的格式，它的默认值为&#8221;%.6g&#8221;：</p>
<pre class="brush: shell; gutter: true">[kodango@devops awk_temp]$ awk &#039;BEGIN {
    printf &quot;CONVFMT=%s, num=%f, str=%s\n&quot;, CONVFMT, 12.11, 12.11
}&#039;   
CONVFMT=%.6g, num=12.110000, str=12.11</pre>
<p>通过更改<code>CONVFMT</code>，我们可以定义自己的转换格式：</p>
<pre class="brush: shell; gutter: true">[kodango@devops awk_temp]$ awk &#039;BEGIN { 
    CONVFMT=&quot;%d&quot;;
    printf &quot;CONVFMT=%s, num=%f, str=%s\n&quot;, CONVFMT, 12.11, 12.11 
}&#039;  
CONVFMT=%d, num=12.110000, str=12</pre>
<p>与此对应地还有一个内置变量<strong>OFMT</strong>，它与<code>CONVFMT</code>的作用是类似的，只不过是影响输出的时候数字转换成字符串的格式：</p>
<pre class="brush: shell; gutter: true">[kodango@devops awk_temp]$ awk &#039;BEGIN { OFMT=&quot;%d&quot;;print 12.11 }&#039;  
12</pre>
<p><strong>3. <code>ENVIRON</code></strong></p>
<p><code>ENVIRON</code>是一个存放系统环境变量的关联数组，它的下标是环境变量名称，值是相应环境变量的值。例如：</p>
<pre class="brush: shell; gutter: true">[kodango@devops awk_temp]$ awk &#039;BEGIN { print ENVIRON[&quot;USER&quot;] }&#039;  
kodango</pre>
<p>利用环境变量也可以将值传递给awk：</p>
<pre class="brush: shell; gutter: true">[kodango@devops awk_temp]$ U=hello awk &#039;BEGIN { print ENVIRON[&quot;U&quot;] }&#039;  
hello</pre>
<p>可以利用for..in循环遍历<code>ENVIRON</code>数组：</p>
<pre class="brush: shell; gutter: true">[kodango@devops awk_temp]$ awk &#039;BEGIN { 
for (env in ENVIRON) 
    printf &quot;%s=%s\n&quot;, env, ENVIRON[env]; 
}&#039;</pre>
<p><strong>4. <code>RLENGTH</code>与<code>RSTART</code></strong></p>
<p><code>RLENGTH</code>与<code>RSTART</code>都是与<code>match</code>函数相关的，前者表示匹配的子串长度，后者表示匹配的子串位于目标字符串的起始下标。例如：</p>
<pre class="brush: shell; gutter: true">[kodango@devops ~]$ awk &#039;BEGIN {match(&quot;hello,world&quot;, /llo/); print RSTART,RLENGTH}&#039;
3 3</pre>
<p>关于<code>match</code>函数，我们会在以后介绍。</p>
<p>&nbsp;</p>
<p><strong>运算符</strong></p>
<p>表达式中必然少不了运算符，awk支持的运算符可以参见man手册中的“Expressions in awk”一小节内容：</p>
<pre class="brush: shell; gutter: true">[kodango@devops awk_temp]$ man awk | grep &quot;^ *Table: Expressions in&quot; -A 42 | sed &#039;s/^ *//&#039;
                                       Table: Expressions in Decreasing Precedence in awk

Syntax                Name                      Type of Result   Associativity
( expr )              Grouping                  Type of expr     N/A
$expr                 Field reference           String           N/A
++ lvalue             Pre-increment             Numeric          N/A
-- lvalue             Pre-decrement             Numeric          N/A
lvalue ++             Post-increment            Numeric          N/A
lvalue --             Post-decrement            Numeric          N/A
expr ^ expr           Exponentiation            Numeric          Right
! expr                Logical not               Numeric          N/A

+ expr                Unary plus                Numeric          N/A
- expr                Unary minus               Numeric          N/A
expr * expr           Multiplication            Numeric          Left

...以下省略...</pre>
<pre class="brush: shell; gutter: true"></pre>
<p><strong>语句（Statement）</strong></p>
<p>到目前为止，用得比较多的语句就是<code>print</code>，其它的还有<code>printf、delete、break、continue、exit、next</code>等等。这些语句与函数不同的是，它们不会使用带括号的参数，并且没有返回值。不过也有意外，比如<code>printf</code>就可以像函数一样的调用：</p>
<pre class="brush: shell; gutter: true">[kodango@devops awk_temp]$ echo 1 | awk &#039;{printf(&quot;%s\n&quot;, &quot;abc&quot;)}&#039;
abc</pre>
<p><code>break</code>和<code>continue</code>语句，大家应该比较了解，分别用于跳出循环和跳到下一个循环。</p>
<p><code>delete</code>用于删除数组中的某个元素，这个我们在上面介绍<code>ARGV</code>的时候也使用过。</p>
<p><code>exit</code>的用法顾名思义，就是退出awk的处理，然后会执行<code>END</code>部分的内容：</p>
<pre class="brush: shell; gutter: true">[kodango@devops awk_temp]$ echo $&#039;line1\nline2&#039; | awk &#039;{print;exit} END {print &quot;exit..&quot;}&#039; 
line1
exit..</pre>
<p><code>next</code>语句类似sed的n命令，它会读取下一条记录，并重新回到脚本的最开始处执行：</p>
<pre class="brush: shell; gutter: true">[kodango@devops awk_temp]$ echo $&#039;line1\nline2&#039; | awk &#039;{
&gt; print &quot;Before next..&quot;
&gt; print $0 
&gt; next
&gt; print &quot;After next..&quot;
&gt; }&#039;
Before next..
line1
Before next..
line2</pre>
<p>从上面可以看出<code>next</code>后面的print语句不会执行。</p>
<p>print与printf语句是使用最多的，它们将内容输出到标准输出。注意在print语句中，输出的变量之间带不带逗号是有区别的：</p>
<pre class="brush: shell; gutter: true">[kodango@devops awk_temp]$ echo &quot;1 2&quot; | awk &#039;{print $1, $2}&#039;
1 2
[kodango@devops awk_temp]$ echo &quot;1 2&quot; | awk &#039;{print $1 $2}&#039;
12</pre>
<p>print输出时，字段之间的分隔符可以由OFS重新定义：</p>
<pre class="brush: shell; gutter: true">[kodango@devops awk_temp]$ echo &quot;1 2&quot; | awk &#039;{OFS=&quot;;&quot;;print $1,$2}&#039;
1;2</pre>
<p>除此之外，print的输出还可以重定向到某个文件中或者某个命令：</p>
<pre class="brush: shell; gutter: true">print items &gt; output-file
print items &gt;&gt; output-file
print items | command</pre>
<p>假设有这一样一个文件，第一列是语句名称，第二列是对应的说明：</p>
<pre class="brush: shell; gutter: true">[kodango@devops awk_temp]$ cat column.txt 
statement|description
delete|delete item from an array
exit|exit from the awk process
next|read next input record and process</pre>
<p>现在我们要将两列的内容分别输出到statement.txt和description.txt两个文件中：</p>
<pre class="brush: shell; gutter: true">[kodango@devops awk_temp]$ awk -F&#039;|&#039; &#039;{
&gt; print $1 &gt; &quot;statement.txt&quot;;
&gt; print $2 &gt; &quot;description.txt&quot;
&gt; }&#039; column.txt 
[kodango@devops awk_temp]$ cat statement.txt 
statement
delete
exit
next
[kodango@devops awk_temp]$ cat description.txt 
description
delete item from an array
exit from the awk process
read next input record and process</pre>
<p>下面是一个重定向到命令的例子，假设我们要对下面的文件进行排序：</p>
<pre class="brush: shell; gutter: true">[kodango@devops awk_temp]$ cat num.list 
1
3
2
9
5</pre>
<p>可以通过将print的内容重定向到&#8221;sort -n&#8221;命令：</p>
<pre class="brush: shell; gutter: true">[kodango@devops awk_temp]$ awk &#039;{print | &quot;sort -n&quot;}&#039; num.list 
1
2
3
5
9</pre>
<p>printf命令的用法与print类似，也可以重定向到文件或者输出，只不过printf比print多了格式化字符串的功能。printf的语法也大多数语言包括bash的printf命令类似，这里就不多介绍了。</p>
<p>awk的函数分成数学函数、字符串函数、I/O处理函数以及用户自定义的函数，其中用户自定义的函数我们在<a title="Sed&amp;awk笔记之awk篇：快速了解Awk（二）" href="http://kodango.me/sed-and-awk-notes-part-8">上一篇</a>中也有简单的介绍，下面我们一一来介绍这几类函数。</p>
<p>&nbsp;</p>
<p><strong>数学函数</strong></p>
<p>awk中支持以下数学函数：</p>
<p>● <code>atan2(y,x)</code>：反正切函数；</p>
<p>● <code>cos(x)</code>：余弦函数；</p>
<p>● <code>sin(x)</code>：正弦函数；</p>
<p>● <code>exp(x)</code>：以自然对数e为底指数函数；</p>
<p>● <code>log(x)</code>：计算以e 为底的对数值；</p>
<p>● <code>sqrt(x)</code>：绝对值函数；</p>
<p>● <code>int(x)</code>：将数值转换成整数；</p>
<p>● <code>rand()</code>：返回0到1的一个随机数值，不包含1；</p>
<p>● <code>srand([expr])</code>：设置随机种子，一般与rand函数配合使用，如果参数为空，默认使用当前时间为种子；</p>
<p>例如，我们使用<code>rand()</code>函数生成一个随机数值：</p>
<pre class="brush: shell; gutter: true">[kodango@devops awk_temp]$ awk &#039;BEGIN {print rand(),rand();}&#039;
0.237788 0.291066
[kodango@devops awk_temp]$ awk &#039;BEGIN {print rand(),rand();}&#039;
0.237788 0.291066</pre>
<p>但是你会发现，每次awk执行都会生成同样的随机数，但是在一次执行过程中产生的随机数又是不同的。因为每次awk执行都使用了同样的种子，所以我们可以用<code>srand()</code>函数来设置种子:</p>
<pre class="brush: shell; gutter: true">[kodango@devops awk_temp]$ awk &#039;BEGIN {srand();print rand(),rand();}&#039;
0.171625 0.00692412
[kodango@devops awk_temp]$ awk &#039;BEGIN {srand();print rand(),rand();}&#039;
0.43269 0.782984</pre>
<p>这样每次生成的随机数就不一样了。</p>
<p>利用<code>rand()</code>函数我们也可以生成1到n的整数：</p>
<pre class="brush: shell; gutter: true">[kodango@devops awk_temp]$ awk &#039;
&gt; function randint(n) { return int(n*rand()); }
&gt; BEGIN { srand(); print randint(10);
&gt; }&#039;
3</pre>
<p><strong>字符串函数</strong></p>
<p>awk中包含大多数常见的字符串操作函数。</p>
<p><strong>1. <code>sub(ere, repl[, in])</code></strong></p>
<p>描述：简单地说，就是将in中匹配ere的部分替换成repl，返回值是替换的次数。如果in参数省略，默认使用$0。替换的动作会直接修改变量的值。</p>
<p>下面是一个简单的替换的例子：</p>
<pre class="brush: shell; gutter: true">[kodango@devops ~]$ echo &quot;hello, world&quot; | awk &#039;{print sub(/ello/, &quot;i&quot;); print}&#039;
1
hi, world</pre>
<p>在repl参数中&amp;是一个元字符，它表示匹配的内容，例如：</p>
<pre class="brush: shell; gutter: true">[kodango@devops ~]$ awk &#039;BEGIN {var=&quot;kodango&quot;; sub(/kodango/, &quot;hello, &amp;&quot;, var); print var}&#039;
hello, kodango</pre>
<p><strong>2. <code>gsub(ere, repl[, in])</code></strong></p>
<p>描述：同<code>sub()</code>函数功能类似，只不过是<code>gsub()</code>是全局替换，即替换所有匹配的内容。</p>
<p><strong>3. <code>index(s, t)</code></strong></p>
<p>描述：返回字符串t在s中出现的位置，注意这里位置是从1开始计算的，如果没有找到则返回0。</p>
<p>例如：</p>
<pre class="brush: shell; gutter: true">[kodango@devops ~]$ awk &#039;BEGIN {print index(&quot;kodango&quot;, &quot;o&quot;)}&#039;
2
[kodango@devops ~]$ awk &#039;BEGIN {print index(&quot;kodango&quot;, &quot;w&quot;)}&#039;
0</pre>
<p><strong>4. <code>length[([s])]</code></strong></p>
<p>描述：返回字符串的长度，如果参数s没有指定，则默认使用$0作为参数。</p>
<p>例如：</p>
<pre class="brush: shell; gutter: true">[kodango@devops ~]$ awk &#039;BEGIN {print length(&#039;kodango&#039;);}&#039;
0
[kodango@devops ~]$ echo &quot;first line&quot; | awk &#039;{print length();}&#039;
10</pre>
<p><strong>5. <code>match(s, ere)</code></strong></p>
<p>描述： 返回字符串s匹配ere的起始位置，如果不匹配则返回0。该函数会定义<code>RSTART</code>和<code>RLENGTH</code>两个内置变量。<code>RSTART</code>与返回值相同，<code>RLENGTH</code>记录匹配子串的长度，如果不匹配则为-1。</p>
<p>例如：</p>
<pre class="brush: shell; gutter: true">[kodango@devops ~]$ awk &#039;BEGIN {
print match(&quot;kodango&quot;, /dango/);
printf &quot;Matched at: %d, Matched substr length: %d\n&quot;, RSTART, RLENGTH;
}&#039;
3
Matched at: 3, Matched substr length: 5</pre>
<p><strong>6. <code>split(s, a[, fs])</code></strong></p>
<p>描述：将字符串按照分隔符fs，分隔成多个部分，并存到数组a中。注意，存放的位置是从第1个数组元素开始的。如果fs为空，则默认使用FS分隔。函数返回值分隔的个数。</p>
<p>例如：</p>
<pre class="brush: shell; gutter: true">[kodango@devops ~]$ awk &#039;BEGIN {
&gt; split(&quot;1;2;3;4;5&quot;, arr, &quot;;&quot;)
&gt; for (i in arr)
&gt;     printf &quot;arr[%d]=%d\n&quot;, i, arr[i];
&gt; }&#039;
arr[4]=4
arr[5]=5
arr[1]=1
arr[2]=2
arr[3]=3</pre>
<p>这里有一个奇怪的地方是for..in..输出的数组不是按顺序输出的，如果要按顺序输出可以用常规的for循环:</p>
<pre class="brush: shell; gutter: true">[kodango@devops ~]$ awk &#039;BEGIN {
&gt; split(&quot;1;2;3;4;5&quot;, arr, &quot;;&quot;)
&gt; for (i=0;^C
[kodango@devops ~]$ awk &#039;BEGIN {
&gt; n=split(&quot;1;2;3;4;5&quot;, arr, &quot;;&quot;)
&gt; for (i=1; i&lt;=n; i++)
&gt;     printf &quot;arr[%d]=%d\n&quot;, i, arr[i];
&gt; }&#039;
arr[1]=1
arr[2]=2
arr[3]=3
arr[4]=4
arr[5]=5</pre>
<p><strong>7. <code>sprintf(fmt, expr, expr, ...)</code></strong></p>
<p>描述：类似printf，只不过不会将格式化后的内容输出到标准输出，而是当作返回值返回。</p>
<p>例如：</p>
<pre class="brush: shell; gutter: true">[kodango@devops ~]$ awk &#039;BEGIN {
&gt; var=sprintf(&quot;%s=%s&quot;, &quot;name&quot;, &quot;value&quot;)
&gt; print var
&gt; }&#039;
name=value</pre>
<p><strong>8. <code>substr(s, m[, n])</code></strong></p>
<p>描述：返回从位置m开始的，长度为n的子串，其中位置从1开始计算，如果未指定n或者n值大于剩余的字符个数，则子串一直到字符串末尾为止。</p>
<p>例如：</p>
<pre class="brush: shell; gutter: true">[kodango@devops ~]$ awk &#039;BEGIN { print substr(&quot;kodango&quot;, 2, 3); }&#039;
oda
[kodango@devops ~]$ awk &#039;BEGIN { print substr(&quot;kodango&quot;, 2); }&#039;
odango</pre>
<p><strong>9. <code>tolower(s)</code></strong></p>
<p>描述：将字符串转换成小写字符。</p>
<p>例如：</p>
<pre class="brush: shell; gutter: true">[kodango@devops ~]$ awk &#039;BEGIN {print tolower(&quot;KODANGO&quot;);}&#039;
kodango</pre>
<p><strong>10. <code>toupper(s)</code></strong></p>
<p>描述：将字符串转换成大写字符。</p>
<p>例如</p>
<pre class="brush: shell; gutter: true">[kodango@devops ~]$ awk &#039;BEGIN {print tolower(&quot;kodango&quot;);}&#039;
KODANGO</pre>
<p>&nbsp;</p>
<p><strong>I/O处理函数</strong></p>
<p><strong>1. <code>getline</code></strong></p>
<p><code>getline</code>的用法相对比较复杂，它有几种不同的形式。不过它的主要作用就是从输入中每次获取一行输入。</p>
<p>a. <code>expression | getline [var]</code></p>
<p>这种形式将前面管道前命令输出的结果作为<code>getline</code>的输入，每次读取一行。如果后面跟有var，则将读取的内容保存到var变量中，否则会重新设置$0和<code>NF</code>。</p>
<p>例如，我们将上面的statement.txt文件的内容显示作为<code>getline</code>的输入：</p>
<pre class="brush: shell; gutter: true">[kodango@devops awk_temp]$ awk &#039;BEGIN { while(&quot;cat statement.txt&quot; | getline var) print var}&#039; 
statement
delete
exit
next</pre>
<p>上面的例子中命令要用双引号，&#8221;<code>cat statement.txt</code>&#8220;，这一点同<code>print/printf</code>是一样的。</p>
<p>如果不加var，则直接写到$0中，注意<code>NF</code>值也会被更新：</p>
<pre class="brush: shell; gutter: true">[kodango@devops awk_temp]$ awk &#039;BEGIN { while(&quot;cat statement.txt&quot; | getline) print $0,NF}&#039; 
statement 1
delete 1
exit 1
next 1</pre>
<p>b. <code>getline [var]</code></p>
<p>第二种形式是直接使用<code>getline</code>，它会从处理的文件中读取输入。同样地，如果var没有，则会设置$0，并且这时候会更新<code>NF</code>, <code>NR</code>和<code>FNR</code>：</p>
<pre class="brush: shell; gutter: true">[kodango@devops awk_temp]$ awk &#039;{      
&gt; while (getline) 
&gt;    print NF, NR, FNR, $0;
&gt; }&#039; statement.txt
1 2 2 delete
1 3 3 exit
1 4 4 next</pre>
<p>c. <code>getline [var] &lt; expression</code></p>
<p>第三种形式从expression中重定向输入，与第一种方法类似，这里就不加赘述了。</p>
<p><strong>2. <code>close</code></strong></p>
<p><code>close</code>函数可以用于关闭已经打开的文件或者管道，例如<code>getline</code>函数的第一种形式用到管道，我们可以用<code>close</code>函数把这个管道关闭，<code>close</code>函数的参数与管道的命令一致：</p>
<pre class="brush: shell; gutter: true">[kodango@devops awk_temp]$ awk &#039;BEGIN {
while(&quot;cat statement.txt&quot; | getline) {
   print $0;
   close(&quot;cat statement.txt&quot;);
}}&#039;
statement
statement
statement
statement
statement</pre>
<p>但是每次读了一行后，关闭管道，然后重新打开又重新读取第一行就死循环了。所以要慎用，一般情况下也很少会用到<code>close</code>函数。</p>
<p><strong>3. <code>system</code></strong></p>
<p>这个函数很简单，就是用于执行外部命令，例如：</p>
<pre class="brush: shell; gutter: true">[kodango@devops awk_temp]$ awk &#039;BEGIN {system(&quot;uname -r&quot;);}&#039;
3.6.2-1-ARCH</pre>
<p>&nbsp;</p>
<p><strong>结束语</strong></p>
<p>快速了解Awk系列的几篇文章相对比较粗糙，我是参考Awk的man手册以及《Sed &amp; Awk》附录B总结而成的，但是应该可以让大家对awk有一个大致的了解，欢迎大家一起交流。</p>
<p>&nbsp;</p>
