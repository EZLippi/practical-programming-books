<p>Vim的学习曲线相当的大，所以，如果你一开始看到的是一大堆VIM的命令分类，你一定会对这个编辑器失去兴趣的。下面的文章翻译自《<a href="http://yannesposito.com/Scratch/en/blog/Learn-Vim-Progressively/" target="_blank">Learn Vim Progressively</a>》，我觉得这是给新手最好的VIM的升级教程了，没有列举所有的命令，只是列举了那些最有用的命令。非常不错。</p>
<p style="text-align: center;"><a href="http://jbcdn2.b0.upaiyun.com/2012/04/code-editor-learing-curve.jpg" rel="lightbox[18339]" title="一些常见编辑器的学习曲线图"><img class="aligncenter size-full wp-image-18359" title="一些常见编辑器的学习曲线图" alt="一些常见编辑器的学习曲线图" src="http://jbcdn2.b0.upaiyun.com/2012/04/code-editor-learing-curve.jpg" width="480" height="332" /></a></p>
<p>——————————正文开始——————————</p>
<p>你想以最快的速度学习人类史上最好的文本编辑器VIM吗？你先得懂得如何在VIM幸存下来，然后一点一点地学习各种戏法。</p>
<p><a href="http://www.vim.org/">Vim</a> the Six Billion Dollar editor</p>
<p><span style="color: #888888;">Better, Stronger, Faster.</span></p>
<p>学习 <a href="http://www.vim.org/">vim</a> 并且其会成为你最后一个使用的文本编辑器。没有比这个更好的文本编辑器了，非常地难学，但是却不可思议地好用。</p>
<p>我建议下面这四个步骤：</p>
<p>1. 存活</p>
<p>2. 感觉良好</p>
<p>3. 觉得更好，更强，更快</p>
<p>4.使用VIM的超能力</p>
<p>当你走完这篇文章，你会成为一个vim的 superstar。</p>
<p>在开始学习以前，我需要给你一些警告：</p>
<p>● 学习vim在开始时是痛苦的。</p>
<p>● 需要时间</p>
<p>● 需要不断地练习，就像你学习一个乐器一样。</p>
<p>● 不要期望你能在3天内把vim练得比别的编辑器更有效率。</p>
<p>● 事实上，你需要2周时间的苦练，而不是3天。</p>
<p>&nbsp;</p>
<p><span style="color: #ff0000;"><strong>第一级 – 存活</strong></span></p>
<p>1. 安装 <a href="http://www.vim.org/">vim</a></p>
<p>2. 启动 vim</p>
<p>3. <strong>什么也别干！</strong>请先阅读</p>
<p>当你安装好一个编辑器后，你一定会想在其中输入点什么东西，然后看看这个编辑器是什么样子。但vim不是这样的，请按照下面的命令操作：</p>
<p>● 启 动Vim后，vim在 <em>Normal</em> 模式下。</p>
<p>● 让我们进入 <em>Insert</em> 模式，请按下键 i 。(陈皓注：你会看到vim左下角有一个–insert–字样，表示，你可以以插入的方式输入了）</p>
<p>● 此时，你可以输入文本了，就像你用“记事本”一样。</p>
<p>● 如果你想返回 <em>Normal</em> 模式，请按 <code>ESC</code> 键。</p>
<p>现在，你知道如何在 <em>Insert</em> 和 <em>Normal</em> 模式下切换了。下面是一些命令，可以让你在 <em>Normal</em> 模式下幸存下来：</p>
<p><code>● i</code> → <em>Insert</em> 模式，按 <code>ESC</code> 回到 <em>Normal</em> 模式.</p>
<p><code>● x</code> → 删当前光标所在的一个字符。</p>
<p><code>● :wq</code> → 存盘 + 退出 (<code>:w</code> 存盘, <code>:q</code> 退出)   （陈皓注：:w 后可以跟文件名）</p>
<p><code>● dd</code> → 删除当前行，并把删除的行存到剪贴板里</p>
<p><code>● p</code> → 粘贴剪贴板</p>
<p><strong>推荐</strong>:</p>
<p><code>● hjkl</code> (强例推荐使用其移动光标，但不必需) →你也可以使用光标键 (←↓↑→). 注: <code>j</code> 就像下箭头。</p>
<p><code>● :help &lt;command&gt;</code> → 显示相关命令的帮助。你也可以就输入 <code>:help</code> 而不跟命令。（陈皓注：退出帮助需要输入:q）</p>
<p>你能在vim幸存下来只需要上述的那5个命令，你就可以编辑文本了，你一定要把这些命令练成一种下意识的状态。于是你就可以开始进阶到第二级了。</p>
<p>当是，在你进入第二级时，需要再说一下 <em>Normal </em>模式。在一般的编辑器下，当你需要copy一段文字的时候，你需要使用 <code>Ctrl</code> 键，比如：<code>Ctrl-C</code>。也就是说，Ctrl键就好像功能键一样，当你按下了功能键Ctrl后，C就不在是C了，而且就是一个命令或是一个快键键了，<strong>在VIM的Normal模式下，所有的键就是功能键了</strong>。这个你需要知道。</p>
<p>标记:</p>
<p>● 下面的文字中，如果是 <code>Ctrl-λ</code>我会写成 <code>&lt;C-λ&gt;</code>.</p>
<p>● 以 <code>:</code> 开始的命令你需要输入 <code>&lt;enter&gt;</code>回车，例如 — 如果我写成 <code>:q</code> 也就是说你要输入 <code>:q&lt;enter&gt;</code>.</p>
<h4 id="nd-level----feel-comfortable"></h4>
<p><span style="color: #ff0000;"><strong>第二级 – 感觉良好</strong></span></p>
<p>上面的那些命令只能让你存活下来，现在是时候学习一些更多的命令了，下面是我的建议：（陈皓注：所有的命令都需要在Normal模式下使用，如果你不知道现在在什么样的模式，你就狂按几次ESC键）</p>
<p><strong>1. 各种插入模式</strong></p>
<p><strong></strong><code>● a</code> → 在光标后插入</p>
<p><code>● o</code> → 在当前行后插入一个新行</p>
<p><code>● O</code> → 在当前行前插入一个新行</p>
<p><code>● cw</code> → 替换从光标所在位置后到一个单词结尾的字符</p>
<p><strong>2. 简单的移动光标</strong></p>
<p><strong></strong><code>● 0</code> → 数字零，到行头</p>
<p><code>● ^</code> → 到本行第一个不是blank字符的位置（所谓blank字符就是空格，tab，换行，回车等）</p>
<p><code>● $</code> → 到本行行尾</p>
<p><code>● g_</code> → 到本行最后一个不是blank字符的位置。</p>
<p><code>● /pattern</code> → 搜索 <code>pattern</code> 的字符串（陈皓注：如果搜索出多个匹配，可按n键到下一个）</p>
<p><strong>3. 拷贝/粘贴</strong> （陈皓注：p/P都可以，p是表示在当前位置之后，P表示在当前位置之前）</p>
<p><code>● P</code> → 粘贴</p>
<p><code>● yy</code> → 拷贝当前行当行于 <code>ddP</code></p>
<p><code></code><strong>4. Undo/Redo</strong></p>
<p><strong></strong><code>● u</code> → undo</p>
<p><code>● &lt;C-r&gt;</code> → redo</p>
<p><strong>5. 打开/保存/退出/改变文件</strong>(Buffer)</p>
<p><code>● :e &lt;path/to/file&gt;</code> → 打开一个文件</p>
<p><code>● :w</code> → 存盘</p>
<p><code>● :saveas &lt;path/to/file&gt;</code> → 另存为 <code>&lt;path/to/file&gt;</code></p>
<p><code></code><code>● :x</code>， <code>ZZ</code> 或 <code>:wq</code> → 保存并退出 (:<code>x</code> 表示仅在需要时保存，ZZ不需要输入冒号并回车)</p>
<p><code>● :q!</code> → 退出不保存 <code>:qa!</code> 强行退出所有的正在编辑的文件，就算别的文件有更改。</p>
<p><code>● :bn</code> 和 <code>:bp</code> → 你可以同时打开很多文件，使用这两个命令来切换下一个或上一个文件。（陈皓注：我喜欢使用:n到下一个文件）</p>
<p>花点时间熟悉一下上面的命令，一旦你掌握他们了，你就几乎可以干其它编辑器都能干的事了。但是到现在为止，你还是觉得使用vim还是有点笨拙，不过没关系，你可以进阶到第三级了。</p>
<h4 id="rd-level----better-stronger-faster"></h4>
<p><span style="color: #ff0000;"><strong>第三级 – 更好，更强，更快</strong></span></p>
<p>先恭喜你！你干的很不错。我们可以开始一些更为有趣的事了。在第三级，我们只谈那些和vi可以兼容的命令。</p>
<p id="better"><strong>更好</strong></p>
<p>下面，让我们看一下vim是怎么重复自己的：</p>
<p><code>1. .</code> → (小数点) 可以重复上一次的命令</p>
<p>2. N&lt;command&gt; → 重复某个命令N次</p>
<p>下面是一个示例，找开一个文件你可以试试下面的命令：</p>
<p><code>● 2dd</code> → 删除2行</p>
<p><code>● 3p</code> → 粘贴文本3次</p>
<p><code>● 100idesu [ESC]</code> → 会写下 “desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu “</p>
<p><code>● .</code> → 重复上一个命令—— 100 “desu “.</p>
<p><code>● 3.</code> → 重复 3 次 “desu” (注意：不是 300，你看，VIM多聪明啊).</p>
<p id="stronger"><strong>更强</strong></p>
<p>你要让你的光标移动更有效率，你一定要了解下面的这些命令，<strong>千万别跳过</strong>。</p>
<p>1. N<code>G</code> → 到第 N 行 （陈皓注：注意命令中的G是大写的，另我一般使用 : N 到第N行，如 :137 到第137行）</p>
<p><code>2. gg</code> → 到第一行。（陈皓注：相当于1G，或 :1）</p>
<p><code>3. G</code> → 到最后一行。</p>
<p>4. 按单词移动：</p>
<p><code>● w</code> → 到下一个单词的开头。</p>
<p><code>● e</code> → 到下一个单词的结尾。</p>
<p>&gt; 如果你认为单词是由默认方式，那么就用小写的e和w。默认上来说，一个单词由字母，数字和下划线组成（陈皓注：程序变量）</p>
<p>&gt; 如果你认为单词是由blank字符分隔符，那么你需要使用大写的E和W。（陈皓注：程序语句）</p>
<p><a href="http://jbcdn2.b0.upaiyun.com/2012/04/word_moves1.jpg" rel="lightbox[18339]" title="简明 Vim 练级攻略"><img class="aligncenter size-full wp-image-18352" title="简明 Vim 练级攻略" alt="简明 Vim 练级攻略" src="http://jbcdn2.b0.upaiyun.com/2012/04/word_moves1.jpg" width="357" height="83" /></a></p>
<p>下面，让我来说说最强的光标移动：</p>
<p><code>● %</code> : 匹配括号移动，包括 <code>(</code>, <code>{</code>, <code>[</code>. （陈皓注：你需要把光标先移到括号上）</p>
<p><code>● *</code> 和 <code>#</code>:  匹配光标当前所在的单词，移动光标到下一个（或上一个）匹配单词（*是下一个，#是上一个）</p>
<p>相信我，上面这三个命令对程序员来说是相当强大的。</p>
<p id="faster"><strong>更快</strong></p>
<p>你一定要记住光标的移动，因为很多命令都可以和这些移动光标的命令连动。很多命令都可以如下来干：</p>
<p><code>&lt;start position&gt;&lt;command&gt;&lt;end position&gt;</code></p>
<p>例如 <code>0y$</code> 命令意味着：</p>
<p><code>● 0</code> → 先到行头</p>
<p><code>● y</code> → 从这里开始拷贝</p>
<p><code>● $</code> → 拷贝到本行最后一个字符</p>
<p>你可可以输入 <code>ye</code>，从当前位置拷贝到本单词的最后一个字符。</p>
<p>你也可以输入 <code>y2/foo</code> 来拷贝2个 “foo” 之间的字符串。</p>
<p>还有很多时间并不一定你就一定要按y才会拷贝，下面的命令也会被拷贝：</p>
<p><code>● d</code> (删除 )</p>
<p><code>● v</code> (可视化的选择)</p>
<p><code>● gU</code> (变大写)</p>
<p><code>● gu</code> (变小写)</p>
<p>● 等等</p>
<div>（陈皓注：可视化选择是一个很有意思的命令，你可以先按v，然后移动光标，你就会看到文本被选择，然后，你可能d，也可y，也可以变大写等）</div>
<h4 id="th-level----vim-superpowers"></h4>
<p><span style="color: #ff0000;"><strong>第四级 – Vim 超能力</strong></span></p>
<p>你只需要掌握前面的命令，你就可以很舒服的使用VIM了。但是，现在，我们向你介绍的是VIM杀手级的功能。下面这些功能是我只用vim的原因。</p>
<p id="move-on-current-line-0---f-f-t-t--"><strong>在当前行上移动光标: <code>0</code> <code>^</code> <code>$</code> <code>f</code> <code>F</code> <code>t</code> <code>T</code> <code>,</code> <code>;</code></strong></p>
<p><strong><code></code></strong><code>● 0</code> → 到行头</p>
<p><code>● ^</code> → 到本行的第一个非blank字符</p>
<p><code>● $</code> → 到行尾</p>
<p><code>● g_</code> → 到本行最后一个不是blank字符的位置。</p>
<p><code>● fa</code> → 到下一个为a的字符处，你也可以fs到下一个为s的字符。</p>
<p><code>● t,</code> → 到逗号前的第一个字符。逗号可以变成其它字符。</p>
<p><code>● 3fa</code> → 在当前行查找第三个出现的a。</p>
<p><code>● F</code> 和 <code>T</code> → 和 <code>f</code> 和 <code>t</code> 一样，只不过是相反方向。</p>
<p><a href="http://jbcdn2.b0.upaiyun.com/2012/04/line_moves2.jpg" rel="lightbox[18339]" title="简明 Vim 练级攻略"><img class="aligncenter size-full wp-image-18347" title="简明 Vim 练级攻略" alt="简明 Vim 练级攻略" src="http://jbcdn2.b0.upaiyun.com/2012/04/line_moves2.jpg" width="407" height="84" /></a></p>
<p>还有一个很有用的命令是 <code>dt"</code> → 删除所有的内容，直到遇到双引号—— <code>"。</code></p>
<p id="zone-selection-actionaobject-or-actioniobject"><strong>区域选择 <code>&lt;action&gt;a&lt;object&gt;</code> 或 <code>&lt;action&gt;i&lt;object&gt;</code></strong></p>
<p>在visual 模式下，这些命令很强大，其命令格式为</p>
<p><code>&lt;action&gt;a&lt;object&gt;</code> 和 <code>&lt;action&gt;i&lt;object&gt;</code></p>
<p><code></code>● action可以是任何的命令，如 <code>d</code> (删除), <code>y</code> (拷贝), <code>v</code> (可以视模式选择)。</p>
<p>● object 可能是： <code>w</code> 一个单词， <code>W</code> 一个以空格为分隔的单词， <code>s</code> 一个句字， <code>p</code> 一个段落。也可以是一个特别的字符：<code>"、</code> <code>'、</code> <code>)、</code> <code>}、</code> <code>]。</code></p>
<p>假设你有一个字符串 <code>(map (+) ("foo"))</code>.而光标键在第一个 <code>o </code>的位置。</p>
<p><code>● vi"</code> → 会选择 <code>foo</code>.</p>
<p><code>● va"</code> → 会选择 <code>"foo"</code>.</p>
<p><code>● vi)</code> → 会选择 <code>"foo"</code>.</p>
<p><code>● va)</code> → 会选择<code>("foo")</code>.</p>
<p><code>● v2i)</code> → 会选择 <code>map (+) ("foo")</code></p>
<p><code></code><code>● v2a)</code> → 会选择 <code>(map (+) ("foo"))</code></p>
<p><a href="http://jbcdn2.b0.upaiyun.com/2012/04/textobjects3.png" rel="lightbox[18339]" title="简明 Vim 练级攻略"><img class="aligncenter size-full wp-image-18351" title="简明 Vim 练级攻略" alt="简明 Vim 练级攻略" src="http://jbcdn2.b0.upaiyun.com/2012/04/textobjects3.png" width="242" height="126" /></a></p>
<p id="select-rectangular-blocks-c-v"><strong>块操作: <code>&lt;C-v&gt;</code></strong></p>
<p>块操作，典型的操作： <code>0 &lt;C-v&gt; &lt;C-d&gt; I-- [ESC]</code></p>
<p><code></code><code>● ^</code> → 到行头</p>
<p><code>● &lt;C-v&gt;</code> → 开始块操作</p>
<p><code>● &lt;C-d&gt;</code> → 向下移动 (你也可以使用hjkl来移动光标，或是使用%，或是别的)</p>
<p><code>● I-- [ESC]</code> → I是插入，插入“<code>--</code>”，按ESC键来为每一行生效。</p>
<p><a href="http://jbcdn2.b0.upaiyun.com/2012/04/rectangular-blocks4.gif" rel="lightbox[18339]" title="简明 Vim 练级攻略"><img class="aligncenter size-full wp-image-18349" title="简明 Vim 练级攻略" alt="简明 Vim 练级攻略" src="http://jbcdn2.b0.upaiyun.com/2012/04/rectangular-blocks4.gif" width="300" height="160" /></a></p>
<p>在Windows下的vim，你需要使用 <code>&lt;C-q&gt;</code> 而不是 <code>&lt;C-v&gt;</code> ，<code>&lt;C-v&gt;</code> 是拷贝剪贴板。</p>
<p id="completion-c-n-and-c-p"><strong>自动提示： <code>&lt;C-n&gt;</code> 和 <code>&lt;C-p&gt;</code></strong></p>
<p>在 Insert 模式下，你可以输入一个词的开头，然后按 <code>&lt;C-p&gt;或是&lt;C-n&gt;，自动补齐功能就出现了……</code></p>
<p><code></code><a href="http://jbcdn2.b0.upaiyun.com/2012/04/completion5.gif" rel="lightbox[18339]" title="简明 Vim 练级攻略"><img class="aligncenter size-full wp-image-18346" title="简明 Vim 练级攻略" alt="简明 Vim 练级攻略" src="http://jbcdn2.b0.upaiyun.com/2012/04/completion5.gif" width="433" height="180" /></a></p>
<p id="macros--qa-do-something-q-a-"><strong>宏录制： <code>qa</code> 操作序列 <code>q</code>, <code>@a</code>, <code>@@</code></strong></p>
<p><strong><code></code></strong><code>● qa</code> 把你的操作记录在寄存器 <code>a。</code></p>
<p><code></code>●  于是 <code>@a</code> 会replay被录制的宏。</p>
<p><code>● @@</code> 是一个快捷键用来replay最新录制的宏。</p>
<p><strong>示例</strong></p>
<p><strong></strong>在一个只有一行且这一行只有“1”的文本中，键入如下命令：</p>
<p><code>● qaYp&lt;C-a&gt;q</code>→</p>
<p style="padding-left: 30px;"><code>● qa</code> 开始录制</p>
<p style="padding-left: 30px;"><code>● Yp</code> 复制行.</p>
<p style="padding-left: 30px;"><code>● &lt;C-a&gt;</code> 增加1.</p>
<p style="padding-left: 30px;"><code>● q</code> 停止录制.</p>
<p><code>● @a</code> → 在1下面写下 2</p>
<p><code>● @@</code> → 在2 正面写下3</p>
<p>● 现在做 <code>100@@</code> 会创建新的100行，并把数据增加到 103.</p>
<p><a href="http://jbcdn2.b0.upaiyun.com/2012/04/macros6.gif" rel="lightbox[18339]" title="简明 Vim 练级攻略"><img class="aligncenter size-full wp-image-18348" title="简明 Vim 练级攻略" alt="简明 Vim 练级攻略" src="http://jbcdn2.b0.upaiyun.com/2012/04/macros6.gif" width="400" height="180" /></a></p>
<p id="visual-selection-vvc-v"><strong>可视化选择： <code>v</code>,<code>V</code>,<code>&lt;C-v&gt;</code></strong></p>
<p>前面，我们看到了 <code>&lt;C-v&gt;</code>的示例 （在Windows下应该是&lt;C-q&gt;），我们可以使用 <code>v</code> 和 <code>V</code>。一但被选好了，你可以做下面的事：</p>
<p><code>● J</code> → 把所有的行连接起来（变成一行）</p>
<p><code>● &lt;</code> 或 <code>&gt;</code> → 左右缩进</p>
<p><code>● =</code> → 自动给缩进 （陈皓注：这个功能相当强大，我太喜欢了）</p>
<p><a href="http://jbcdn2.b0.upaiyun.com/2012/04/autoindent7.gif" rel="lightbox[18339]" title="简明 Vim 练级攻略"><img class="aligncenter size-full wp-image-18345" title="简明 Vim 练级攻略" alt="简明 Vim 练级攻略" src="http://jbcdn2.b0.upaiyun.com/2012/04/autoindent7.gif" width="433" height="180" /></a></p>
<p>在所有被选择的行后加上点东西：</p>
<p>● &lt;C-v&gt;</p>
<p>● 选中相关的行 (可使用 <code>j</code> 或 <code>&lt;C-d&gt;</code> 或是 <code>/pattern</code> 或是 <code>%</code> 等……)</p>
<p><code>● $</code> 到行最后</p>
<p><code>● A</code>, 输入字符串，按 <code>ESC。</code></p>
<p><a href="http://jbcdn2.b0.upaiyun.com/2012/04/append-to-many-lines8.gif" rel="lightbox[18339]" title="简明 Vim 练级攻略"><img class="aligncenter size-full wp-image-18344" title="简明 Vim 练级攻略" alt="简明 Vim 练级攻略" src="http://jbcdn2.b0.upaiyun.com/2012/04/append-to-many-lines8.gif" width="433" height="180" /></a></p>
<h5 id="splits-split-and-vsplit">分屏: <code>:split</code> 和 <code>vsplit</code>.</h5>
<p>下面是主要的命令，你可以使用VIM的帮助 <code>:help split</code>. 你可以参考本站以前的一篇文章<a title="Vim的分屏功能" href="http://coolshell.cn/articles/1679.html" target="_blank">VIM分屏</a>。</p>
<p><code>● :split</code> → 创建分屏 (<code>:vsplit</code>创建垂直分屏)</p>
<p><code>● &lt;C-w&gt;&lt;dir&gt;</code> : dir就是方向，可以是 <code>hjkl</code> 或是 ←↓↑→ 中的一个，其用来切换分屏。</p>
<p><code>● &lt;C-w&gt;_</code> (或 <code>&lt;C-w&gt;|</code>) : 最大化尺寸 (&lt;C-w&gt;| 垂直分屏)</p>
<p><code>● &lt;C-w&gt;+</code> (或 <code>&lt;C-w&gt;-</code>) : 增加尺寸</p>
<p><a href="http://jbcdn2.b0.upaiyun.com/2012/04/split9.gif" rel="lightbox[18339]" title="简明 Vim 练级攻略"><img class="aligncenter size-full wp-image-18350" title="简明 Vim 练级攻略" alt="简明 Vim 练级攻略" src="http://jbcdn2.b0.upaiyun.com/2012/04/split9.gif" width="433" height="180" /></a></p>
<h4 id="conclusion"></h4>
<p><strong>结束语</strong></p>
<p>● 上面是作者最常用的90%的命令。</p>
<p>● 我建议你每天都学1到2个新的命令。</p>
<p>● 在两到三周后，你会感到vim的强大的。</p>
<p>● 有时候，学习VIM就像是在死背一些东西。</p>
<p>● 幸运的是，vim有很多很不错的工具和优秀的文档。</p>
<p>● 运行vimtutor直到你熟悉了那些基本命令。</p>
<p>● 其在线帮助文档中你应该要仔细阅读的是 <code>:help usr_02.txt</code>.</p>
<p>● 你会学习到诸如  <code>!，</code> 目录，寄存器，插件等很多其它的功能。</p>
<p>学习vim就像学弹钢琴一样，一旦学会，受益无穷。</p>
<p id="aeaoofnhgocdbnbeljkmbjdmhbcokfdb-mousedown">——————————正文结束——————————</p>
<p>对于vi/vim只是点评一点：这是一个你不需要使用鼠标，不需使用小键盘，只需要使用大键盘就可以完成很多复杂功能文本编辑的编辑器。不然，Visual Studio也不就会有vim的插件了。</p>