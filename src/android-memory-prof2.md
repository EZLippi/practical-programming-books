<div>
<p style="color:rgb(102,51,51); font-family:'Microsoft YaHei'; font-size:18px; font-weight:bold; line-height:26px">
<strong><span style="font-family:'Microsoft YaHei'; font-size:24px; color:rgb(204,0,0)">OOM：</span></strong></p>
<p style="color:rgb(102,51,51); font-family:'Microsoft YaHei'; font-size:18px; font-weight:bold; line-height:26px">
<span style="font-family:'Microsoft YaHei'; font-size:18px; color:rgb(204,102,0)"><strong><br>
</strong></span></p>
<p style="color:rgb(102,51,51); font-family:'Microsoft YaHei'; font-size:18px; font-weight:bold; line-height:26px">
<span style="font-family:'Microsoft YaHei'; font-size:18px; color:rgb(204,102,0)"><strong>内存泄露可以引发很多的问题：</strong></span></p>
<p>1.程序卡顿，响应速度慢（内存占用高时JVM虚拟机会频繁触发GC）</p>
<p>2.莫名消失（当你的程序所占内存越大，它在后台的时候就越可能被干掉。反之内存占用越小，在后台存在的时间就越长）</p>
<p>3.直接崩溃（OutOfMemoryError）</p>
<p style="color:rgb(102,51,51); font-family:'Microsoft YaHei'; font-size:18px; font-weight:bold; line-height:26px; margin-top:0px; margin-bottom:8px; padding-top:0px; padding-bottom:0px; border-width:0px; list-style:none">
<br>
</p>
<p style="color:rgb(102,51,51); font-family:'Microsoft YaHei'; font-size:18px; font-weight:bold; line-height:26px">
<span style="font-family:'Microsoft YaHei'; font-size:18px; color:rgb(204,102,0)"><strong>ANDROID内存面临的问题：</strong></span></p>
<p style="color:rgb(102,51,51); font-family:'Microsoft YaHei'; font-size:18px; font-weight:bold; line-height:26px">
</p>
<p>1.有限的堆内存，原始只有16M</p>
<p>2.内存大小消耗等根据设备，操作系统等级，屏幕尺寸的不同而不同</p>
<p>3.程序不能直接控制</p>
<p>4.支持后台多任务处理（multitasking）</p>
<p>5.运行在虚拟机之上</p>
</div>
<div style="color:rgb(102,51,51); font-family:SimSun; font-size:14px; font-weight:bold; line-height:26px">
<span style="font-family:'Microsoft YaHei'; font-size:18px"><br>
</span></div>
<p><span style="font-family:'Microsoft YaHei'; font-size:24px; color:rgb(204,0,0)"><strong>5R：</strong></span></p>
<p><span style="font-family:'Microsoft YaHei'; font-size:14px">本文主要通过如下的5R方法来对ANDROID内存进行优化：</span></p>
<p><span style="font-family:'Microsoft YaHei'; font-size:14px"><br>
</span></p>
<p><strong><span style="font-family:'Microsoft YaHei'; font-size:18px; color:rgb(204,102,0)">1.Reckon（计算）</span></strong></p>
<p><span style="font-size:14px"><span style="font-family:'Microsoft YaHei'"><span style="white-space:pre"></span>首先需要知道你的app所消耗内存的情况，知己知彼才能百战不殆</span></span></p>
<p><strong><span style="font-family:'Microsoft YaHei'; font-size:18px; color:rgb(204,102,0)">2.Reduce（减少）</span></strong></p>
<p><span style="font-size:14px"><span style="font-family:'Microsoft YaHei'"><span style="white-space:pre"></span>消耗更少的资源</span></span></p>
<p><span style="font-size:14px"><span style="font-family:'Microsoft YaHei'"></span></span></p>
<p><strong><span style="font-family:'Microsoft YaHei'; font-size:18px; color:rgb(204,102,0)">3.Reuse（重用）</span></strong></p>
<p><span style="font-size:14px"><span style="font-family:'Microsoft YaHei'"><span style="white-space:pre"></span>当第一次使用完以后，尽量给其他的使用</span></span></p>
<p><strong><span style="font-family:'Microsoft YaHei'; font-size:18px; color:rgb(204,102,0)">5.Recycle（回收）</span></strong></p>
<p><span style="font-size:14px"><span style="font-family:'Microsoft YaHei'"><span style="white-space:pre"></span>回收资源</span></span></p>
<p><strong><span style="font-family:'Microsoft YaHei'"><span style="font-size:18px; color:rgb(204,102,0)">4.Review（检查）</span><br>
</span></strong></p>
<p><span style="white-space:pre"><span style="font-size:14px"><span style="font-family:'Microsoft YaHei'">回顾检查你的程序，看看设计或代码有什么不合理的地方。</span></span></span></p>
<div class="column"><br>
</div>
<br>
<p style="text-align:center"><strong><span style="color:rgb(204,0,0)"><span style="font-size:32px">Reckon：</span></span></strong></p>
<p style="text-align:left"><span style="font-size:18px"><br>
</span></p>
<p style="text-align:left"><span style="font-size:18px">关于内存简介，和Reckon（内存计算）的内容请看上一篇文章：<a target="_blank" target="_blank" href="http://blog.csdn.net/a396901990/article/details/37914465">ANDROID内存优化(大汇总——上)</a></span></p>
<p style="text-align:center"><strong><span style="font-size:32px; color:#cc0000"><br>
</span></strong></p>
<p style="text-align:center"><strong><span style="font-size:32px; color:#cc0000"><br>
</span></strong></p>
<p style="text-align:center"><strong><span style="font-size:32px; color:#cc0000">Reduce ：</span></strong></p>
<p><span style="font-size:18px"><br>
</span></p>
<p><span style="font-size:18px">Reduce的意思就是减少，直接减少内存的使用是最有效的优化方式。</span></p>
<p><span style="font-size:18px">下面来看看有哪些方法可以减少内存使用：</span></p>
<p><br>
</p>
<p></p>
<div class="column"><span style="font-family:'Microsoft YaHei'; font-size:24px; color:rgb(153,102,51)"><strong>Bitmap</strong></span><span style="color:rgb(153,102,51); font-family:'Microsoft YaHei'; font-size:24px"><strong>：</strong></span></div>
<div class="column"><span style="font-size:18px">Bitmap是内存消耗大户，绝大多数的OOM崩溃都是在操作Bitmap时产生的，下面来看看几个处理图片的方法：</span></div>
<div class="column">
<p><br>
</p>
<p><strong><span style="font-family:'Microsoft YaHei'; font-size:18px">图片显示：</span></strong></p>
<p>我们需要根据需求去加载图片的大小。</p>
<p>例如在列表中仅用于预览时加载缩略图（thumbnails&nbsp;）。</p>
<p>只有当用户点击具体条目想看详细信息的时候，这时另启动一个fragment／activity／对话框等等，去显示整个图片</p>
<p></p>
<p><br>
</p>
<p><strong><span style="font-family:'Microsoft YaHei'; font-size:18px">图片大小：</span></strong></p>
<div class="page" title="Page 19">
<div class="layoutArea">
<div class="column">
<p>直接使用ImageView显示bitmap会占用较多资源，特别是图片较大的时候，可能导致崩溃。&nbsp;<br>
使用<strong>BitmapFactory.Options</strong>设置inSampleSize, 这样做可以减少对系统资源的要求。&nbsp;<br>
属性&#20540;inSampleSize表示缩略图大小为原始图片大小的几分之一，即如果这个&#20540;为2，则取出的缩略图的宽和高都是原始图片的1/2，图片大小就为原始大小的1/4。&nbsp;<br>
</p>
<p></p>
<pre name="code" class="java">        BitmapFactory.Options bitmapFactoryOptions = new BitmapFactory.Options();
        bitmapFactoryOptions.inJustDecodeBounds = true;
        bitmapFactoryOptions.inSampleSize = 2;
        // 这里一定要将其设置回false，因为之前我们将其设置成了true  
        // 设置inJustDecodeBounds为true后，decodeFile并不分配空间，即，BitmapFactory解码出来的Bitmap为Null,但可计算出原始图片的长度和宽度  
        options.inJustDecodeBounds = false;
        Bitmap bmp = BitmapFactory.decodeFile(sourceBitmap, options);</pre>
<p></p>
<p><br>
</p>
<p><strong><span style="font-family:'Microsoft YaHei'; font-size:18px">图片像素：</span></strong></p>
<div class="page" title="Page 20">
<div class="layoutArea">
<div class="column"><span style="font-size:14px">Android中图片有四种属性，分别是：</span><br>
<strong>ALPHA_8：</strong>每个像素占用1byte内存&nbsp;<br>
<strong>ARGB_4444：</strong>每个像素占用2byte内存&nbsp;<br>
<strong>ARGB_8888：</strong>每个像素占用4byte内存 （默认）<br>
<strong>RGB_565：</strong>每个像素占用2byte内存&nbsp;</div>
<div class="column"><br>
</div>
<div class="column">Android默认的颜色模式为ARGB_8888，这个颜色模式色彩最细腻，显示质量最高。但同样的，占用的内存也最大。 所以在对图片效果不是特别高的情况下使用RGB_565（565没有透明度属性），如下：</div>
<div class="column"><span style="font-family:Tahoma,Helvetica,Arial,宋体,sans-serif"><span style="font-size:14px; line-height:25.200000762939453px"><span style="background-color:rgb(247,252,255)"></span></span></span><pre name="code" class="java">        publicstaticBitmapreadBitMap(Contextcontext, intresId) {
            BitmapFactory.Optionsopt = newBitmapFactory.Options();
            opt.inPreferredConfig = Bitmap.Config.RGB_565;
            opt.inPurgeable = true;
            opt.inInputShareable = true;
            //获取资源图片 
            InputStreamis = context.getResources().openRawResource(resId);
            returnBitmapFactory.decodeStream(is, null, opt);
        }</pre></div>
<div class="column">
<div class="page" title="Page 20">
<div class="layoutArea">
<div class="column"><br>
<p><span style="line-height:25.200000762939453px"><strong><span style="font-family:'Microsoft YaHei'; font-size:18px">图片回收：</span></strong></span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; font-family:Arial; font-size:14px; line-height:26px">
使用Bitmap过后，就需要及时的调用<strong>Bitmap.recycle()</strong>方法来释放Bitmap占用的内存空间，而不要等Android系统来进行释放。</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; font-family:Arial; font-size:14px; line-height:26px">
下面是释放Bitmap的示例代码片段。</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
</p>
<pre name="code" class="java" style="font-family: Arial; font-size: 14px; line-height: 26px;">        // 先判断是否已经回收
        if(bitmap != null &amp;&amp; !bitmap.isRecycled()){
            // 回收并且置为null
            bitmap.recycle();
            bitmap = null;
        }
        System.gc();</pre><br>
</div>
<div class="column">
<p><span style="line-height:26px"><strong><span style="font-family:Microsoft YaHei; font-size:18px">捕获异常：</span></strong></span><br>
</p>
<p>经过上面这些优化后还会存在报OOM的风险，所以下面需要一道最后的关卡——捕获OOM异常：</p>
<p></p>
<pre name="code" class="java">        Bitmap bitmap = null;
        try {
            // 实例化Bitmap
            bitmap = BitmapFactory.decodeFile(path);
        } catch (OutOfMemoryError e) {
            // 捕获OutOfMemoryError，避免直接崩溃
        }
        if (bitmap == null) {
            // 如果实例化失败 返回默认的Bitmap对象
            return defaultBitmapMap;
        }</pre>
<div><br>
</div>
<br>
</div>
</div>
</div>
</div>
</div>
</div>
</div>
</div>
</div>
</div>
<br>
<p></p>
<p><span style="font-family:'Microsoft YaHei'; font-size:24px; color:rgb(153,102,51)"><strong>修改对象引用类型：</strong></span></p>
<p><br>
</p>
<p><strong><span style="font-family:Microsoft YaHei; font-size:18px">引用类型：</span></strong></p>
<p><span style="font-family:Arial; font-size:14px; line-height:26px">引用分为四种级别，这四种级别由高到低依次为：强引用&gt;软引用&gt;弱引用&gt;虚引用。</span><br>
</p>
<p><strong>强引用（strong reference）</strong><br>
如：Object object=new Object（），object就是一个强引用了。当内存空间不足，Java虚拟机宁愿抛出OutOfMemoryError错误，使程序异常终止，也不会靠随意回收具有强引用的对象来解决内存不足问题。<br>
</p>
<p><strong>软引用（SoftReference）</strong><br>
只有内存不够时才回收,常用于缓存；当内存达到一个阀&#20540;，GC就会去回收它；<br>
</p>
<p></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<strong>弱引用（WeakReference）&nbsp;&nbsp;&nbsp;</strong></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
弱引用的对象拥有更短暂的生命周期。在垃圾回收器线程扫描它 所管辖的内存区域的过程中，一旦发现了只具有弱引用的对象，不管当前内存空间足够与否，都会回收它的内存。&nbsp;</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<br>
</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<strong>虚引用（PhantomReference）</strong>&nbsp;&nbsp;&nbsp;</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
&quot;虚引用&quot;顾名思义，就是形同虚设，与其他几种引用都不同，虚引用并不会决定对象的生命周期。如果一个对象仅持有虚引用，那么它就和没有任何引用一样，在任何时候都可能被垃圾回收。 &nbsp;</p>
<p><br>
</p>
<p><strong><span style="font-family:Microsoft YaHei; font-size:18px">软引用和弱引用的应用实例：</span></strong><br>
</p>
<p><span style="font-size:14px; color:#ff6666"><span style="padding:0px; margin:0px"><span style="padding:0px; margin:0px">注意</span></span>：对于<span style="padding:0px; margin:0px"><span style="padding:0px; margin:0px">SoftReference</span></span>(<span style="padding:0px; margin:0px"><span style="padding:0px; margin:0px">软引用</span></span>)或者<span style="padding:0px; margin:0px"><span style="padding:0px; margin:0px">WeakReference</span></span>(<span style="padding:0px; margin:0px"><span style="padding:0px; margin:0px">弱引用</span></span>)的Bitmap缓存方案，现在已经不推荐使用了。自<span style="padding:0px; margin:0px">Android2.3</span>版本(<span style="padding:0px; margin:0px">API
 Level 9</span>)开始，垃圾回收器更着重于对软/弱引用的回收，</span><span style="color:rgb(255,102,102); font-size:14px">所以下面的内容可以选择忽略。</span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:5px; padding-bottom:5px">
在Android应用的开发中，为了防止内存溢出，在处理一些占用内存大而且声明周期较长的对象时候，可以尽量应用软引用和弱引用技术。</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:5px; padding-bottom:5px">
下面以使用软引用为例来详细说明（弱引用的使用方式与软引用是类&#20284;的）：</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:5px; padding-bottom:5px">
假设我们的应用会用到大量的默认图片，而且这些图片很多地方会用到。如果每次都去读取图片，由于读取文件需要硬件操作，速度较慢，会导致性能较低。所以我们考虑将图片缓存起来，需要的时候直接从内存中读取。但是，由于图片占用内存空间比较大，缓存很多图片需要很多的内存，就可能比较容易发生OutOfMemory异常。这时，我们可以考虑使用软引用技术来避免这个问题发生。</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:5px; padding-bottom:5px">
首先定义一个HashMap，保存软引用对象。<br>
</p>
<pre name="code" class="java">private Map&lt;String, SoftReference&lt;Bitmap&gt;&gt; imageCache = new HashMap&lt;String, SoftReference&lt;Bitmap&gt;&gt;();</pre>再来定义一个方法，保存Bitmap的软引用到HashMap。<br>
<p><span style="font-family:Arial; font-size:14px; line-height:26px"><span style="font-family:Arial; font-size:14px; line-height:26px"></span></span></p>
<pre name="code" class="java"> public void addBitmapToCache(String path) {
        // 强引用的Bitmap对象
        Bitmap bitmap = BitmapFactory.decodeFile(path);
        // 软引用的Bitmap对象
        SoftReference&lt;Bitmap&gt; softBitmap = new SoftReference&lt;Bitmap&gt;(bitmap);
        // 添加该对象到Map中使其缓存
        imageCache.put(path, softBitmap);
    }
</pre>获取的时候，可以通过SoftReference的get()方法得到Bitmap对象。<pre name="code" class="java" style="font-size: 14px; line-height: 25.200000762939453px;">public Bitmap getBitmapByPath(String path) {
        // 从缓存中取软引用的Bitmap对象
        SoftReference&lt;Bitmap&gt; softBitmap = imageCache.get(path);
        // 判断是否存在软引用
        if (softBitmap == null) {
            return null;
        }
        // 取出Bitmap对象，如果由于内存不足Bitmap被回收，将取得空
        Bitmap bitmap = softBitmap.get();
        return bitmap;
    }</pre>使用软引用以后，在OutOfMemory异常发生之前，这些缓存的图片资源的内存空间可以被释放掉的，从而避免内存达到上限，避免Crash发生。
<p>需要注意的是，在垃圾回收器对这个Java对象回收前，SoftReference类所提供的get方法会返回Java对象的强引用，一旦垃圾线程回收该Java对象之后，get方法将返回null。所以在获取软引用对象的代码中，一定要判断是否为null，以免出现NullPointerException异常导致应用崩溃。</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:5px; padding-bottom:5px">
<br>
</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:5px; padding-bottom:5px">
<span style="font-family:Microsoft YaHei; font-size:18px"><strong>到底什么时候使用软引用，什么时候使用弱引用呢？</strong></span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:5px; padding-bottom:5px">
个人认为，如果只是想避免OutOfMemory异常的发生，则可以使用软引用。如果对于应用的性能更在意，想尽快回收一些占用内存比较大的对象，则可以使用弱引用。</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:5px; padding-bottom:5px">
还有就是可以根据对象是否经常使用来判断。如果该对象可能会经常使用的，就尽量用软引用。如果该对象不被使用的可能性更大些，就可以用弱引用。</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:5px; padding-bottom:5px">
另外，和弱引用功能类&#20284;的是WeakHashMap。WeakHashMap对于一个给定的键，其映射的存在并不阻止垃圾回收器对该键的回收，回收以后，其条目从映射中有效地移除。WeakHashMap使用ReferenceQueue实现的这种机制。</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:5px; padding-bottom:5px">
<br>
</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:5px; padding-bottom:5px">
<strong><span style="font-family:Microsoft YaHei; font-size:24px; color:#996633">其他小tips：</span></strong></p>
</div>
<div class="column">
<p style="margin:10px auto; padding-top:0px; padding-bottom:0px"></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:5px; padding-bottom:5px">
<strong><span style="font-family:Microsoft YaHei; font-size:18px">对常量使用static final修饰符</span></strong></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:5px; padding-bottom:5px">
让我们来看看这两段在类前面的声明：</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:5px; padding-bottom:5px">
<strong>static </strong>int intVal = 42;<br>
<strong>static </strong>String strVal = &quot;Hello, world!&quot;;<br>
编译器会生成一个叫做clinit的初始化类的方法，当类第一次被使用的时候这个方法会被执行。方法会将42赋给intVal，然后把一个指向类中常量表 的引用赋给strVal。当以后要用到这些&#20540;的时候，会在成员变量表中查找到他们。 下面我们做些改进，使用“final”关键字：</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:5px; padding-bottom:5px">
<strong>static final </strong>int intVal = 42;<br>
<strong>static final </strong>String strVal = &quot;Hello, world!&quot;;</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:5px; padding-bottom:5px">
现在，类不再需要clinit方法，因为在成员变量初始化的时候，会将常量直接保存到类文件中。用到intVal的代码被直接替换成42，而使用strVal的会指向一个字符串常量，而不是使用成员变量。</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:5px; padding-bottom:5px">
将一个方法或类声明为final不会带来性能的提升，但是会帮助编译器优化代码。举例说，如果编译器知道一个getter方法不会被重载，那么编译器会对其采用内联调用。</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:5px; padding-bottom:5px">
你也可以将本地变量声明为final，同样，这也不会带来性能的提升。使用“final”只能使本地变量看起来更清晰些（但是也有些时候这是必须的，比如在使用匿名内部类的时候）。</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:5px; padding-bottom:5px">
</p>
<p style="margin:10px auto; padding-top:0px; padding-bottom:0px"><span style="margin:0px; padding:0px"><strong><span style="font-family:Microsoft YaHei; font-size:18px">静态方法代替虚拟方法</span></strong></span></p>
<p style="margin:10px auto; padding-top:0px; padding-bottom:0px">如果不需要访问某对象的字段，将方法设置为静态，调用会加速15%到20%。这也是一种好的做法，因为你可以从方法声明中看出调用该方法不需要更新此对象的状态。</p>
<p style="margin:10px auto; padding-top:0px; padding-bottom:0px"><br>
</p>
<p style="margin:10px auto; padding-top:0px; padding-bottom:0px"><span style="margin:0px; padding:0px"><strong><span style="font-family:Microsoft YaHei; font-size:18px">减少不必要的全局变量</span></strong></span></p>
<p style="margin:10px auto; padding-top:0px; padding-bottom:0px">尽量避免static成员变量引用资源耗费过多的实例,比如Context</p>
<p style="margin:10px auto; padding-top:0px; padding-bottom:0px"></p>
<div class="column">因为Context的引用超过它本身的生命周期，会导致Context泄漏。所以尽量使用Application这种Context类型。&nbsp;你可以通过调用Context.getApplicationContext()或 Activity.getApplication()轻松得到Application对象。&nbsp;</div>
<div class="column"><br>
</div>
<p style="margin:10px auto; padding-top:0px; padding-bottom:0px; line-height:21px">
<span style="margin:0px; padding:0px; line-height:21px"><span style="margin:0px; padding:0px"><strong><span style="font-family:Microsoft YaHei; font-size:18px">避免创建不必要的对象</span></strong></span></span><br>
</p>
<p style="margin:10px auto; padding-top:0px; padding-bottom:0px"><span style="margin:0px; padding:0px"><span style="margin:0px; padding:0px">最常见的例子就是当你要频繁操作一个字符串时，使用StringBuffer代替String。</span></span></p>
<p style="margin:10px auto; padding-top:0px; padding-bottom:0px"><span style="margin:0px; padding:0px">对于所有所有基本类型的组合：int数组比Integer数组好，这也概括了一个基本事实，两个平行的int数组比</span>&nbsp;(<span style="margin:0px; padding:0px">int</span>,<span style="margin:0px; padding:0px">int</span>)<span style="margin:0px; padding:0px">对象数组性能要好很多。</span></p>
<p style="margin:10px auto; padding-top:0px; padding-bottom:0px">总体来说，就是避免创建短命的临时对象。减少对象的创建就能减少垃圾收集，进而减少对用户体验的影响。</p>
<span style="margin:0px; padding:0px; font-family:Verdana,Arial,Helvetica,sans-serif; font-size:14px; line-height:21px"><strong></strong></span>
<div class="column"><span style="margin:0px; padding:0px; font-family:Verdana,Arial,Helvetica,sans-serif; font-size:14px; line-height:21px"><strong><br>
</strong></span></div>
<strong><span style="font-family:Microsoft YaHei; font-size:18px">避免内部Getters/Setters</span></strong></div>
<div class="column">在Android中，虚方法调用的代价比直接字段访问高昂许多。通常根据面向对象语言的实践，在公共接口中使用Getters和Setters是有道理的，但在一个字段经常被访问的类中宜采用直接访问。<br>
<br>
</div>
<div class="column">
<div class="page" title="Page 19">
<div class="layoutArea">
<div class="column">
<div class="page" title="Page 20">
<div class="layoutArea">
<div class="column">
<div class="page" title="Page 20">
<div class="layoutArea">
<div class="column">
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
</p>
<p style="margin:10px auto; padding-top:0px; padding-bottom:0px; line-height:21px">
<span style="margin:0px; padding:0px"><strong><span style="font-family:Microsoft YaHei; font-size:18px">避免使用浮点数</span></strong></span></p>
<p style="margin:10px auto; padding-top:0px; padding-bottom:0px">通常的经验是，在Android设备中，浮点数会比整型慢两倍。</p>
<p style="margin:10px auto; padding-top:0px; padding-bottom:0px"><br>
</p>
<p style="margin:10px auto; padding-top:0px; padding-bottom:0px; line-height:21px">
<span style="margin:0px; padding:0px"><strong><span style="font-family:Microsoft YaHei; font-size:18px">使用实体类比接口好</span></strong></span></p>
<p style="margin:10px auto; padding-top:0px; padding-bottom:0px"></p>
<p style="margin:10px auto; padding-top:0px; padding-bottom:0px">假设你有一个HashMap对象，你可以将它声明为HashMap或者Map：</p>
<pre style="margin-top:0px; margin-bottom:0px; padding:0px; word-wrap:break-word">Map map1 = new HashMap();
HashMap map2 = new HashMap();</pre>
<p style="margin:10px auto; padding-top:0px; padding-bottom:0px">哪个更好呢？</p>
<p style="margin:10px auto; padding-top:0px; padding-bottom:0px">按照传统的观点Map会更好些，因为这样你可以改变他的具体实现类，只要这个类继承自Map接口。传统的观点对于传统的程序是正确的，但是它并不适合嵌入式系统。调用一个接口的引用会比调用实体类的引用多花费一倍的时间。如果HashMap完全适合你的程序，那么使用Map就没有什么价&#20540;。如果有些地方你不能确定，先避免使用Map，剩下的交给IDE提供的重构功能好了。(当然公共API是一个例外：一个好的API常常会牺牲一些性能）</p>
<p style="margin:10px auto; padding-top:0px; padding-bottom:0px"><br>
</p>
<p style="margin:10px auto; padding-top:0px; padding-bottom:0px; line-height:21px">
<span style="margin:0px; padding:0px"><strong><span style="font-family:Microsoft YaHei; font-size:18px">避免使用枚举</span></strong></span></p>
<p style="margin:10px auto; padding-top:0px; padding-bottom:0px">枚举变量非常方便，但不幸的是它会牺牲执行的速度和并大幅增加文件体积。</p>
使用枚举变量可以让你的API更出色，并能提供编译时的检查。所以在通常的时候你毫无疑问应该为公共API选择枚举变量。但是当性能方面有所限制的时候，你就应该避免这种做法了。
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="font-family:Verdana,Arial,Helvetica,sans-serif; font-size:14px; line-height:21px"><br>
</span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="margin:0px; padding:0px; line-height:21px"><strong><span style="font-family:Microsoft YaHei; font-size:18px">for循环</span></strong></span><br>
</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="margin:0px; padding:0px">访问成员变量比访问本地变量慢得多，如下面一段代码：<br>
</span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="margin:0px; padding:0px"></span></p>
<pre name="code" class="java">for(int i =0; i &lt; this.mCount; i++)  {}</pre>
<p>永远不要在for的第二个条件中调用任何方法，如下面一段代码：</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="margin:0px; padding:0px"></span></p>
<pre name="code" class="java">for(int i =0; i &lt; this.getCount(); i++) {}</pre>
<p>对上面两个例子最好改为：</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="margin:0px; padding:0px"></span></p>
<pre name="code" class="java">int count = this.mCount; / int count = this.getCount();
for(int i =0; i &lt; count; i++)  {}</pre>在java1.5中引入的for-each语法。编译器会将对数组的引用和数组的长度保存到本地变量中，这对访问数组元素非常好。 但是编译器还会在每次循环中产生一个额外的对本地变量的存储操作<span style="font-family:Verdana,Arial,Helvetica,sans-serif; font-size:14px; line-height:21px">（如下面例子中的变量a）</span>，这样会比普通循环多出4个字节，速度要稍微慢一些：
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="margin:0px; padding:0px"><span style="font-family:Verdana,Arial,Helvetica,sans-serif"><span style="font-size:14px; line-height:21px"></span></span></span></p>
<pre name="code" class="java">for (Foo a : mArray) {
    sum += a.mSplat;
}</pre><br>
<p style="margin:10px auto; padding-top:0px; padding-bottom:0px"><span style="margin:0px; padding:0px"><strong><span style="font-family:Microsoft YaHei; font-size:18px">了解并使用类库</span></strong></span></p>
<p style="margin:10px auto; padding-top:0px; padding-bottom:0px">选择Library中的代码而非自己重写，除了通常的那些原因外，考虑到系统空闲时会用汇编代码调用来替代library方法，这可能比JIT中生成的等价的最好的Java代码还要好。</p>
<p style="margin:10px auto; padding-top:0px; padding-bottom:0px">当你在处理字串的时候，不要吝惜使用<strong>String.indexOf()</strong>，<strong>String.lastIndexOf()</strong>等特殊实现的方法。这些方法都是使用C/C&#43;&#43;实现的，比起Java循环快10到100倍。</p>
<p style="margin:10px auto; padding-top:0px; padding-bottom:0px"><strong>System.arraycopy</strong>方法在有JIT的Nexus One上，自行编码的循环快9倍。</p>
<p style="margin:10px auto; padding-top:0px; padding-bottom:0px">android.text.format包下的<strong>Formatter</strong>类，提供了IP地址转换、文件大小转换等方法；DateFormat类，提供了各种时间转换，都是非常高效的方法。</p>
<p style="margin:10px auto; padding-top:0px; padding-bottom:0px"><strong>TextUtils</strong>类，对于字符串处理Android为我们提供了一个简单实用的TextUtils类，如果处理比较简单的内容不用去思考正则表达式不妨试试这个在android.text.TextUtils的类</p>
<p style="margin:10px auto; padding-top:0px; padding-bottom:0px">高性能<strong>MemoryFile</strong>类，很多人抱怨Android处理底层I/O性能不是很理想，如果不想使用NDK则可以通过MemoryFile类实现高性能的文件读写操作。MemoryFile适用于哪些地方呢？对于I/O需要频繁操作的，主要是和外部存储相关的I/O操作，MemoryFile通过将 NAND或SD卡上的文件，分段映射到内存中进行修改处理，这样就用高速的RAM代替了ROM或SD卡，性能自然提高不少，对于Android手机而言同时还减少了电量消耗。该类实现的功能不是很多，直接从Object上继承，通过JNI的方式直接在C底层执行。</p>
<p style="margin:10px auto; padding-top:0px; padding-bottom:0px"><br>
</p>
<p style="margin:10px auto; padding-top:0px; padding-bottom:0px"><br>
</p>
<p style="text-align:center; margin:10px auto; padding-top:0px; padding-bottom:0px">
<strong><span style="font-family:Microsoft YaHei; font-size:32px; color:#cc0000">Reuse:</span></strong></p>
<div class="column"><span style="font-family:Tahoma,Helvetica,Arial,宋体,sans-serif"><span style="font-size:14px"><br>
</span></span></div>
<div class="column"><span style="font-family:Tahoma,Helvetica,Arial,宋体,sans-serif"><span style="font-size:14px">Reuse重用，减少内存消耗的重要手段之一。</span></span></div>
<div class="column"><span style="font-size:14px"><span style="font-family:Tahoma,Helvetica,Arial,宋体,sans-serif">核心思路就是将已经存在的内存资源重新使用而避免去创建新的，最典型的使用就是<strong>缓存（Cache</strong></span><span style="font-family:Tahoma,Helvetica,Arial,宋体,sans-serif; color:rgb(67,67,67); line-height:24px"><strong>）</strong>和<strong>池（Pool）</strong>。</span></span></div>
<div class="column"><span style="font-size:14px"><span style="font-family:Tahoma,Helvetica,Arial,宋体,sans-serif; color:rgb(67,67,67); line-height:24px"><br>
</span></span></div>
<div class="column"><span style="line-height:24px"><span style="color:rgb(153,102,51)"><strong><span style="font-family:Microsoft YaHei; font-size:24px">Bitmap缓存：</span></strong></span></span></div>
<div class="column"><span style="font-family:Tahoma,Helvetica,Arial,宋体,sans-serif; line-height:24px"><strong><span style="font-size:24px; color:#996633"><br>
</span></strong></span></div>
<div class="column">
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="font-size:18px">Bitmap缓存分为两种：</span></p>
<p style="color:rgb(67,67,67); font-family:Arial; font-size:14px; line-height:26px; margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="line-height:26px"><span style="color:rgb(67,67,67); font-family:Arial; font-size:14px; line-height:26px">一种是内存缓存，</span>一种是硬盘缓存。</span></p>
<p style="color:rgb(67,67,67); font-family:Arial; font-size:14px; line-height:26px; margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<span style="line-height:26px"><br>
</span></p>
<p><span style="font-family:Microsoft YaHei; font-size:18px"><strong>内存缓存（LruCache）：</strong></span></p>
<p>以牺牲宝贵的应用内存为代价，内存缓存提供了快速的Bitmap访问方式。系统提供的<span style="padding:0px; margin:0px"><span style="padding:0px; margin:0px">LruCache</span></span>类是非常适合用作缓存Bitmap任务的，它将最近被引用到的对象存储在一个强引用的<span style="padding:0px; margin:0px"><span style="padding:0px; margin:0px">LinkedHashMap</span></span>中，并且在缓存超过了指定大小之后将最近不常使用的对象释放掉。</p>
<p><span style="font-family:微软雅黑,Verdana,sans-serif,宋体; font-size:14px; line-height:26px; padding:0px; margin:0px; color:rgb(229,102,0)"><span style="padding:0px; margin:0px">注意</span></span><span style="color:rgb(67,67,67); font-family:微软雅黑,Verdana,sans-serif,宋体; font-size:14px; line-height:26px">：</span>以前有一个非常流行的内存缓存实现是<span style="padding:0px; margin:0px"><span style="padding:0px; margin:0px">SoftReference</span></span>(<span style="padding:0px; margin:0px"><span style="padding:0px; margin:0px">软引用</span></span>)或者<span style="padding:0px; margin:0px"><span style="padding:0px; margin:0px">WeakReference</span></span>(<span style="padding:0px; margin:0px"><span style="padding:0px; margin:0px">弱引用</span></span>)的Bitmap缓存方案，然而现在已经不推荐使用了。自<span style="padding:0px; margin:0px">Android2.3</span>版本(<span style="padding:0px; margin:0px">API
 Level 9</span>)开始，垃圾回收器更着重于对软/弱引用的回收，这使得上述的方案相当无效。<br>
</p>
<p><br>
</p>
<p><span style="font-family:Microsoft YaHei; font-size:18px"><strong>硬盘缓存（DiskLruCache）：</strong></span></p>
<p></p>
<p style="margin-top:0px; margin-bottom:15px; padding-top:0px; padding-bottom:0px">
<span style="padding:0px; margin:0px">一个内存缓存对加速访问最近浏览过的Bitmap非常有帮助，但是你不能局限于内存中的可用图片。GridView这样有着更大的数据集的组件可以很轻易消耗掉内存缓存。你的应用有可能在执行其他任务(如打电话)的时候被打断，并且在后台的任务有可能被杀死或者缓存被释放。一旦用户重新聚焦(<span style="padding:0px; margin:0px">resume</span>)到你的应用，你得再次处理每一张图片。</span></p>
<p style="margin-top:0px; margin-bottom:15px; padding-top:0px; padding-bottom:0px">
在这种情况下，硬盘缓存可以用来存储Bitmap并在图片被内存缓存释放后减小图片加载的时间(次数)。当然，从硬盘加载图片比内存要慢，并且应该在后台线程进行，因为硬盘读取的时间是不可预知的。</p>
<p style="color:rgb(67,67,67); font-family:微软雅黑,Verdana,sans-serif,宋体; font-size:14px; line-height:26px; margin-top:0px; margin-bottom:15px; padding-top:0px; padding-bottom:0px">
<span style="padding:0px; margin:0px"><span style="padding:0px; margin:0px; color:rgb(229,102,0)"><span style="padding:0px; margin:0px">注意</span></span>：如果访问图片的次数非常频繁，那么<span style="padding:0px; margin:0px; color:rgb(0,102,0)"><span style="padding:0px; margin:0px">ContentProvider</span></span>可能更适合用来存储缓存图片，例如<span style="padding:0px; margin:0px; color:rgb(0,102,0)">Image
 Gallery</span>这样的应用程序。</span></p>
更多关于内存缓存和硬盘缓存的内容请看Google官方教程<a target="_blank" target="_blank" href="https://developer.android.com/develop/index.html">https://developer.android.com/develop/index.html</a><br>
</div>
<div class="column"><span style="font-size:24px; color:#996633"><br>
</span></div>
<div class="column"><strong><span style="font-family:Microsoft YaHei; font-size:24px; color:#996633">图片缓存的开源项目：</span></strong></div>
<div class="column"><span style="font-family:Tahoma,Helvetica,Arial,宋体,sans-serif; font-size:14px; color:#434343"><span style="line-height:24px">对于图片的缓存现在都倾向于使用开源项目，这里我列出几个我搜到的：</span></span></div>
<div class="column"><span style="font-family:Tahoma,Helvetica,Arial,宋体,sans-serif; font-size:14px; color:#434343"><span style="line-height:24px"><br>
</span></span></div>
<div class="column"><span style="line-height:24px"></span>
<p style="margin-top:0px; margin-bottom:5px; padding-top:0px; padding-bottom:0px; border:0px; list-style:none; word-wrap:normal; word-break:normal; line-height:21px">
<span style="background-color:rgb(255,255,255)"><strong><span style="font-family:Microsoft YaHei; font-size:18px">1. Android-Universal-Image-Loader 图片缓存</span></strong></span></p>
<p style="font-family:simsun; font-size:14px; margin-top:0px; margin-bottom:5px; padding-top:0px; padding-bottom:0px; border:0px; list-style:none; word-wrap:normal; word-break:normal; line-height:21px">
<span style="background-color:rgb(255,255,255)">目前使用最广泛的图片缓存，支持主流图片缓存的绝大多数特性。<br>
项目地址：https://github.com/nostra13/Android-Universal-Image-Loader<br>
</span></p>
<p style="color:rgb(85,85,85); font-family:simsun; font-size:14px; margin-top:0px; margin-bottom:5px; padding-top:0px; padding-bottom:0px; border:0px; list-style:none; word-wrap:normal; word-break:normal; line-height:21px">
<span style="background-color:rgb(255,255,255)">&nbsp;<wbr></span></p>
<p style="margin-top:0px; margin-bottom:5px; padding-top:0px; padding-bottom:0px; border:0px; list-style:none; word-wrap:normal; word-break:normal; line-height:21px">
<span style="background-color:rgb(255,255,255)"><strong><span style="font-family:Microsoft YaHei; font-size:18px">2. picasso square开源的图片缓存</span></strong><br>
<span style="font-family:simsun; font-size:14px">项目地址：https://github.com/square/picasso</span><br>
<span style="font-family:simsun; font-size:14px">特点：(1)可以自动检测adapter的重用并取消之前的下载</span><br>
<span style="font-family:simsun; font-size:14px">(2)图片变换</span><br>
<span style="font-family:simsun; font-size:14px">(3)可以加载本地资源</span><br>
<span style="font-family:simsun; font-size:14px">(4)可以设置占位资源</span><br>
<span style="font-family:simsun; font-size:14px">(5)支持debug模式</span></span></p>
<p style="color:rgb(85,85,85); font-family:simsun; font-size:14px; margin-top:0px; margin-bottom:5px; padding-top:0px; padding-bottom:0px; border:0px; list-style:none; word-wrap:normal; word-break:normal; line-height:21px">
<span style="background-color:rgb(255,255,255)">&nbsp;<wbr></span></p>
<p style="margin-top:0px; margin-bottom:5px; padding-top:0px; padding-bottom:0px; border:0px; list-style:none; word-wrap:normal; word-break:normal; line-height:21px">
<span style="background-color:rgb(255,255,255)"><strong><span style="font-family:Microsoft YaHei; font-size:18px">3. ImageCache 图片缓存，包含内存和Sdcard缓存</span></strong><br>
<span style="font-family:simsun; font-size:14px">项目地址：https://github.com/Trinea/AndroidCommon</span><br>
<span style="font-family:simsun; font-size:14px">特点：</span></span></p>
<p style="margin-top:0px; margin-bottom:5px; padding-top:0px; padding-bottom:0px; border:0px; list-style:none; word-wrap:normal; word-break:normal; line-height:21px">
<span style="background-color:rgb(255,255,255)"><span style="font-family:simsun; font-size:14px">(1)支持预取新图片，支持等待队列</span><br>
<span style="font-family:simsun; font-size:14px">(2)包含二级缓存，可自定义文件名保存规则</span><br>
<span style="font-family:simsun; font-size:14px">(3)可选择多种缓存算法(FIFO、LIFO、LRU、MRU、LFU、MFU等13种)或自定义缓存算法</span><br>
<span style="font-family:simsun; font-size:14px">(4)可方便的保存及初始化恢复数据</span><br>
<span style="font-family:simsun; font-size:14px">(5)支持不同类型网络处理</span><br>
<span style="font-family:simsun; font-size:14px">(6)可根据系统配置初始化缓存等</span></span></p>
<br>
</div>
<div class="column"><span style="line-height:24px"><strong><span style="font-family:Microsoft YaHei; font-size:18px">4.&nbsp;Android 网络通信框架Volley</span></strong></span></div>
<div class="column"><span style="font-size:14px"><span style="font-family:Tahoma,Helvetica,Arial,宋体,sans-serif; line-height:24px"><span style="font-family:Arial; font-size:14px; line-height:26px; orphans:2; widows:2">项目地址：https://android.googlesource.com/platform/frameworks/volley</span></span></span></div>
<div class="column"><span style="font-size:14px"><span style="line-height:24px; font-family:Tahoma,Helvetica,Arial,宋体,sans-serif"><span style="orphans:2; widows:2; line-height:26px; font-size:14px; font-family:Arial">我们在程序中需要和网络通信的时候，大体使用的东西莫过于AsyncTaskLoader，HttpURLConnection，AsyncTask，HTTPClient（Apache）等，在2013年的Google
 I/O发布了Volley。Volley是Android平台上的网络通信库，能使网络通信更快，更简单，更健壮。</span></span></span></div>
<div class="column"><span style="line-height:21px; font-variant:inherit; font-style:inherit; font-size:14px; font-family:simsun">特点：</span></div>
<div class="column"><span style="orphans:2; widows:2; line-height:inherit; font-variant:inherit; font-style:inherit; font-size:14px; font-family:inherit"><span style="line-height:21px; font-size:14px; font-family:simsun">(1)</span>JSON，图像等的异步下载；</span></div>
<div class="column"><span style="orphans:2; widows:2; line-height:inherit; font-variant:inherit; font-style:inherit; font-size:14px; font-family:inherit"><span style="line-height:21px; font-size:14px; font-family:simsun">(2)</span>网络请求的排序（scheduling）</span></div>
<div class="column"><span style="orphans:2; widows:2; line-height:inherit; font-variant:inherit; font-style:inherit; font-size:14px; font-family:inherit"><span style="line-height:21px; font-size:14px; font-family:simsun">(3)</span>网络请求的优先级处理</span></div>
<div class="column"><span style="orphans:2; widows:2; line-height:inherit; font-variant:inherit; font-style:inherit; font-size:14px; font-family:inherit"><span style="line-height:21px; font-size:14px; font-family:simsun">(4)</span>缓存</span></div>
<div class="column"><span style="orphans:2; widows:2; line-height:inherit; font-variant:inherit; font-style:inherit; font-size:14px; font-family:inherit"><span style="line-height:21px; font-size:14px; font-family:simsun">(5)</span>多级别取消请求</span></div>
<div class="column"><span style="orphans:2; widows:2; line-height:inherit; font-variant:inherit; font-style:inherit; font-size:14px; font-family:inherit"><span style="font-family:simsun; font-size:14px; line-height:21px">(6)</span>和Activity和生命周期的联动（Activity结束时同时取消所有网络请求）</span></div>
<div class="column"><span style="font-size:14px"><span style="font-family:Tahoma,Helvetica,Arial,宋体,sans-serif; color:rgb(67,67,67); line-height:24px"><br>
</span></span></div>
<div class="column">
<div class="column"><span style="line-height:25.200000762939453px"><span style="font-family:Microsoft YaHei; font-size:24px; color:#996633"><strong>Adapter适配器</strong></span></span></div>
<div class="column"><br>
</div>
<div class="column">在Android中Adapter使用十分广泛，特别是在list中。所以adapter是数据的<strong> “集散地”</strong> ，所以对其进行内存优化是很有必要的。</div>
<div class="column">下面算是一个标准的使用模版：</div>
<div class="column">主要使用convertView和ViewHolder来进行缓存处理</div>
<div class="column">
<p></p>
<pre name="code" class="java">    @Override
    public View getView(int position, View convertView, ViewGroup parent) {
        ViewHolder vHolder = null;
        //如果convertView对象为空则创建新对象，不为空则复用  
        if (convertView == null) {
            convertView = inflater.inflate(..., null);
            // 创建 ViewHodler 对象  
            vHolder = new ViewHolder();
            vHolder.img= (ImageView) convertView.findViewById(...);
            vHolder.tv= (TextView) convertView.findViewById(...);
            // 将ViewHodler保存到Tag中(Tag可以接收Object类型对象，所以任何东西都可以保存在其中)
            convertView.setTag(vHolder);
        } else {
            //当convertView不为空时，通过getTag()得到View  
            vHolder = (ViewHolder) convertView.getTag();
        }
        // 给对象赋值，修改显示的值  
        vHolder.img.setImageBitmap(...);
        vHolder.tv.setText(...);
        return convertView;
    }
    //将显示的View 包装成类  
    static class ViewHolder {
        TextView tv;
        ImageView img;
    }</pre>
<div><br>
</div>
</div>
<br>
</div>
<div class="column"><span style="line-height:24px"><span style="font-family:Microsoft YaHei; font-size:24px; color:#996633"><strong>池（PooL）</strong></span></span></div>
<div class="column"><br>
</div>
<p><span style="line-height:1.5; color:rgb(75,75,75)"><span style="font-family:Microsoft YaHei; font-size:18px"><strong>对象池：</strong></span></span></p>
<p>对象池使用的基本思路是：将用过的对象保存起来，等下一次需要这种对象的时候，再拿出来重复使用，从而在一定程度上减少频繁创建对象所造成的开销。&nbsp;并非所有对象都适合拿来池化――因为维护对象池也要造成一定开销。对生成时开销不大的对象进行池化，反而可能会出现“维护对象池的开销”大于“生成新对象的开销”，从而使性能降低的情况。但是对于生成时开销可观的对象，池化技术就是提高性能的有效策略了。</p>
<p><br>
</p>
<p></p>
<div class="column"><span style="color:rgb(67,67,67); line-height:24px"><span style="color:rgb(68,68,68); line-height:21px"><strong><span style="font-family:Microsoft YaHei; font-size:18px">线程池：</span></strong></span></span></div>
<div class="column">
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
线程池的基本思想还是一种对象池的思想，开辟一块内存空间，里面存放了众多(未死亡)的线程，池中线程执行调度由池管理器来处理。当有线程任务时，从池中取一个，执行完成后线程对象归池，这样可以避免反复创建线程对象所带来的性能开销，节省了系统的资源。</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
比如：一个应用要和网络打交道，有很多步骤需要访问网络，为了不阻塞主线程，每个步骤都创建个线程，在线程中和网络交互，用线程池就变的简单，线程池是对线程的一种封装，让线程用起来更加简便，只需要创一个线程池，把这些步骤像任务一样放进线程池，在程序销毁时只要调用线程池的销毁函数即可。</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<br>
</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
java提供了<strong>ExecutorService</strong>和<strong>Executors</strong>类，我们可以应用它去建立线程池。</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
通常可以建立如下4种：</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
</p>
<pre name="code" class="java">/** 每次只执行一个任务的线程池 */
ExecutorService singleTaskExecutor =  Executors.newSingleThreadExecutor();

/** 每次执行限定个数个任务的线程池 */
ExecutorService limitedTaskExecutor = Executors.newFixedThreadPool(3);

/** 所有任务都一次性开始的线程池 */
ExecutorService allTaskExecutor = Executors.newCachedThreadPool();

/** 创建一个可在指定时间里执行任务的线程池，亦可重复执行 */
ExecutorService scheduledTaskExecutor = Executors.newScheduledThreadPool(3);</pre>
<p></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px">
<br>
</p>
更多关于线程池的内容我推荐这篇文章：<a target="_blank" target="_blank" href="http://www.xuanyusong.com/archives/2439">http://www.xuanyusong.com/archives/2439</a><br>
<br>
</div>
<div><span style="font-family:Microsoft YaHei; font-size:24px; color:#996633"><strong>注意：</strong></span></div>
<p></p>
<p style="margin:10px auto; padding-top:0px; padding-bottom:0px; font-family:Verdana,Arial,Helvetica,sans-serif; font-size:14px; line-height:21px">
要根据情况适度使用缓存，因为内存有限。</p>
<p style="margin:10px auto; padding-top:0px; padding-bottom:0px; font-family:Verdana,Arial,Helvetica,sans-serif; font-size:14px; line-height:21px">
能保存路径地址的就不要存放图片数据，不经常使用的尽量不要缓存，不用时就清空。</p>
<div><br>
</div>
<div><br>
</div>
<p></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; font-family:Arial; font-size:14px; line-height:26px">
<span style="font-family:'Microsoft YaHei'; font-size:24px; color:rgb(204,0,0)"><strong>写在最后：</strong></span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; font-family:Arial; font-size:14px; line-height:26px">
<span style="font-family:SimSun; font-size:14px"></span></p>
<p style="font-size:13.3333339691162px"><span style="font-size:18px">我准备将文章分为上、中、下三部分。现在已经全部完成：</span></p>
<p style="font-size:13.3333339691162px"><span style="font-family:'Microsoft YaHei'; font-size:18px"><strong>内存简介，Recoken（计算）</strong></span><span style="font-size:18px">请看</span><span style="font-family:'Microsoft YaHei'"><strong><span style="font-size:18px">：</span><a target="_blank" target="_blank" href="http://blog.csdn.net/a396901990/article/details/37914465" style="text-decoration:none; line-height:30px"><span style="font-size:18px; color:rgb(255,0,0)">ANDROID内存优化(大汇总——上)</span></a></strong></span></p>
<p style="font-size:13.3333339691162px"><span style="font-size:18px"><span style="font-family:'Microsoft YaHei'"><strong>Reduce（减少），Reuse（重用）&nbsp;</strong></span><span style="font-family:SimSun">请看：<a target="_blank" target="_blank" href="http://blog.csdn.net/a396901990/article/details/38707007" style="text-decoration:none; font-family:'Microsoft YaHei'; line-height:30px"><span style="color:rgb(255,0,0)"><strong>ANDROID内存优化(大汇总——中)</strong></span></a></span></span></p>
<p style="font-size:13.3333339691162px"><span style="font-family:'Microsoft YaHei'; font-size:18px"><strong>Recycle（回收）, Review（检查）&nbsp;</strong></span><span style="font-family:SimSun"><span style="font-size:18px">请看：</span><a target="_blank" href="http://blog.csdn.net/a396901990/article/details/38904543" style="text-decoration:none; font-family:'Microsoft YaHei'; line-height:30px"><span style="font-size:18px; color:#ff0000"><strong>ANDROID内存优化(大汇总——全)</strong></span></a></span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; font-family:Arial; font-size:14px; line-height:26px">
<span style="font-family:SimSun; font-size:14px"></span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; font-family:Arial; line-height:26px">
<span style="font-family:SimSun"><span style="font-size:18px"><br>
</span></span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; font-family:Arial; line-height:26px">
<span style="font-family:SimSun"><span style="font-size:18px">写这篇文章的目的就是想弄一个大汇总，将零散的内存知识点总结一下，如果有错误、不足或建议都希望告诉我。</span><br>
</span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; font-family:Arial; font-size:14px; line-height:26px">
<span style="font-weight:bold"><span style="color:rgb(204,0,0)"><span style="font-family:SimSun; font-size:14px"><br>
</span></span></span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; font-family:Arial; font-size:14px; line-height:26px">
<span style="font-weight:bold"><span style="color:rgb(204,0,0)"><span style="font-family:SimSun; font-size:14px"><br>
</span></span></span></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; font-family:Arial; font-size:14px; line-height:26px">
<span style="font-family:'Microsoft YaHei'; font-size:24px; color:rgb(204,0,0)"><strong>参考文章：</strong></span></p>
解析Android开发优化之:软引用与弱引用的应用（http://www.jb51.net/article/36627.htm）<br style="font-family:Arial; font-size:14px; line-height:26px">
<span style="font-family:Arial; font-size:14px; line-height:26px">android内存泄露优化总结（http://blog.csdn.net/imain/article/details/8560986）</span><br style="font-family:Arial; font-size:14px; line-height:26px">
<span style="font-family:Arial; font-size:14px; line-height:26px">Android 内存优化（http://blog.csdn.net/awangyunke/article/details/20380719）</span><br style="font-family:Arial; font-size:14px; line-height:26px">
<span style="font-family:Arial; font-size:14px; line-height:26px">Android开发优化之——对Bitmap的内存优化（http://blog.csdn.net/arui319/article/details/7953690）</span><br style="font-family:Arial; font-size:14px; line-height:26px">
<span style="font-family:Arial; font-size:14px; line-height:26px">关于android性能，内存优化（http://www.cnblogs.com/zyw-205520/archive/2013/02/17/2914190.html）</span>
<p><span style="font-family:Arial; font-size:14px; line-height:26px">Android研究院之应用开发线程池的经典使用（http://www.xuanyusong.com/archives/2439）</span></p>
<p><br>
</p>
<p><br>
</p>
</div>
</div>
</div>
</div>
</div>
</div>
</div>
</div>
</div>
</div>
</div>
</div>
</div>


