<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; color:rgb(102,51,51); font-family:'Microsoft YaHei'; font-size:18px; font-weight:bold">
<span style="font-size:18px; color:rgb(204,102,0)">内存泄露可以引发很多的问题：</span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
1.程序卡顿，响应速度慢（内存占用高时JVM虚拟机会频繁触发GC）</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
2.莫名消失（当你的程序所占内存越大，它在后台的时候就越可能被干掉。反之内存占用越小，在后台存在的时间就越长）</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
3.直接崩溃（OutOfMemoryError）</p>
<p style="margin-top:0px; margin-bottom:8px; padding-top:0px; padding-bottom:0px; color:rgb(102,51,51); font-family:'Microsoft YaHei'; font-size:18px; font-weight:bold; border-width:0px; list-style:none">
<br>
</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; color:rgb(102,51,51); font-family:'Microsoft YaHei'; font-size:18px; font-weight:bold">
<span style="font-size:18px; color:rgb(204,102,0)">ANDROID内存面临的问题：</span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; color:rgb(102,51,51); font-family:'Microsoft YaHei'; font-size:18px; font-weight:bold">
</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
1.有限的堆内存，原始只有16M</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
2.内存大小消耗等根据设备，操作系统等级，屏幕尺寸的不同而不同</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
3.程序不能直接控制</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
4.支持后台多任务处理（multitasking）</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
5.运行在虚拟机之上</p>
</div>
<div style="font-size:14px; line-height:26px; color:rgb(102,51,51); font-family:SimSun; font-weight:bold">
<span style="font-family:'Microsoft YaHei'; font-size:18px"><br>
</span></div>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; font-family:Arial; font-size:14px; line-height:26px">
<span style="font-family:'Microsoft YaHei'; font-size:24px; color:rgb(204,0,0)"><strong>5R：</strong></span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; font-family:Arial; font-size:14px; line-height:26px">
<span style="font-family:'Microsoft YaHei'; font-size:14px">本文主要通过如下的5R方法来对ANDROID内存进行优化：</span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; font-family:Arial; font-size:14px; line-height:26px">
<span style="font-family:'Microsoft YaHei'; font-size:14px"><br>
</span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; font-family:Arial; font-size:14px; line-height:26px">
<strong><span style="font-family:'Microsoft YaHei'; font-size:18px; color:rgb(204,102,0)">1.Reckon（计算）</span></strong></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; font-family:Arial; font-size:14px; line-height:26px">
<span style="font-size:14px"><span style="font-family:'Microsoft YaHei'"><span style="white-space:pre"></span>首先需要知道你的app所消耗内存的情况，知己知彼才能百战不殆</span></span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; font-family:Arial; font-size:14px; line-height:26px">
<strong><span style="font-family:'Microsoft YaHei'; font-size:18px; color:rgb(204,102,0)">2.Reduce（减少）</span></strong></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; font-family:Arial; font-size:14px; line-height:26px">
<span style="font-size:14px"><span style="font-family:'Microsoft YaHei'"><span style="white-space:pre"></span>消耗更少的资源</span></span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; font-family:Arial; font-size:14px; line-height:26px">
<span style="font-size:14px"><span style="font-family:'Microsoft YaHei'"></span></span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; font-family:Arial; font-size:14px; line-height:26px">
<strong><span style="font-family:'Microsoft YaHei'; font-size:18px; color:rgb(204,102,0)">3.Reuse（重用）</span></strong></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; font-family:Arial; font-size:14px; line-height:26px">
<span style="font-size:14px"><span style="font-family:'Microsoft YaHei'"><span style="white-space:pre"></span>当第一次使用完以后，尽量给其他的使用</span></span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; font-family:Arial; font-size:14px; line-height:26px">
<strong><span style="font-family:'Microsoft YaHei'; font-size:18px; color:rgb(204,102,0)">5.Recycle（回收）</span></strong></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; font-family:Arial; font-size:14px; line-height:26px">
<span style="font-size:14px"><span style="font-family:'Microsoft YaHei'"><span style="white-space:pre"></span>回收资源</span></span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; font-family:Arial; font-size:14px; line-height:26px">
<strong><span style="font-family:'Microsoft YaHei'"><span style="font-size:18px; color:rgb(204,102,0)">4.Review（检查）</span><br>
</span></strong></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; font-family:Arial; font-size:14px; line-height:26px">
<span style="white-space:pre"><span style="font-size:14px"><span style="font-family:'Microsoft YaHei'">回顾检查你的程序，看看设计或代码有什么不合理的地方。</span></span></span></p>
<div class="column" style="font-family:Arial; font-size:14px; line-height:26px"><br>
</div>
<br style="font-family:Arial; font-size:14px; line-height:26px">
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; line-height:26px; text-align:center">
<br>
</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; line-height:26px; text-align:center">
<strong><span style="color:rgb(204,0,0)"><span style="font-family:Microsoft YaHei; font-size:24px"><br>
</span></span></strong></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; font-family:Arial; line-height:26px; text-align:center">
<span style="font-weight:bold"><span style="color:rgb(204,0,0)"><span style="font-size:32px">
<span style="font-size:18px">Recycle（回收），回收可以说是在内存使用中最重要的部分。因为内存空间有限，无论你如何优化，如何节省内存总有用完的时候。而回收的意义就在于去清理和释放那些已经闲置，废弃不再使用的内存资源和内存空间。</span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="font-size:18px">因为在Java中有垃圾回收（GC）机制，所以我们平时都不会太关注它，下面就来简单的介绍一下回收机制：</span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="font-size:12px"><br>
</span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="font-family:Microsoft YaHei; font-size:24px; color:#996633"><strong><br>
</strong></span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="font-family:Microsoft YaHei; font-size:24px; color:#996633"><strong>垃圾回收（GC）：</strong></span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="font-family:Microsoft YaHei; font-size:18px"><strong><br>
</strong></span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="font-family:Microsoft YaHei; font-size:18px"><strong>Java垃圾回收器：</strong></span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="font-size:14px">在C，C&#43;&#43;或其他程序设计语言中，资源或内存都必须由程序员自行声明产生和回收，否则其中的资源将消耗，造成资源的浪费甚至崩溃。但手工回收内存往往是一项复杂而艰巨的工作。<br>
</span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="font-size:14px">于是，Java技术提供了一个系统级的线程，即垃圾收集器线程（Garbage Collection Thread），来跟踪每一块分配出去的内存空间，当Java 虚拟机（Java Virtual Machine）处于空闲循环时，垃圾收集器线程会自动检查每一快分配出去的内存空间，然后自动回收每一快可以回收的无用的内存块。&nbsp;</span><br>
</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="font-family:Arial,sans-serif,Helvetica,Tahoma; font-size:17px; line-height:18px"><br>
</span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="font-family:Microsoft YaHei; font-size:18px"><span style="line-height:18px"><strong>作用：</strong></span></span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="font-size:14px"><strong>1.</strong><span style="font-family:Verdana,Geneva,Arial,Helvetica,sans-serif; line-height:20.799999237060547px">清除不用的对象来释放内存：</span></span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="font-family:Arial,sans-serif,Helvetica,Tahoma; line-height:18px"><span style="font-family:Verdana,Geneva,Arial,Helvetica,sans-serif; line-height:20.799999237060547px"><span style="font-size:14px">采用<span style="font-family:Arial,sans-serif,Helvetica,Tahoma; line-height:18px">一种动态存储管理技术，它自动地释放不再被程序引用的对象，按照特定的垃圾收集算法来实现资源自动回收的功能。当一个对象不再被引用的时候，内存回收它占领的空间，以便空间被后来的新对象使用。&nbsp;</span></span></span></span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="font-size:14px; font-family:Arial,sans-serif,Helvetica,Tahoma; line-height:18px"><strong>2.</strong></span><span style="font-size:14px; font-family:Verdana,Geneva,Arial,Helvetica,sans-serif; line-height:20.799999237060547px">消除堆内存空间的碎片：</span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="font-family:Arial,sans-serif,Helvetica,Tahoma; line-height:18px"><span style="font-family:Verdana,Geneva,Arial,Helvetica,sans-serif; line-height:20.799999237060547px"><span style="color:rgb(51,51,51); font-family:Arial; line-height:26px"><span style="font-size:14px">由于创建对象和垃圾收集器释放丢弃对象所占的内存空间，内存会出现碎片。碎片是分配给对象的内存块之间的空闲内存洞。碎片整理将所占用的堆内存移到堆的一端，JVM将整理出的内存分配给新的对象。&nbsp;</span></span><br>
</span></span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="font-family:Arial,sans-serif,Helvetica,Tahoma; font-size:17px; line-height:18px"><br>
</span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="font-family:Microsoft YaHei; font-size:18px"><span style="line-height:18px"><strong>垃圾回收器优点：</strong></span></span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="font-family:Arial,sans-serif,Helvetica,Tahoma"><span style="line-height:18px"><span style="color:rgb(51,51,51); font-family:Arial; line-height:26px"><span style="font-size:14px"><strong>1.</strong><span style="color:rgb(51,51,51); font-family:Arial; line-height:26px">减轻编程的负担，提高效率</span>：</span></span></span></span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="font-family:Arial,sans-serif,Helvetica,Tahoma"><span style="line-height:18px"><span style="color:rgb(51,51,51); font-family:Arial; line-height:26px"><span style="font-size:14px"><span style="color:rgb(17,17,17); font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:25px">使程序员从手工回收内存空间的繁重工作中解脱了出来，因为</span>在没有垃圾收集机制的时候，可能要花许多时间来解决一个难懂的存储器问题。在用Java语言编程的时候，靠垃圾收集机制可大大缩短时间。</span></span></span></span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="font-size:14px; color:rgb(51,51,51); font-family:Arial; line-height:26px"><strong>2.</strong>它保护程序的完整性：</span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="font-family:Arial,sans-serif,Helvetica,Tahoma"><span style="line-height:18px"><span style="color:rgb(51,51,51); font-family:Arial; line-height:26px"><span style="font-size:14px">因此垃圾收集是Java语言安全性策略的一个重要部份。&nbsp;</span></span><br>
</span></span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="font-family:Arial,sans-serif,Helvetica,Tahoma; font-size:14px"><span style="line-height:18px"><span style="color:rgb(51,51,51); font-family:Arial; font-size:14px; line-height:26px"><br>
</span></span></span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="font-size:18px"><span style="line-height:18px"><span style="color:rgb(51,51,51); line-height:26px"><strong><span style="font-family:Microsoft YaHei">垃圾回收器缺点：</span></strong></span></span></span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="font-family:Arial,sans-serif,Helvetica,Tahoma"><span style="line-height:18px"><span style="color:rgb(51,51,51); font-family:Arial; line-height:26px"><span style="color:rgb(51,51,51); font-family:Arial; line-height:26px"><span style="color:rgb(17,17,17); font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:25px"><span style="font-size:14px"><strong>1.</strong>占用资源时间：</span></span></span></span></span></span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="font-family:Arial,sans-serif,Helvetica,Tahoma"><span style="line-height:18px"><span style="color:rgb(51,51,51); font-family:Arial; line-height:26px"><span style="color:rgb(51,51,51); font-family:Arial; line-height:26px"><span style="color:rgb(17,17,17); font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:25px"><span style="font-size:14px"><span style="color:rgb(51,51,51); font-family:Arial; line-height:26px">Java虚拟机必须追踪运行程序中有用的对象,
 而且最终释放没用的对象。这一个过程需要花费处理器的时间。</span><br>
</span></span></span></span></span></span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="font-size:14px; color:rgb(17,17,17); font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:25px"><strong>2.</strong>不可预知:</span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="font-family:Arial,sans-serif,Helvetica,Tahoma"><span style="line-height:18px"><span style="color:rgb(51,51,51); font-family:Arial; line-height:26px"><span style="color:rgb(51,51,51); font-family:Arial; line-height:26px"><span style="color:rgb(17,17,17); font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:25px"><span style="font-size:14px"><span style="color:rgb(17,17,17); font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:25px">垃圾收集器线程虽然是作为低优先级的线程运行，但在系统可用内存量过低的时候，它可能会突发地执行来挽救内存资源。当然其执行与否也是不可预知的。&nbsp;</span><br>
</span></span></span></span></span></span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="font-size:14px; color:rgb(17,17,17); font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:25px"><strong>3.</strong>不确定性：</span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="font-family:Arial,sans-serif,Helvetica,Tahoma"><span style="line-height:18px"><span style="color:rgb(51,51,51); font-family:Arial; line-height:26px"><span style="color:rgb(51,51,51); font-family:Arial; line-height:26px"><span style="color:rgb(17,17,17); font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:25px"><span style="font-size:14px"><span style="color:rgb(17,17,17); font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:25px"></span></span></span></span></span></span></span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="font-family:Arial,sans-serif,Helvetica,Tahoma; line-height:18px"><span style="color:rgb(17,17,17); font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:25px"><span style="font-size:14px">不能保证一个无用的对象一定会被垃圾收集器收集，也不能保证垃圾收集器在一段Java语言代码中一定会执行。<br>
</span></span></span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="font-size:14px">同样也没有办法预知在一组均符合垃圾收集器收集标准的对象中，哪一个会被首先收集。&nbsp;</span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="font-family:Arial,sans-serif,Helvetica,Tahoma"><span style="line-height:18px"><span style="color:rgb(51,51,51); font-family:Arial; line-height:26px"><span style="color:rgb(51,51,51); font-family:Arial; line-height:26px"><span style="color:rgb(17,17,17); font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:25px"><span style="font-size:14px"><strong>4.</strong>不可操作</span></span></span></span></span></span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="font-family:Arial,sans-serif,Helvetica,Tahoma"><span style="line-height:18px"><span style="color:rgb(51,51,51); font-family:Arial; line-height:26px"><span style="color:rgb(51,51,51); font-family:Arial; line-height:26px"><span style="font-size:14px"><span style="color:rgb(17,17,17); font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:25px"></span></span></span></span></span></span></p>
<span style="color:rgb(17,17,17); font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:25px"><span style="font-size:14px">垃圾收集器不可以被强制执行，但程序员可以通过调用System. gc方法来建议执行垃圾收集器。</span></span></div>
<div class="column"><br>
</div>
<div class="column"><strong><span style="font-size:18px"><span style="font-family:Microsoft YaHei">垃圾回收算法：</span></span></strong></div>
<div class="column"><span style="font-size:14px"></span></div>
<span style="font-size:14px"><span style="color:rgb(68,68,68); font-family:Helvetica,Tahoma,Arial,sans-serif; line-height:25px"><strong>1.</strong>引用计数（Reference Counting）</span><span style="color:rgb(68,68,68); font-family:Helvetica,Tahoma,Arial,sans-serif; line-height:25px">&nbsp;</span><br style="color:rgb(68,68,68); font-family:Helvetica,Tahoma,Arial,sans-serif; line-height:25px">
<span style="color:rgb(68,68,68); font-family:Helvetica,Tahoma,Arial,sans-serif; line-height:25px">比较古老的回收算法。原理是此对象有一个引用，即增加一个计数，删除一个引用则减少一个计数。垃圾回收时，只用收集计数为0的对象。此算法最致命的是无法处理循环引用的问题。&nbsp;</span></span></div>
<div class="layoutArea"><span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px; color:#444444"><span style="line-height:25px"><span style="color:rgb(68,68,68); font-family:Helvetica,Tahoma,Arial,sans-serif; line-height:25px"><strong>2.</strong>标记-清除（Mark-Sweep）</span><span style="color:rgb(68,68,68); font-family:Helvetica,Tahoma,Arial,sans-serif; line-height:25px">&nbsp;</span><br style="color:rgb(68,68,68); font-family:Helvetica,Tahoma,Arial,sans-serif; line-height:25px">
<span style="color:rgb(68,68,68); font-family:Helvetica,Tahoma,Arial,sans-serif; line-height:25px">此算法执行分两阶段。第一阶段从引用根节点开始标记所有被引用的对象，第二阶段遍历整个堆，把未标记的对象清除。此算法需要暂停整个应用，同时，会产生内存碎片。</span></span></span></div>
<div class="layoutArea"><span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px; color:#444444"><span style="line-height:25px"><span style="color:rgb(68,68,68); font-family:Helvetica,Tahoma,Arial,sans-serif; line-height:25px"><strong>3.</strong>复制（Copying）</span><span style="color:rgb(68,68,68); font-family:Helvetica,Tahoma,Arial,sans-serif; line-height:25px">&nbsp;</span><br style="color:rgb(68,68,68); font-family:Helvetica,Tahoma,Arial,sans-serif; line-height:25px">
<span style="color:rgb(68,68,68); font-family:Helvetica,Tahoma,Arial,sans-serif; line-height:25px">此算法把内存空间划为两个相等的区域，每次只使用其中一个区域。垃圾回收时，遍历当前使用区域，把正在使用中的对象复制到另外一个区域中。次算法每次只处理正在使用中的对象，因此复制成本比较小，同时复制过去以后还能进行相应的内存整理，不过出现“碎片”问题。当然，此算法的缺点也是很明显的，就是需要两倍内存空间。</span></span></span></div>
<div class="layoutArea"><span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px; color:#444444"><span style="line-height:25px"><span style="color:rgb(68,68,68); font-family:Helvetica,Tahoma,Arial,sans-serif; line-height:25px"><strong>4.</strong>标记-整理（Mark-Compact）</span><span style="color:rgb(68,68,68); font-family:Helvetica,Tahoma,Arial,sans-serif; line-height:25px">&nbsp;</span><br style="color:rgb(68,68,68); font-family:Helvetica,Tahoma,Arial,sans-serif; line-height:25px">
<span style="color:rgb(68,68,68); font-family:Helvetica,Tahoma,Arial,sans-serif; line-height:25px">此算法结合了 “标记-清除”和“复制”两个算法的优点。也是分两阶段，第一阶段从根节点开始标记所有被引用对象，第二阶段遍历整个堆，把清除未标记对象并且把存活对象 “压缩”到堆的其中一块，按顺序排放。此算法避免了“标记-清除”的碎片问题，同时也避免了“复制”算法的空间问题。&nbsp;</span></span></span></div>
<div class="layoutArea"><span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px; color:#444444"><span style="line-height:25px"><span style="color:rgb(68,68,68); font-family:Helvetica,Tahoma,Arial,sans-serif; line-height:25px"><strong>5.</strong>增量收集（Incremental
 Collecting）</span><span style="color:rgb(68,68,68); font-family:Helvetica,Tahoma,Arial,sans-serif; line-height:25px">&nbsp;</span><br style="color:rgb(68,68,68); font-family:Helvetica,Tahoma,Arial,sans-serif; line-height:25px">
<span style="color:rgb(68,68,68); font-family:Helvetica,Tahoma,Arial,sans-serif; line-height:25px">实施垃圾回收算法，即：在应用进行的同时进行垃圾回收。不知道什么原因JDK5.0中的收集器没有使用这种算法的。&nbsp;</span></span></span></div>
<div class="layoutArea"><span style="font-family:Helvetica,Tahoma,Arial,sans-serif; color:#444444"><span style="line-height:25px"><span style="font-size:14px"><span style="color:rgb(68,68,68); font-family:Helvetica,Tahoma,Arial,sans-serif; line-height:25px"><strong>6.</strong>分代（Generational
 Collecting）</span><span style="color:rgb(68,68,68); font-family:Helvetica,Tahoma,Arial,sans-serif; line-height:25px">&nbsp;</span><br style="color:rgb(68,68,68); font-family:Helvetica,Tahoma,Arial,sans-serif; line-height:25px">
<span style="color:rgb(68,68,68); font-family:Helvetica,Tahoma,Arial,sans-serif; line-height:25px">基于对对象生命周期分析后得出的垃圾回收算法。把对象分为年青代、年老代、持久代，对不同生命周期的对象使用不同的算法（上述方式中的一个）进行回收。现在的垃圾回收器（从J2SE1.2开始）都是使用此算法的。&nbsp;</span></span><br>
</span></span><span style="color:rgb(51,51,51); font-size:14px; line-height:26px; font-family:宋体"></span>
<div class="column"><span style="font-size:14px"></span><br>
</div>
<div class="column"><br>
</div>
<div class="column">
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="color:rgb(17,17,17); line-height:25px"><strong><span style="font-family:Microsoft YaHei; font-size:18px">finalize（）:</span></strong></span><br>
</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="color:rgb(17,17,17); font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:25px"><span style="color:rgb(17,17,17); font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:25px"><span style="font-size:14px">每一个对象都有一个finalize方法，这个方法是从Object类继承来的。&nbsp;</span></span></span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="font-size:14px"><span style="color:rgb(17,17,17); font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:25px"><span style="color:rgb(17,17,17); font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:25px"><span style="color:rgb(17,17,17); font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:25px">当垃圾回收确定不存在对该对象的更多引用时，由对象的垃圾回收器调用此方法</span></span></span><span style="color:rgb(17,17,17); font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:25px">。</span></span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<br>
</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="color:rgb(17,17,17); font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:25px"><span style="font-size:14px">Java 技术允许使用finalize方法在垃圾收集器将对象从内存中清除出去之前做必要的清理工作。一旦垃圾回收器准备好释放对象占用的空间，将首先调用其finalize()方法，并且在下一次垃圾回收动作发生时，才会真正回收对象占用的内存。<br>
简单的说finalize方法是在垃圾收集器删除对象之前对这个对象调用的</span></span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="color:rgb(17,17,17); font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:25px"><br>
<span style="font-size:17.77777862548828px; color:rgb(17,17,17); font-family:'Microsoft YaHei'; line-height:25px"><strong>System.gc（）：</strong></span></span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="font-size:14px">我们可以调用System.gc方法，建议虚拟机进行垃圾回收工作（注意，是建议，但虚拟机会不会这样干，我们也无法预知！）</span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="font-family:'Microsoft YaHei'"><span style="font-family:Arial,Helvetica,sans-serif"><span style="font-size:14px"><br>
</span></span></span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="font-family:'Microsoft YaHei'"><span style="font-family:Arial,Helvetica,sans-serif"><span style="font-size:14px">下面来看一个例子来了解<span style="color:rgb(17,17,17); font-family:'Microsoft YaHei'; line-height:25px"><strong>finalize（）</strong></span><span style="color:rgb(17,17,17); font-family:'Microsoft YaHei'; line-height:25px">和<span style="color:rgb(17,17,17); font-family:'Microsoft YaHei'; line-height:25px"><strong>System.gc（）</strong></span><span style="color:rgb(17,17,17); font-family:'Microsoft YaHei'; line-height:25px">的使用：</span></span></span></span></span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="font-family:Arial,Helvetica,sans-serif; font-size:14px"></span></p>
<pre name="code" class="java" style="color: rgb(17, 17, 17); font-family: 'Microsoft YaHei'; font-size: 17.77777862548828px; font-weight: bold; line-height: 25px;">public class TestGC {
    public TestGC() {}
    
    //当垃圾回收器确定不存在对该对象的更多引用时，由对象的垃圾回收器调用此方法。
    protected void finalize() {
        System.out.println(&quot;我已经被垃圾回收器回收了...&quot;);
    }
    
    public static void main(String [] args) {
        TestGC gc = new TestGC();
        gc = null;  
        // 建议虚拟机进行垃圾回收工作
        System.gc();
    }
}</pre><span style="font-size:14px">如上面的例子所示，大家可以猜猜重写的finalize方法会不会执行？</span>
<p></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="font-size:14px">答案是：<strong>不一定</strong>！</span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="font-size:14px"><br>
</span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="font-size:14px">因为无论是设置gc的引用为null还是调用System.gc()方法都只是<strong>&quot;建议&quot;</strong>垃圾回收器进行垃圾回收，但是最终所有权还在垃圾回收器手中，它会不会进行回收我们无法预知！<br>
</span><br>
<strong><span style="font-family:Microsoft YaHei; font-size:18px">垃圾回收面试题：</span></strong><br>
<span style="font-family:Arial,Helvetica,sans-serif"><span style="font-size:18px">最后通过网上找到的3道面试题来结束垃圾回收的内容。</span></span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="color:rgb(17,17,17); font-family:宋体,Verdana,Arial,Helvetica,sans-serif; font-size:14px; line-height:25px">&nbsp;</span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; color:rgb(17,17,17)"><span style="line-height:25px"><span style="color:rgb(17,17,17); font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:25px"><span style="font-size:14px"><strong>面试题一：</strong>&nbsp;</span></span></span></span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; color:rgb(17,17,17)"><span style="font-size:14.44444465637207px; line-height:25px"><span style="color:rgb(17,17,17); font-family:宋体,Verdana,Arial,Helvetica,sans-serif; font-size:14px; line-height:25px"></span></span></span></p>
<pre name="code" class="java">1．fobj = new Object ( ) ; 
2．fobj. Method ( ) ; 
3．fobj = new Object ( ) ; 
4．fobj. Method ( ) ; </pre>
<p></p>
<span style="font-size:14px"><span style="color:rgb(17,17,17); font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:25px"><strong>问：</strong>这段代码中，第几行的fobj 符合垃圾收集器的收集标准？&nbsp;</span><br style="color:rgb(17,17,17); font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:25px; padding:0px; margin:0px">
<span style="color:rgb(17,17,17); font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:25px"><strong>答：</strong>第3行。因为第3行的fobj被赋了新&#20540;，产生了一个新的对象，即换了一块新的内存空间，也相当于为第1行中的fobj赋了null&#20540;。这种类型的题是最简单的。&nbsp;</span><br style="color:rgb(17,17,17); font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:25px; padding:0px; margin:0px">
<br style="color:rgb(17,17,17); font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:25px; padding:0px; margin:0px">
<span style="color:rgb(17,17,17); font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:25px"><strong>面试题二：</strong>&nbsp;</span></span></div>
<div class="column"><span style="color:rgb(17,17,17); font-family:宋体,Verdana,Arial,Helvetica,sans-serif; font-size:14px; line-height:25px"></span><pre name="code" class="java">1．Object sobj = new Object ( ) ; 
2．Object sobj = null ; 
3．Object sobj = new Object ( ) ; 
4．sobj = new Object ( ) ; </pre><span style="font-size:14px"><span style="color:rgb(17,17,17); font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:25px"><strong>问：</strong>这段代码中，第几行的内存空间符合垃圾收集器的收集标准？&nbsp;</span><br style="color:rgb(17,17,17); font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:25px; padding:0px; margin:0px">
<span style="color:rgb(17,17,17); font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:25px"><strong>答：</strong>第2行和第4行。因为第2行为sobj赋&#20540;为null，所以在此第1行的sobj符合垃圾收集器的收集标准。而第4行相当于为sobj赋&#20540;为null，所以在此第3行的sobj也符合垃圾收集器的收集标准。&nbsp;</span><br style="color:rgb(17,17,17); font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:25px; padding:0px; margin:0px">
<br style="color:rgb(17,17,17); font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:25px; padding:0px; margin:0px">
<span style="color:rgb(17,17,17); font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:25px">如果有一个对象的句柄a，且你把a作为某个构造器的参数，即 new Constructor ( a )的时候，即使你给a赋&#20540;为null，a也不符合垃圾收集器的收集标准。直到由上面构造器构造的新对象被赋空&#20540;时，a才可以被垃圾收集器收集。&nbsp;</span></span><br style="color:rgb(17,17,17); font-family:宋体,Verdana,Arial,Helvetica,sans-serif; font-size:14px; line-height:25px; padding:0px; margin:0px">
<br style="color:rgb(17,17,17); font-family:宋体,Verdana,Arial,Helvetica,sans-serif; font-size:14px; line-height:25px; padding:0px; margin:0px">
<span style="color:rgb(17,17,17); font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:25px"><span style="font-size:14px"><strong>面试题三：&nbsp;</strong></span></span><br style="color:rgb(17,17,17); font-family:宋体,Verdana,Arial,Helvetica,sans-serif; font-size:14px; line-height:25px; padding:0px; margin:0px">
<pre name="code" class="java">1．Object aobj = new Object ( ) ; 
2．Object bobj = new Object ( ) ; 
3．Object cobj = new Object ( ) ; 
4．aobj = bobj; 
5．aobj = cobj; 
6．cobj = null; 
7．aobj = null; </pre><span style="font-size:14px"><span style="color:rgb(17,17,17); font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:25px"><strong>问：</strong>这段代码中，第几行的内存空间符合垃圾收集器的收集标准？&nbsp;</span><br style="color:rgb(17,17,17); font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:25px; padding:0px; margin:0px">
<span style="color:rgb(17,17,17); font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:25px"><strong>答：</strong>第4，7行。注意这类题型是认证考试中可能遇到的最难题型了。&nbsp;</span><br style="color:rgb(17,17,17); font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:25px; padding:0px; margin:0px">
<span style="color:rgb(17,17,17); font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:25px">行1-3：分别创建了Object类的三个对象：aobj，bobj，cobj</span><br style="color:rgb(17,17,17); font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:25px; padding:0px; margin:0px">
<span style="color:rgb(17,17,17); font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:25px">行4：此时对象aobj的句柄指向bobj，原来aojb指向的对象已经没有任何引用或变量指向，这时，就符合回收标准。</span><br style="color:rgb(17,17,17); font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:25px; padding:0px; margin:0px">
<span style="color:rgb(17,17,17); font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:25px">行5：此时对象aobj的句柄指向cobj，所以该行的执行不能使aobj符合垃圾收集器的收集标准。&nbsp;</span><br style="color:rgb(17,17,17); font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:25px; padding:0px; margin:0px">
<span style="color:rgb(17,17,17); font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:25px">行6：此时仍没有任何一个对象符合垃圾收集器的收集标准。&nbsp;</span><br style="color:rgb(17,17,17); font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:25px; padding:0px; margin:0px">
<span style="color:rgb(17,17,17); font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:25px">行7：对象cobj符合了垃圾收集器的收集标准，因为cobj的句柄指向单一的地址空间。在第6行的时候，cobj已经被赋&#20540;为null，但由cobj同时还指向了aobj（第5行），所以此时cobj并不符合垃圾收集器的收集标准。而在第7行，aobj所指向的地址空间也被赋予了空&#20540;null，这就说明了，由cobj所指向的地址空间已经被完全地赋予了空&#20540;。所以此时cobj最终符合了垃圾收集器的收集标准。
 但对于aobj和bobj，仍然无法判断其是否符合收集标准。&nbsp;</span><br style="color:rgb(17,17,17); font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:25px; padding:0px; margin:0px">
<br style="color:rgb(17,17,17); font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:25px; padding:0px; margin:0px">
<span style="color:rgb(17,17,17); font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:25px">总之，在Java语言中，判断一块内存空间是否符合垃圾收集器收集的标准只有两个：&nbsp;</span><br style="color:rgb(17,17,17); font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:25px; padding:0px; margin:0px">
<span style="color:rgb(17,17,17); font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:25px"><strong>1．</strong>给对象赋予了空&#20540;null，以下再没有调用过。&nbsp;</span><br style="color:rgb(17,17,17); font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:25px; padding:0px; margin:0px">
<span style="color:rgb(17,17,17); font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:25px"><strong>2．</strong>给对象赋予了新&#20540;，既重新分配了内存空间。&nbsp;</span><br style="color:rgb(17,17,17); font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:25px; padding:0px; margin:0px">
<br style="color:rgb(17,17,17); font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:25px; padding:0px; margin:0px">
<span style="color:rgb(17,17,17); font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:25px">最后再次提醒一下，一块内存空间符合了垃圾收集器的收集标准，并不意味着这块内存空间就一定会被垃圾收集器收集。</span></span><br>
<p></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="color:rgb(17,17,17); font-family:宋体,Verdana,Arial,Helvetica,sans-serif; font-size:14px; line-height:25px"><span style="color:rgb(17,17,17); font-family:宋体,Verdana,Arial,Helvetica,sans-serif; font-size:14px; line-height:25px"><br>
</span></span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="color:rgb(17,17,17); font-family:宋体,Verdana,Arial,Helvetica,sans-serif; font-size:14px; line-height:25px"><span style="color:rgb(17,17,17); font-family:宋体,Verdana,Arial,Helvetica,sans-serif; font-size:14px; line-height:25px"><br>
</span></span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="font-family:Microsoft YaHei; font-size:24px; color:#996633"><strong>资源的回收：</strong></span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="color:rgb(17,17,17); font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:25px"><span style="color:rgb(17,17,17); font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:25px"><span style="font-size:18px">刚才讲了一堆理论的东西，下面来点实际能用上的，资源的回收：</span></span></span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="color:rgb(17,17,17); font-family:宋体,Verdana,Arial,Helvetica,sans-serif; font-size:14px; line-height:25px"><span style="color:rgb(17,17,17); font-family:宋体,Verdana,Arial,Helvetica,sans-serif; font-size:14px; line-height:25px"><br>
</span></span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="font-family:Microsoft YaHei; font-size:18px; color:#111111"><span style="line-height:25px"><strong>Thread（线程）回收：</strong></span></span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="font-size:14px">线程中涉及的任何东西GC都不能回收（Anything reachable by a thread cannot be GC'd&nbsp;），<span style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; color:#111111"><span style="line-height:25px">所以线程很容易造成内存泄露。</span></span></span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="font-size:14px"><span style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; color:#111111"><span style="line-height:25px">如下面代码所示：</span></span></span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
</p>
<pre name="code" class="java" style="color: rgb(17, 17, 17); font-family: 宋体, Verdana, Arial, Helvetica, sans-serif; font-size: 14px; line-height: 25px;">Thread t = new Thread() {
	public void run() {
		while (true) {
			try {
				Thread.sleep(1000);
				System.out.println(&quot;thread is running...&quot;);
			} catch (InterruptedException e) {
			
			}
		}
	}
};
t.start();
t = null;
System.gc();</pre><span style="font-size:14px">如上在线程t中每间隔一秒输出一段话，然后将线程设置为null并且调用System.gc方法。</span>
<p></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="font-size:14px">最后的结果是线程并不会被回收，它会一直的运行下去。</span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="font-size:14px"><br>
</span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
</p>
<p style="margin-top:0px; margin-bottom:5px; padding-top:0px; padding-bottom:0px; border:0px; list-style:none; word-wrap:normal; word-break:normal">
<span style="font-size:14px">因为运行中的线程是称之为垃圾回收根（GC Roots）对象的一种，不会被垃圾回收。当垃圾回收器判断一个对象是否可达，总是使用垃圾回收根对象作为参考点。</span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; color:rgb(17,17,17)"><span style="font-size:14.44444465637207px; line-height:25px"><br>
</span></span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="color:#111111"><span style="line-height:25px"><span style="font-family:Microsoft YaHei; font-size:18px"><strong>Cursor（游标）回收：</strong></span><br>
</span></span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="color:rgb(17,17,17); font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:25px"><span style="color:rgb(17,17,17); font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:25px"></span></span></p>
<p><span style="font-size:14px">Cursor是Android查询数据后得到的一个管理数据集合的类，在使用结束以后。应该保证Cursor占用的内存被及时的释放掉，而不是等待GC来处理。并且Android明显是倾向于编程者手动的将Cursor&nbsp;<wbr><wbr>close掉，因为在源代码中我们发现，如果等到垃圾回收器来回收时，会给用户以错误提示。</span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; border:0px; list-style:none; word-wrap:normal; word-break:normal">
<span style="font-size:14px">所以我们使用Cursor的方式一般如下：</span></p>
<p style="font-size:14px; margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; border:0px; list-style:none; word-wrap:normal; word-break:normal">
</p>
<pre name="code" class="java" style="font-size: 14px;">        Cursor cursor = null;
        try {
            cursor = mContext.getContentResolver().query(uri,null, null,null,null);
            if(cursor != null) {
                cursor.moveToFirst();
                //do something
            }
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            if (cursor != null) {
                cursor.close();
            }
        }</pre><span style="font-size:14px">有一种情况下，我们不能直接将Cursor关闭掉，这就是在CursorAdapter中应用的情况，但是注意，CursorAdapter在Acivity结束时并没有自动的将Cursor关闭掉，因此，你需要在onDestroy函数中，手动关闭。</span><br>
<pre name="code" class="java" style="font-size: 14px;">@Override  
protected void onDestroy() {        
    if (mAdapter != null &amp;&amp; mAdapter.getCurosr() != null) {  
        mAdapter.getCursor().close();  
    }  
    super.onDestroy();   
}  </pre><br>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="color:rgb(17,17,17); font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:25px"><span style="color:rgb(17,17,17); font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:25px"></span></span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="color:rgb(17,17,17); line-height:25px"><span style="line-height:25.200000762939453px"><strong><span style="font-family:Microsoft YaHei; font-size:18px">Receiver（接收器）回收</span></strong></span><br>
</span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="color:rgb(17,17,17); font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:25px"></span></p>
<div class="column"><span style="font-size:14px"><span style="font-family:Helvetica,Tahoma,Arial,sans-serif; line-height:25.200000762939453px">调用registerReceiver()后未调用unregisterReceiver().</span><span style="font-family:Helvetica,Tahoma,Arial,sans-serif; line-height:25.200000762939453px">&nbsp;</span></span></div>
<div class="column"><span style="font-family:Helvetica,Tahoma,Arial,sans-serif"><span style="line-height:25.200000762939453px"><span style="font-size:14px">当我们Activity中使用了registerReceiver()方法注册了BroadcastReceiver，一定要在Activity的生命周期内调用unregisterReceiver()方法取消注册&nbsp;<br>
也就是说registerReceiver()和unregisterReceiver()方法一定要成对出现，通常我们可以重写Activity的onDestory()方法：&nbsp;</span></span></span></div>
<div class="column" style="font-size:14px"><pre name="code" class="java" style="color: rgb(17, 17, 17); line-height: 25px;">@Override  
protected void onDestroy() {  
      this.unregisterReceiver(receiver);  
      super.onDestroy();  
}  </pre>
<div><br>
</div>
</div>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="color:rgb(17,17,17); line-height:25px"><span style="color:rgb(17,17,17); line-height:25px"><span style="line-height:25.200000762939453px"><span style="font-size:18px"><strong><span style="font-family:Microsoft YaHei">Stream/File（流/文件）回收：</span></strong></span></span></span></span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="color:rgb(17,17,17); font-family:宋体,Verdana,Arial,Helvetica,sans-serif; font-size:14px; line-height:25px"><span style="color:rgb(17,17,17); font-family:宋体,Verdana,Arial,Helvetica,sans-serif; font-size:14px; line-height:25px"><span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px; line-height:25.200000762939453px"><span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px; line-height:25.200000762939453px">主要针对各种流，文件资源等等如：</span></span></span></span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="color:rgb(17,17,17); font-family:宋体,Verdana,Arial,Helvetica,sans-serif; font-size:14px; line-height:25px"><span style="color:rgb(17,17,17); font-family:宋体,Verdana,Arial,Helvetica,sans-serif; font-size:14px; line-height:25px"><span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px; line-height:25.200000762939453px"><span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px; line-height:25.200000762939453px">InputStream/OutputStream，</span><span style="font-family:Verdana,Arial,Helvetica,sans-serif; font-size:14.44444465637207px; line-height:21px">SQLiteOpenHelper，SQLiteDatabase，Cursor，文件，I/O，Bitmap图片等操作等都应该记得显示关闭。</span><span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px; line-height:25.200000762939453px"></span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px; line-height:25.200000762939453px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px; line-height:25.200000762939453px">和之前介绍的Cursor道理类&#20284;，就不多说了。</span><br>
</span></span></span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="color:rgb(17,17,17); font-family:宋体,Verdana,Arial,Helvetica,sans-serif; font-size:14px; line-height:25px"><span style="color:rgb(17,17,17); font-family:宋体,Verdana,Arial,Helvetica,sans-serif; font-size:14px; line-height:25px"><br>
</span></span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<br>
</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<br>
</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="color:rgb(17,17,17); font-family:宋体,Verdana,Arial,Helvetica,sans-serif; font-size:14px; line-height:25px"><br>
</span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; font-family:Arial; line-height:26px; text-align:center">
<strong><span style="color:rgb(204,0,0)"><span style="font-size:32px">Review:</span></span></strong></p>
<div>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<br>
</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="font-size:18px">Review（回顾，检查），大家都知道Code Review的重要性。而这里我说的Review和Code Review差不多，主要目的就是检查代码中存在的不合理和可以改进的地方，当然这个Review需要大家自己来做啦。</span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<br>
</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<br>
</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="font-family:Microsoft YaHei; font-size:24px; color:#996633"><strong>Code Review（代码检查）：</strong></span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="font-size:18px">Code Review主要检查代码中存在的一些不合理或可以改进优化的地方，大家可以参考之前写的Reduce，Reuse和Recycle都是侧重讲解这方面的。</span><br>
</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<br>
</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<br>
</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<br>
</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="font-family:Microsoft YaHei; font-size:24px; color:#996633"><strong>UI Review（视图检查）：</strong></span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="font-size:18px">Android对于视图中控件的布局渲染等会消耗很多的资源和内存，所以这部分也是我们需要注意的。</span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<br>
</p>
<span style="font-family:'Microsoft YaHei'"><strong><span style="font-size:18px">减少视图层级：</span></strong></span></div>
<div><span style="font-size:14px">减少视图层级可以有效的减少内存消耗，因为视图是一个树形结构，每次刷新和渲染都会遍历一次。</span></div>
<div><span style="font-size:18px"><span style="font-size:18px"><br>
</span></span></div>
<div><strong><span style="font-size:14px">hierarchyviewer：</span></strong></div>
<div><span style="font-size:14px">想要减少视图层级首先就需要知道视图层级，所以下面介绍一个SDK中自带的一个非常好用的工具hierarchyviewer。</span></div>
<div><span style="font-size:14px">你可以在下面的地址找到它：<span style="font-family:Arial; font-size:14px; line-height:26px"><strong>your sdk path\sdk\tools</strong></span></span></div>
<div><span style="font-size:14px"><img src="http://img.blog.csdn.net/20140908210538701?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYTM5NjkwMTk5MA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" alt=""><br>
</span></div>
<div>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="font-size:14px"><br>
</span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="font-size:14px">如上图大家可以看到，</span><span style="font-size:14px">hierarchyviewer</span><span style="font-size:14px">可以非常清楚的看到当前视图的层级结构，并且可以查看视图的执行效率（视图上的小圆点，绿色表示流畅，黄色和红色次之），所以我们可以很方便的查看哪些view可能会影响我们的性能从而去进一步优化它。</span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<br>
</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="font-size:14px">hierarchyviewer还提供另外一种列表式的查看方式，<span style="font-family:Arial; line-height:26px">可以查看详细的屏幕画面，具体到像素级别的问题都可以通过它发现。</span></span><br>
</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<br>
</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
</p>
<p style="margin:10px auto; padding-top:0px; padding-bottom:0px; line-height:21px">
<span style="margin:0px; padding:0px"><strong><span style="font-family:'Microsoft YaHei'"><span style="font-size:14px">ViewStub标签</span></span></strong></span></p>
<p style="margin:10px auto; padding-top:0px; padding-bottom:0px; font-family:Verdana,Arial,Helvetica,sans-serif; line-height:21px">
<span style="color:rgb(35,35,35); font-family:Verdana,Arial,helvetica,sans-seriff; line-height:21.600000381469727px"><span style="font-size:14px">此标签可以使UI在特殊情况下，直观效果类&#20284;于设置View的不可见性，但是其更大的意义在于被这个标签所包裹的Views在默认状态下不会占用任何内存空间。</span></span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<strong><span style="font-family:Microsoft YaHei; font-size:18px"></span></strong></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<strong><span style="font-size:14px"><br>
</span></strong></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<strong><span style="font-size:14px">include标签</span></strong><br>
</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="font-size:14px">可以通过这个标签直接加载外部的xml到当前结构中，是复用UI资源的常用标签。</span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<strong><span style="font-family:Microsoft YaHei; font-size:18px"><br>
</span></strong></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<strong><span style="font-family:'Microsoft YaHei'"><span style="font-size:14px">merge标签</span></span></strong><br>
</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="font-size:14px"><span style="color:rgb(35,35,35); font-family:Verdana,Arial,helvetica,sans-seriff; line-height:21.600000381469727px">它在优化UI结构时起到很重要的作用。目的是通过删减多余或者额外的层级，从而优化整个Android Layout的结构。</span><br>
</span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="font-size:14px"><br>
</span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="font-size:14px">（<span style="color:#ff0000">注意</span>：灵活运用以上3个标签可以有效减少视图层级，具体使用大家可以上网搜搜）</span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<br>
</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
</p>
<p style="margin:10px auto; padding-top:0px; padding-bottom:0px; font-family:Verdana,Arial,Helvetica,sans-serif; line-height:21px">
<span style="margin:0px; padding:0px"><strong><span style="font-size:14px">布局用Java代码比写在XML中快</span></strong></span></p>
<p style="margin:10px auto; padding-top:0px; padding-bottom:0px; font-family:Verdana,Arial,Helvetica,sans-serif; line-height:21px">
<span style="font-size:14px">一般情况下对于Android程序布局往往使用XML文件来编写，这样可以提高开发效率，但是考虑到代码的安全性以及执行效率，可以通过Java代码执行创建，虽然Android编译过的XML是二进制的，但是加载XML解析器的效率对于资源占用还是比较大的，Java处理效率比XML快得多，但是对于一个复杂界面的编写，可能需要一些套嵌考虑，如果你思维灵活的话，使用Java代码来布局你的Android应用程序是一个更好的方法。</span></p>
<div><br>
</div>
<div><br>
</div>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<strong><span style="font-family:'Microsoft YaHei'"><span style="font-size:18px">重用系统资源：</span></span></strong></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
</p>
<p style="color:rgb(51,51,51); line-height:26px"><span style="font-family:'Microsoft YaHei'"><strong><span style="font-size:14px">1. 利用系统定义的id</span></strong></span></p>
<p style="color:rgb(51,51,51); font-family:Arial; line-height:26px"><span style="font-size:14px">比如我们有一个定义ListView的xml文件，一般的，我们会写类&#20284;下面的代码片段。</span></p>
<p style="color:rgb(51,51,51); font-family:Arial; font-size:14px; line-height:26px">
</p>
<pre name="code" class="html">&lt;ListView
    android:id=&quot;@+id/mylist&quot;
    android:layout_width=&quot;fill_parent&quot;
    android:layout_height=&quot;fill_parent&quot;/&gt;</pre>
<p></p>
<p style="color:rgb(51,51,51); font-family:Arial; line-height:26px"><span style="font-size:14px">这里我们定义了一个ListView，定义它的id是&quot;@&#43;id/mylist&quot;。实际上，如果没有特别的需求，就可以利用系统定义的id，类&#20284;下面的样子。</span></p>
<p style="color:rgb(51,51,51); font-family:Arial; line-height:26px"></p>
<pre name="code" class="html" style="font-size: 14px;">&lt;ListView
    android:id=&quot;@android:id/list&quot;
    android:layout_width=&quot;fill_parent&quot;
    android:layout_height=&quot;fill_parent&quot;/&gt;</pre><span style="color:rgb(51,51,51); font-family:Arial; line-height:26px"><span style="font-size:14px">在xml文件中引用系统的id，只需要加上“@android:”前缀即可。如果是在Java代码中使用系统资源，和使用自己的资源基本上是一样的。不同的是，需要使用android.R类来使用系统的资源，而不是使用应用程序指定的R类。这里如果要获取ListView可以使用android.R.id.list来获取。</span></span>
<p></p>
<p style="color:rgb(51,51,51); line-height:26px"><strong><span style="font-family:'Microsoft YaHei'; font-size:14px">2. 利用系统的图片资源</span></strong><br>
</p>
<p style="color:rgb(51,51,51); font-family:Arial; font-size:14px; line-height:26px">
<span style="font-size:14px"><span style="font-size:14px">这样做的好处，一个是美工不需要重复的做一份已有的图片了，可以节约不少工时；另一个是能保证我们的应用程序的风&#26684;与系统一致。</span><br>
</span></p>
<p style="color:rgb(51,51,51); line-height:26px"><strong><span style="font-family:'Microsoft YaHei'; font-size:14px">3. 利用系统的字符串资源</span></strong></p>
<p style="color:rgb(51,51,51); font-family:Arial; font-size:14px; line-height:26px">
<span style="font-size:14px">如果使用系统的字符串，默认就已经支持多语言环境了。如上述代码，直接使用了@android:string/yes和@android:string/no，在简体中文环境下会显示“确定”和“取消”，在英文环境下会显示“OK”和“Cancel”。</span></p>
<p style="color:rgb(51,51,51); line-height:26px"><span style="font-family:'Microsoft YaHei'"><strong><span style="font-size:14px">4. 利用系统的Style</span></strong></span></p>
<p style="color:rgb(51,51,51); font-family:Arial; line-height:26px"><span style="font-size:14px">&nbsp;假设布局文件中有一个TextView，用来显示窗口的标题，使用中等大小字体。可以使用下面的代码片段来定义TextView的Style。</span></p>
<p style="color:rgb(51,51,51); font-family:Arial; line-height:26px"></p>
<pre name="code" class="html" style="font-size: 14px;">&lt;TextView
        android:id=&quot;@+id/title&quot;
        android:layout_width=&quot;wrap_content&quot;
        android:layout_height=&quot;wrap_content&quot;
        android:textAppearance=&quot;?android:attr/textAppearanceMedium&quot; /&gt;</pre><span style="color:rgb(51,51,51); font-family:Arial; line-height:26px"><span style="font-size:14px">其中android:textAppearance=&quot;?android:attr/textAppearanceMedium&quot;就是使用系统的style。需要注意的是，使用系统的style，需要在想要使用的资源前面加“?android:”作为前缀，而不是“@android:”。</span></span><br>
<p></p>
<p style="color:rgb(51,51,51); line-height:26px"><span style="font-family:'Microsoft YaHei'"><strong><span style="font-size:14px">5. 利用系统的颜色定义</span></strong></span><br>
</p>
<p style="color:rgb(51,51,51); font-family:Arial; line-height:26px"><span style="font-size:14px">除了上述的各种系统资源以外，还可以使用系统定义好的颜色。在项目中最常用的，就是透明色的使用。</span><br>
</p>
<p style="color:rgb(51,51,51); font-family:Arial; font-size:14px; line-height:26px">
<span style="font-size:14px"></span></p>
<pre name="code" class="html">android:background =&quot;@android:color/transparent&quot;</pre>
<p><br>
</p>
<p style="color:rgb(51,51,51); font-family:Arial; line-height:26px"><span style="font-size:14px">除了上面介绍的以外还有很多其他Android系统本身自带的资源，它们在应用中都可以直接使用。具体的，可以进入android-sdk的相应文件夹中去查看。例如：可以进入$android-sdk$\platforms\android-8\data\res，里面的系统资源就一览无余了。</span></p>
<p style="color:rgb(51,51,51); font-family:Arial; line-height:26px"><span style="font-size:14px">开发者需要花一些时间去熟悉这些资源，特别是图片资源和各种Style资源，这样在开发过程中，能重用的尽量重用，而且有时候使用系统提供的效果可能会更好。</span></p>
<div><br>
</div>
<br>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
</p>
<p style="margin:10px auto; padding-top:0px; padding-bottom:0px; line-height:21px">
<strong><span style="font-family:Microsoft YaHei; font-size:18px">其他小tips：</span></strong></p>
<p style="margin:10px auto; padding-top:0px; padding-bottom:0px"><span style="font-size:14px"><strong>1.&nbsp;</strong><span style="font-family:Verdana,Arial,Helvetica,sans-serif; line-height:21px">分辨率适配</span><span style="font-family:Verdana,Arial,Helvetica,sans-serif; line-height:21px">-ldpi,-mdpi,
 -hdpi配置不同精度资源，系统会根据设备自适应，包括drawable, layout,style等不同资源。</span></span></p>
<p style="margin:10px auto; padding-top:0px; padding-bottom:0px; font-family:Verdana,Arial,Helvetica,sans-serif; line-height:21px">
<span style="font-size:14px"><strong>2.</strong>尽量使用dp(density independent pixel)开发，不用px(pixel)。</span></p>
<p style="margin:10px auto; padding-top:0px; padding-bottom:0px; font-family:Verdana,Arial,Helvetica,sans-serif; line-height:21px">
<span style="font-size:14px"><strong>3.</strong>多用wrap_content, match_parent</span></p>
<p style="margin:10px auto; padding-top:0px; padding-bottom:0px; font-family:Verdana,Arial,Helvetica,sans-serif; line-height:21px">
<span style="font-size:14px"><strong>4.</strong>永远不要使用AbsoluteLayout</span></p>
<p style="margin:10px auto; padding-top:0px; padding-bottom:0px; font-family:Verdana,Arial,Helvetica,sans-serif; line-height:21px">
<span style="font-size:14px"><strong>5.</strong>使用9patch（通过~/tools/draw9patch.bat启动应用程序），png&#26684;式</span></p>
<p style="margin:10px auto; padding-top:0px; padding-bottom:0px; font-family:Verdana,Arial,Helvetica,sans-serif; line-height:21px">
<span style="font-size:14px"><strong>6.</strong>将Acitivity中的Window的背景图设置为空。getWindow().setBackgroundDrawable(null);android的默认背景是不是为空。</span></p>
<p style="margin:10px auto; padding-top:0px; padding-bottom:0px; font-family:Verdana,Arial,Helvetica,sans-serif; line-height:21px">
<span style="font-size:14px"><strong>7.</strong>View中设置缓存属性.setDrawingCache为true。</span></p>
<br>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<strong><br>
</strong></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<strong><span style="font-family:Microsoft YaHei; font-size:24px"></span></strong></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; color:rgb(153,102,51)">
<strong><span style="font-family:Microsoft YaHei; font-size:24px">Desgin Review（设计检查）：</span></strong></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="font-size:18px">Desgin Review主要侧重检查一下程序的设计是否合理，包括框架的设计，界面的设计，逻辑的设计<span style="font-size:17.77777862548828px">(其实这些东西开发之前就应该想好了)</span>。</span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<br>
</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<strong><span style="font-family:Microsoft YaHei; font-size:18px">框架设计：</span></strong></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="font-size:14px">是否定义了自己的Activity和fragment等常用控件的基类去避免进行重复的工作</span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="font-size:14.44444465637207px">是否有完善的异常处理机制，即使真的出现OOM也不会直接崩溃导致直接退出程序</span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<br>
</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="font-family:Microsoft YaHei; font-size:18px"><strong>界面设计：</strong></span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
</p>
<p><span style="font-size:14px"><strong>1.</strong>在视图中加载你所需要的，而不是你所拥有。因为用户不可能同时看到所有东西。</span><span style="font-size:14px">最典型的例子就是ListView中的滑动加载。</span></p>
<p><span style="font-size:14px"><strong>2.</strong>如果数据特别大，此时应该暗示用户去点击加载，而不是直接加载。</span></p>
<p><strong><span style="font-size:14px">3.</span></strong>合理运用分屏，转屏等，它是个双刃剑，因为它即可以使程序更加美观功能更加完善，但也相应增加了资源开销。</p>
<p><strong><br>
</strong></p>
<p><span style="font-family:Microsoft YaHei; font-size:18px"><strong>逻辑设计：</strong></span></p>
<p><span style="font-size:14px">避免子类直接去控制父类中内容，可以使用监听等方式去解决</span></p>
<p><br>
</p>
<p><span style="font-size:18px">关于这三点由于我工作经验比较少，加上一时半会也想不出来多少，如果大家有建议希望可以留言，之后我给加进去。</span></p>

