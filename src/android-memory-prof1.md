<div style="text-align:justify"><span style="font-family:SimSun; font-size:14px">RAM（random access memory）随机存取存储器。说白了就是内存。</span></div>
<p style="margin-top:10px; margin-bottom:10px; padding-top:0px; padding-bottom:0px">
<span style="font-family:SimSun; font-size:14px">一般Java在内存分配时会涉及到以下区域：</span></p>
<p style="margin-top:10px; margin-bottom:10px; padding-top:0px; padding-bottom:0px; color:rgb(51,51,51); text-indent:28px; line-height:28px; background-color:rgb(248,248,248)">
</p>
<p style="margin-top:10px; margin-bottom:10px; padding-top:0px; padding-bottom:0px">
<span style="font-family:SimSun; font-size:14px"><strong>寄存器（Registers）：</strong><span style="line-height:25.200000762939453px">速度最快的存储场所，因为寄存器位于处理器内部<strong>，</strong></span>我们在程序中无法控制</span></p>
<p style="margin-top:10px; margin-bottom:10px; padding-top:0px; padding-bottom:0px">
<span style="font-family:SimSun; font-size:14px"><strong>栈（Stack）：</strong>存放基本类型的数据和对象的引用，但对象本身不存放在栈中，而是存放在堆中</span></p>
<p style="margin-top:10px; margin-bottom:10px; padding-top:0px; padding-bottom:0px">
<span style="font-family:SimSun; font-size:14px"><strong>堆（Heap）：</strong>堆内存用来存放由new创建的对象和数组。在堆中分配的内存，由Java虚拟机的自动垃圾回收器（GC）来管理。</span></p>
<p style="margin-top:10px; margin-bottom:10px; padding-top:0px; padding-bottom:0px">
<span style="font-family:SimSun; font-size:14px"><strong>静态域（static field）：</strong><span style="line-height:25.200000762939453px">&nbsp; 静态存储区域就是指在固定的位置存放应用程序运行时一直存在的数据，Java在内存中专门划分了一个静态存储区域来管理一些特殊的数据变量如静态的数据变量</span></span></p>
<p style="margin-top:10px; margin-bottom:10px; padding-top:0px; padding-bottom:0px">
<span style="font-family:SimSun; font-size:14px"><strong>常量池（constant pool）：</strong><span style="line-height:25.200000762939453px">虚拟机必须为每个被装载的类型维护一个常量池。常量池就是该类型所用到常量的一个有序集和，包括直接常量（string,integer和floating point常量）和对其他类型，字段和方法的符号引用。</span></span></p>
<p style="margin-top:10px; margin-bottom:10px; padding-top:0px; padding-bottom:0px">
<span style="font-family:SimSun; font-size:14px"><strong>非RAM存储：</strong>硬盘等永久存储空间</span></p>
<p style="color:rgb(51,51,51); line-height:26px"><span style="font-family:SimSun; font-size:14px"></span></p>
<p><strong><span style="font-family:Microsoft YaHei; font-size:18px; color:#cc6600"><br>
</span></strong></p>
<p><strong><span style="font-family:Microsoft YaHei; font-size:18px; color:#cc6600">堆栈特点对比：</span></strong></p>
<p><span style="text-indent:28px"><span style="font-family:SimSun; font-size:14px">由于篇幅原因，下面只简单的介绍一下堆栈的一些特性。<br>
</span></span></p>
<p><span style="font-family:SimSun; font-size:14px"><span style="text-indent:28px"><span style="color:#6666cc"><strong>栈</strong>：当定义一个变量时，Java就在栈中为这个变量分配内存空间，当该变量退出该作用域后，Java会自动释放掉为该变量所分配的内存空间，该内存空间可以立即被另作他用。</span></span><br>
</span></p>
<p><span style="color:#663333"><span style="font-family:SimSun; font-size:14px"><strong>堆</strong>：当堆中的new产生数组和对象超出其作用域后，它们不会被释放，只有在没有引用变量指向它们的时候才变成垃圾，不能再被使用。即使这样，所占内存也不会立即释放，而是等待被垃圾回收器收走。这也是Java比较占内存的原因。</span></span></p>
<p><span style="font-family:SimSun; font-size:14px"><br>
</span></p>
<p><span style="color:#6666cc"><span style="font-family:SimSun; font-size:14px"><strong>栈</strong>：<span style="text-indent:28px">存取速度比堆要快，仅次于寄存器。</span><span style="text-indent:28px">但缺点是，存在栈中的数据大小与生存期必须是确定的，缺乏灵活性。</span></span></span></p>
<p><span style="text-indent:28px"><span style="color:#663333"><span style="font-family:SimSun; font-size:14px"><strong>堆</strong>：<span style="text-indent:28px">堆是一个运行时数据区，<span style="text-indent:28px">可以动态地分配内存大小，因此<span style="text-indent:28px">存取速度较慢。</span>也正因为这个特点，堆的<span style="text-indent:28px">生存期不必事先告诉编译器，而且</span>Java的垃圾收集器会自动收走这些不再使用的数据。</span></span></span></span></span></p>
<p><span style="font-family:SimSun; font-size:14px"><br>
</span></p>
<p><span style="text-indent:28px"><span style="text-indent:28px"><span style="text-indent:28px"><span style="color:#6666cc"><span style="font-family:SimSun; font-size:14px"><strong>栈</strong>：<span style="text-indent:28px">栈中的数据可以共享，<span style="text-indent:28px">&nbsp;它是由编译器完成的，有利于节省空间。</span></span></span></span></span></span></span></p>
<p><span style="text-indent:28px"><span style="text-indent:28px"><span style="text-indent:28px"><span style="text-indent:28px"><span style="color:#6666cc"><span style="font-family:SimSun; font-size:14px">例如：需要定义两个变量<span style="text-indent:28px">int a = 3；</span><span style="text-indent:28px">int
 b = 3；</span></span></span></span></span></span></span></p>
<p><span style="color:#6666cc"><span style="font-family:SimSun; font-size:14px"><span style="text-indent:28px"><span style="text-indent:28px"><span style="text-indent:28px"><span style="text-indent:28px"></span></span></span></span><span style="text-indent:28px">编译器先处理int
 a = 3；首先它会在栈中创建一个变量为a的引用，然后查找栈中是否有3这个&#20540;，如果没找到，就将3存放进来，然后将a指向3。接着处理int b = 3；在创建完b的引用变量后，因为在栈中已经有3这个&#20540;，便将b直接指向3。这样，就出现了a与b同时均指向3的情况。这时，如果再</span>让<span style="text-indent:28px">a=4；那么编译器会重新搜索栈中是否有4&#20540;，如果没有，则将4存放进来，并让a指向4；如果已经有了，则直接将a指向这个地址。因此a&#20540;的改变不会影响到b的&#20540;。</span></span></span></p>
<p><span style="text-indent:28px"><span style="text-indent:28px"><span style="color:#663333"><span style="font-family:SimSun; font-size:14px"><strong>堆</strong>：<span style="text-indent:28px">例如上面栈中a的修改并不会影响到b, 而在堆中一个对象引用变量修改了这个对象的内部状态，会影响到另一个对象引用变量。</span></span></span></span></span></p>
<p><span style="text-indent:28px"><br>
</span></p>
<p><span style="text-indent:28px"><span style="font-family:Microsoft YaHei; font-size:18px; color:#cc6600"><strong>内存耗用名词解析：</strong></span></span></p>
<p><span style="text-indent:28px"><strong>VSS</strong></span><span style="text-indent:28px"><strong>&nbsp;</strong>- Virtual Set Size 虚拟耗用内存（包含共享库占用的内存）</span></p>
<p><span style="text-indent:28px"><strong>RSS</strong></span><span style="text-indent:28px"><strong>&nbsp;</strong>- Resident Set Size 实际使用物理内存（包含共享库占用的内存）</span></p>
<p><span style="text-indent:28px"><strong>PSS</strong></span><span style="text-indent:28px"><strong>&nbsp;</strong>- Proportional Set Size 实际使用的物理内存（比例分配共享库占用的内存）</span></p>
<p><span style="text-indent:28px"><strong>USS</strong></span><span style="text-indent:28px"><strong>&nbsp;</strong>- Unique Set Size 进程独自占用的物理内存（不包含共享库占用的内存）</span></p>
<p><span style="text-indent:28px">一般来说内存占用大小有如下规律：VSS &gt;= RSS &gt;= PSS &gt;= USS<br>
</span></p>
<p><span style="text-indent:28px"><span style="text-indent:28px"><br>
</span></span></p>
<p><span style="text-indent:28px"><span style="text-indent:28px"></span></span></p>
<p><strong><span style="font-family:Microsoft YaHei; font-size:24px; color:#cc0000">OOM：</span></strong></p>
<p><br>
</p>
<p><span style="font-family:Microsoft YaHei; font-size:18px; color:#cc6600"><strong>内存泄露可以引发很多的问题：</strong></span></p>
<p>1.程序卡顿，响应速度慢（内存占用高时JVM虚拟机会频繁触发GC）</p>
<p>2.莫名消失（当你的程序所占内存越大，它在后台的时候就越可能被干掉。反之内存占用越小，在后台存在的时间就越长）</p>
<p>3.直接崩溃（OutOfMemoryError）</p>
<p style="border-width:0px; padding-top:0px; padding-bottom:0px; margin-top:0px; margin-bottom:8px; list-style:none">
<br>
</p>
<p><span style="font-family:Microsoft YaHei; font-size:18px; color:#cc6600"><strong>ANDROID内存面临的问题：</strong></span></p>
<p></p>
<p>1.有限的堆内存，原始只有16M</p>
<p>2.内存大小消耗等根据设备，操作系统等级，屏幕尺寸的不同而不同</p>
<p>3.程序不能直接控制</p>
<p>4.支持后台多任务处理（multitasking）</p>
<p>5.运行在虚拟机之上</p>
<p></p>
<p><span style="font-family:Microsoft YaHei; font-size:24px; color:#cc0000"><strong><br>
</strong></span></p>
<p><span style="font-family:Microsoft YaHei; font-size:24px; color:#cc0000"><strong></strong></span></p>
<p><span style="font-family:'Microsoft YaHei'; font-size:24px; color:rgb(204,0,0)"><strong>5R：</strong></span></p>
<p><span style="font-family:'Microsoft YaHei'; font-size:14px">本文主要通过如下的5R方法来对ANDROID内存进行优化：</span></p>
<p><strong><span style="font-family:'Microsoft YaHei'; font-size:18px; color:rgb(204,102,0)">1.Reckon（计算）</span></strong></p>
<p><span style="font-size:14px"><span style="font-family:'Microsoft YaHei'"><span style="white-space:pre"></span>首先需要知道你的app所消耗内存的情况，知己知彼才能百战不殆</span></span></p>
<p><strong><span style="font-family:'Microsoft YaHei'; font-size:18px; color:rgb(204,102,0)">2.Reduce（减少）</span></strong></p>
<p><span style="font-size:14px"><span style="font-family:'Microsoft YaHei'"><span style="white-space:pre"></span>消耗更少的资源</span></span></p>
<p><span style="font-size:14px"></span></p>
<p><strong><span style="font-family:'Microsoft YaHei'; font-size:18px; color:rgb(204,102,0)">3.Reuse（重用）</span></strong></p>
<p><span style="font-size:14px"><span style="font-family:'Microsoft YaHei'"><span style="white-space:pre"></span>当第一次使用完以后，尽量给其他的使用</span></span></p>
<p></p>
<p><strong><span style="font-family:'Microsoft YaHei'; font-size:18px; color:rgb(204,102,0)">5.Recycle（回收）</span></strong></p>
<p><span style="font-size:14px"><span style="font-family:'Microsoft YaHei'"><span style="white-space:pre"></span>返回资源</span></span></p>
<p><strong><span style="font-family:'Microsoft YaHei'"><span style="font-size:18px; color:rgb(204,102,0)">4.Review（检查）</span><br>
</span></strong></p>
<p><span style="white-space:pre"><span style="font-size:14px"><span style="font-family:'Microsoft YaHei'">回顾检查你的程序，看看设计或代码有什么不合理的地方。</span></span></span></p>
<p><span style="font-size:14px"><span style="font-family:Microsoft YaHei"></span></span></p>
<p><br>
</p>
<p><strong><span style="color:rgb(204,0,0)"><span style="font-family:Microsoft YaHei; font-size:24px">Reckon （计算）：</span></span></strong></p>
<p><span style="font-family:SimSun; font-size:14px">了解自己应用的内存使用情况是很有必要的。如果当内存使用过高的话就需要对其进行优化，因为更少的使用内存可以减少ANDROID系统终止我们的进程的几率，也可以提高多任务执行效率和体验效果。</span></p>
<p><span style="font-family:SimSun; font-size:14px">下面从系统内存（system ram）和堆内存(heap)两个方面介绍一些查看和计算内存使用情况的方法：</span></p>
<p><span style="font-family:SimSun; font-size:14px"><br>
</span></p>
<p><strong><span style="font-family:'Microsoft YaHei'; font-size:18px"><span style="color:#cc6600">System Ram(系统内存)：</span></span></strong><br>
</p>
<p>观察和计算系统内存使用情况，可以使用Android提供给我们的两个工具<span style="text-indent:16px"><strong>procstats</strong>，<strong>meminfo</strong></span>。他们一个侧重于后台的内存使用，另一个是运行时的内存使用。</p>
<p></p>
<h1 style="margin:0px 0px 5px; line-height:1.5em; color:rgb(51,51,51)"><span style="font-family:Microsoft YaHei; font-size:18px">Process Stats:&nbsp;</span></h1>
<div><span style="text-indent:16px">Android 4.4 KitKat&nbsp;提出了一个新系统服务，叫做procstats。它将帮助你更好的理解你app在后台（background）时的内存使用情况。</span></div>
<div><span style="text-indent:16px">Procstats可以去监视你app在一段时间的行为，包括在后台运行了多久，并在此段时间使用了多少内存。从而帮助你快速的找到应用中不效率和不规范的地方去避免影响其performs，尤其是在低内存的设备上运行时。</span></div>
<div><span style="text-indent:16px">你可以通过adb shell命令去<span style="text-indent:16px">使用procstats（<span style="color:#ff0000; background-color:rgb(255,255,204)">adb shell dumpsys procstats --hours 3</span>）</span>，或者更方便的方式是运行Process Stats开发者工具（在4.4版本的手机中点击<span style="text-indent:16px">Settings
 &gt; Developer options &gt; Process Stats</span>）</span></div>
<div><span style="text-indent:16px"><span style="font-family:SimSun; font-size:14px"><img src="http://img.blog.csdn.net/20140811223043097?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYTM5NjkwMTk5MA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" alt=""><br>
</span></span></div>
<div><span style="text-indent:16px">点击单个条目还可以查看详细信息</span></div>
<div><span style="font-family:SimSun; font-size:14px"><br>
</span></div>
<div><strong><span style="font-size:18px">meminfo：</span></strong><br>
</div>
<div>Android还提供了一个工具叫做meminfo。<span style="text-indent:16px">它是根据PSS标准 （Proportional Set Size——实际物理内存）计算每个进程的内存使用并且按照重要程度排序。</span></div>
<div>你可以通过命令行去执行它：（<span style="color:#ff0000; background-color:rgb(255,255,153)">adb shell dumpsys meminfo</span>）或者使用在设备上点击Settings &gt; Apps &gt; Running（与Procstats不用，它也可以在老版本上运行）</div>
<div><span style="line-height:26px"><span style="font-family:SimSun; font-size:14px"><img src="http://img.blog.csdn.net/20140811223419281?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYTM5NjkwMTk5MA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" alt=""><br>
</span></span></div>
<div><span style="line-height:26px"><span style="font-family:SimSun; font-size:14px">更多关于<span style="text-indent:16px"><strong>Procstats</strong>和<strong>meninfo</strong></span>的介绍可以参考我翻译的一篇文章：<a target="_blank" target="_blank" href="http://blog.csdn.net/a396901990/article/details/38390135">Process
 Stats:了解你的APP如何使用内存</a></span></span></div>
<p><span style="font-family:SimSun; font-size:14px"><br>
</span></p>
<p><span style="font-family:'Microsoft YaHei'; font-size:18px"><strong><span style="color:#cc6600">Heap（堆内存）：</span></strong></span></p>
<p><span style="text-indent:2em">在程序中可以使用如下的方法去查询内存使用情况</span></p>
<p><span style="text-indent:2em"><br>
</span></p>
<p><span style="text-indent:2em"><strong><span style="font-size:18px">ActivityManager#getMemoryClass()</span></strong></span></p>
<p><span style="text-indent:2em">查询可用堆内存的限制</span></p>
<p><span style="text-indent:2em">3.0(HoneyComb)以上的版本可以通过largeHeap=“true”来申请更多的堆内存（不过这算作“作弊”）</span></p>
<p><span style="font-family:SimSun; font-size:14px"><br>
</span></p>
<p></p>
<div><span style="font-size:18px"><strong>ActivityManager#getMemoryInfo(ActivityManager.MemoryInfo)</strong></span></div>
<div>得到的MemoryInfo中可以查看如下Field的属性：<br>
</div>
<div>availMem:表示系统剩余内存</div>
<div>lowMemory：它是boolean&#20540;，表示系统是否处于低内存运行</div>
<div>hreshold：它表示当系统剩余内存低于好多时就看成低内存运行</div>
<br>
<p></p>
<p><strong><span style="font-size:18px"><span style="word-wrap:normal; word-break:normal">android.os.Debug#</span><span style="word-wrap:normal; word-break:normal">getMemoryInfo(Debug.MemoryInfo memoryInfo</span><span style="word-wrap:normal; word-break:normal">)</span></span></strong></p>
<p>得到的MemoryInfo中可以查看如下Field的属性：</p>
<p></p>
<div style="color:rgb(54,46,43); font-family:Arial,sans-serif,Helvetica,Tahoma; font-size:14px; line-height:21px">
<span style="word-wrap:normal; word-break:normal">dalvikPrivateDirty</span><span style="word-wrap:normal; word-break:normal; color:rgb(0,51,102)">：&nbsp;<wbr></span><span style="word-wrap:normal; word-break:normal; color:rgb(0,0,128)">The private dirty pages used
 by dalvik。</span></div>
<div style="color:rgb(54,46,43); font-family:Arial,sans-serif,Helvetica,Tahoma; font-size:14px; line-height:21px">
<span style="word-wrap:normal; word-break:normal">dalvikPss</span><span style="word-wrap:normal; word-break:normal; color:rgb(0,51,102)">&nbsp;<wbr>：</span><span style="word-wrap:normal; word-break:normal; color:rgb(0,0,128)">The proportional set size for dalvik.</span></div>
<div style="color:rgb(54,46,43); font-family:Arial,sans-serif,Helvetica,Tahoma; font-size:14px; line-height:21px">
<span style="word-wrap:normal; word-break:normal">dalvikSharedDirty&nbsp;<wbr></span><span style="word-wrap:normal; word-break:normal; color:rgb(0,51,102)">：</span><span style="word-wrap:normal; word-break:normal; color:rgb(0,0,128)">The shared dirty pages used
 by dalvik.</span></div>
<div style="color:rgb(54,46,43); font-family:Arial,sans-serif,Helvetica,Tahoma; font-size:14px; line-height:21px">
<span style="word-wrap:normal; word-break:normal">nativePrivateDirty</span>&nbsp;<wbr><span style="word-wrap:normal; word-break:normal">：</span><span style="word-wrap:normal; word-break:normal; color:rgb(0,0,128)">The private dirty pages used by the&nbsp;<wbr></span><span style="word-wrap:normal; word-break:normal; color:rgb(255,102,0)">native
 heap</span><span style="word-wrap:normal; word-break:normal; color:rgb(153,51,0)">.</span></div>
<div style="color:rgb(54,46,43); font-family:Arial,sans-serif,Helvetica,Tahoma; font-size:14px; line-height:21px">
<span style="word-wrap:normal; word-break:normal">nativePss</span>&nbsp;<wbr><span style="word-wrap:normal; word-break:normal">：</span><span style="word-wrap:normal; word-break:normal; color:rgb(0,0,128)">The proportional set size for the native heap.</span></div>
<div style="color:rgb(54,46,43); font-family:Arial,sans-serif,Helvetica,Tahoma; font-size:14px; line-height:21px">
<span style="word-wrap:normal; word-break:normal">nativeSharedDirty</span><span style="word-wrap:normal; word-break:normal; color:rgb(0,51,102)">&nbsp;<wbr>：</span><span style="word-wrap:normal; word-break:normal; color:rgb(0,0,128)">The shared dirty pages used
 by the&nbsp;<wbr></span><span style="word-wrap:normal; word-break:normal; color:rgb(255,102,0)">native heap.</span></div>
<div style="color:rgb(54,46,43); font-family:Arial,sans-serif,Helvetica,Tahoma; font-size:14px; line-height:21px">
<span style="word-wrap:normal; word-break:normal">otherPrivateDirty</span>&nbsp;<wbr><span style="word-wrap:normal; word-break:normal">：</span><span style="word-wrap:normal; word-break:normal; color:rgb(0,0,128)">The private dirty pages used by everything else.</span></div>
<div style="color:rgb(54,46,43); font-family:Arial,sans-serif,Helvetica,Tahoma; font-size:14px; line-height:21px">
<span style="word-wrap:normal; word-break:normal">otherPss</span><span style="word-wrap:normal; word-break:normal; color:rgb(0,51,102)">&nbsp;<wbr>：</span><span style="word-wrap:normal; word-break:normal; color:rgb(0,0,128)">The proportional set size for everything
 else.</span></div>
<div style="color:rgb(54,46,43); font-family:Arial,sans-serif,Helvetica,Tahoma; font-size:14px; line-height:21px">
<span style="word-wrap:normal; word-break:normal">otherSharedDirty</span><span style="word-wrap:normal; word-break:normal; color:rgb(0,51,102)">&nbsp;<wbr>：</span><span style="word-wrap:normal; word-break:normal; color:rgb(0,0,128)">The shared dirty pages used by
 everything else.</span></div>
<p></p>
<p><span style="word-wrap:normal; word-break:normal"></span></p>
<div><span style="word-wrap:normal; word-break:normal"></span></div>
<div><span style="word-wrap:normal; word-break:normal"><span style="word-wrap:normal; word-break:normal"><span style="word-wrap:normal; word-break:normal">dalvik</span><span style="word-wrap:normal; word-break:normal">：</span><span style="word-wrap:normal; word-break:normal">是指</span></span></span><span style="word-wrap:normal; word-break:normal"><span style="word-wrap:normal; word-break:normal"><span style="word-wrap:normal; word-break:normal">dalvik所使用的内存</span><span style="word-wrap:normal; word-break:normal">。</span></span></span></div>
<div><span style="word-wrap:normal; word-break:normal"><span style="word-wrap:normal; word-break:normal"><span style="word-wrap:normal; word-break:normal">native</span><span style="word-wrap:normal; word-break:normal">：</span><span style="word-wrap:normal; word-break:normal">是被native堆使用的内存。应该指使用C\C&#43;&#43;在堆上分配的</span></span></span><span style="word-wrap:normal; word-break:normal"><span style="word-wrap:normal; word-break:normal">内存</span><span style="word-wrap:normal; word-break:normal">。</span></span></div>
<div><span style="word-wrap:normal; word-break:normal"><span style="word-wrap:normal; word-break:normal">other:</span><span style="word-wrap:normal; word-break:normal">是指除</span></span><span style="word-wrap:normal; word-break:normal">dalvik和</span><span style="word-wrap:normal; word-break:normal"><span style="word-wrap:normal; word-break:normal">native使用的内存。但是具体是指什么呢？至少包括在C\C&#43;&#43;分配的非堆内存，比如分配在栈上的内存。</span></span></div>
<div><span style="word-wrap:normal; word-break:normal"><span style="word-wrap:normal; word-break:normal"><span style="word-wrap:normal; word-break:normal">private</span>:</span><span style="word-wrap:normal; word-break:normal">是指私有的。非共享的。</span></span></div>
<div><span style="word-wrap:normal; word-break:normal"><span style="word-wrap:normal; word-break:normal"><span style="word-wrap:normal; word-break:normal">share</span>:</span><span style="word-wrap:normal; word-break:normal">是指共享的内存</span><span style="word-wrap:normal; word-break:normal">。</span></span></div>
<div><span style="word-wrap:normal; word-break:normal"><span style="word-wrap:normal; word-break:normal">PSS</span>：</span><span style="word-wrap:normal; word-break:normal"><span style="word-wrap:normal; word-break:normal">实际使用的物理内存（比例分配共享库占用的内存）</span></span></div>
<div>
<div><span style="word-wrap:normal; word-break:normal"><span style="word-wrap:normal; word-break:normal"><span style="word-wrap:normal; word-break:normal">&nbsp;<wbr>PrivateDirty</span></span>：<span style="word-wrap:normal; word-break:normal">它是指非共享的，又不能换页出去（</span></span><span style="word-wrap:normal; word-break:normal"><span style="word-wrap:normal; word-break:normal">can
 not be paged to disk&nbsp;<wbr></span><span style="word-wrap:normal; word-break:normal">）的内存的大小。比如Linux为了提高分配内存速度而缓冲的小对象，即使你的进程结束，该内存也不会释放掉，它只是又重新回到缓冲中而已。</span></span></div>
<div><span style="word-wrap:normal; word-break:normal"><span style="word-wrap:normal; word-break:normal">SharedDirty:参照</span></span><span style="word-wrap:normal; word-break:normal"><span style="word-wrap:normal; word-break:normal">PrivateDirty</span><span style="word-wrap:normal; word-break:normal">我认为</span></span><span style="word-wrap:normal; word-break:normal"><span style="word-wrap:normal; word-break:normal">它应该是指共享的，又不能换页出去（</span></span><span style="word-wrap:normal; word-break:normal"><span style="word-wrap:normal; word-break:normal">can
 not be paged to disk&nbsp;<wbr></span><span style="word-wrap:normal; word-break:normal">）的内存的大小。比如Linux为了提高分配内存速度而缓冲的小对象，即使所有共享它的进程结束，该内存也不会释放掉，它只是又重新回到缓冲中而已。</span></span></div>
</div>
<div><span style="word-wrap:normal; word-break:normal"><span style="word-wrap:normal; word-break:normal"><br>
</span></span></div>
<p></p>
<p><strong><span style="font-size:18px">android.os.Debug#getNativeHeapSize()</span></strong></p>
<p>返回的是当前进程navtive堆本身总的内存大小</p>
<p><strong><span style="font-size:18px">android.os.Debug#getNativeHeapAllocatedSize()</span></strong></p>
<p>返回的是当前进程navtive堆中已使用的内存大小</p>
<p><strong><span style="font-size:18px">android.os.Debug#getNativeHeapFreeSize()</span></strong><br>
</p>
<p>返回的是当前进程navtive堆中已经剩余的内存大小</p>
<p><br>
</p>
<p></p>
<p><strong><span style="font-size:18px">Memory Analysis Tool（MAT）：</span></strong></p>
<p>通常内存泄露分析被认为是一件很有难度的工作，一般由团队中的资深人士进行。不过，今天我们要介绍的 MAT（Eclipse Memory Analyzer）被认为是一个“傻瓜式“的堆转储文件分析工具，你只需要轻轻点击一下鼠标就可以生成一个专业的分析报告。</p>
<p>如下图：<br>
</p>
<p><img src="http://img.blog.csdn.net/20140814225714406?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYTM5NjkwMTk5MA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" alt=""><br>
</p>
<p>关于详细的MAT使用我推荐下面这篇文章：<a target="_blank" target="_blank" href="http://www.ibm.com/developerworks/cn/opensource/os-cn-ecl-ma/index.html">使用 Eclipse Memory Analyzer 进行堆转储文件分析</a></p>
<p><br>
</p>
<p><br>
</p>
<p><span style="font-family:Microsoft YaHei; font-size:24px; color:#cc0000"><strong>写在最后：</strong></span></p>
<p><span style="font-size:18px">我准备将文章分为上、中、下三部分。现在已经全部完成：</span></p>
<p><span style="font-family:Microsoft YaHei; font-size:18px"><strong>内存简介，Recoken（计算）</strong></span><span style="font-size:18px">请看</span><span style="font-family:Microsoft YaHei"><strong><span style="font-size:18px">：</span><a target="_blank" target="_blank" href="http://blog.csdn.net/a396901990/article/details/37914465" style="text-decoration:none; line-height:30px; font-family:'Microsoft YaHei'"><span style="font-size:18px; color:#ff0000">ANDROID内存优化(大汇总——上)</span></a></strong></span></p>
<p><span style="font-size:18px"><span style="font-family:Microsoft YaHei"><strong>Reduce（减少），Reuse（重用）&nbsp;</strong></span><span style="font-family:SimSun">请看：<a target="_blank" target="_blank" href="http://blog.csdn.net/a396901990/article/details/38707007" style="text-decoration:none; font-family:'Microsoft YaHei'; line-height:30px"><span style="color:#ff0000"><strong>ANDROID内存优化(大汇总——中)</strong></span></a></span></span></p>
<p><span style="font-family:Microsoft YaHei; font-size:18px"><strong>Recycle（回收）, Review（检查）&nbsp;</strong></span><span style="font-family:SimSun"><span style="font-size:18px">请看：</span><a target="_blank" href="http://blog.csdn.net/a396901990/article/details/38904543" style="text-decoration:none; font-family:'Microsoft YaHei'; line-height:30px"><span style="font-size:18px; color:#ff0000"><strong>ANDROID内存优化(大汇总——全)</strong></span></a></span></p>
<p><br>
</p>
<p><span style="font-family:SimSun"><span style="font-family:SimSun"><span style="font-size:18px">写这篇文章的目的就是想弄一个大汇总，将零散的内存知识点总结一下，<span style="font-family:SimSun">如果有错误、不足或建议都希望告诉我。</span></span></span><br>
</span></p>
<p><span style="font-weight:bold"><span style="color:rgb(204,0,0)"><span style="font-family:SimSun; font-size:14px"><span style="font-family:SimSun; font-size:14px"><br>
</span></span></span></span></p>
<p><span style="font-family:Microsoft YaHei; font-size:24px; color:#cc0000"><strong>参考文章：</strong></span></p>
<p><span style="font-family:Arial; font-size:14px">AnDevCon开发者大会演讲PPT：Putting Your App on a Memory Diet</span></p>
<p><span style="font-family:Arial; font-size:14px"><span style="text-align:center">深入Java核心 Java内存分配原理精讲(</span><span style="text-align:center">http://developer.51cto.com/art/201009/225071.htm</span><span style="text-align:center">)</span></span></p>
<p></p>
<h1 style="margin:0px 0px 5px"><span style="font-family:Arial; font-size:14px; font-weight:normal">Process Stats: Understanding How Your App Uses RAM（http://blog.csdn.net/a396901990/article/details/38390135）</span></h1>
<h1 style="margin:0px 0px 5px"><span style="font-weight:normal"><span style="font-family:Arial; font-size:14px">Android中如何查看内存（http://blog.csdn.net/hudashi/article/details/7050897）</span></span></h1>
<p><span style="font-family:Arial; font-size:14px"><span style="text-align:center">Android内存性能优化(内部资料总结)（</span>http://www.2cto.com/kf/201405/303276.html）</span></p>
<p><br>
</p>
</div>

