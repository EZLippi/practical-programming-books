<p>现在几乎任何一个网站、Web App以及移动APP等应用都需要有图片展示的功能，对于图片功能从下至上都是很重要的。必须要具有前瞻性的规划好图片服务器，图片的上传和下载速度至关重要，当然这并不是说一上来就搞很NB的架构，至少具备一定扩展性和稳定性。虽然各种架构设计都有，在这里我只是谈谈我的一些个人想法。</p>
<p><span id="more-967"></span></p>
<p>&nbsp;</p>
<p>对于图片服务器来说IO无疑是消耗资源最为严重的，对于web应用来说需要将图片服务器做一定的分离，否则很可能因为图片服务器的IO负载导致应用崩溃。因此尤其对于大型网站和应用来说，非常有必要将图片服务器和应用服务器分离，构建独立的图片服务器集群，构建独立的图片服务器其主要优势：</p>
<p>1）分担Web服务器的I/O负载-将耗费资源的图片服务分离出来，提高服务器的性能和稳定性。</p>
<p>2）能够专门对图片服务器进行优化-为图片服务设置有针对性的缓存方案，减少带宽网络成本，提高访问速度。</p>
<p>3）提高网站的可扩展性-通过增加图片服务器，提高图片服务吞吐能力。</p>
<p>&nbsp;</p>
<p>从传统互联网的web1.0，历经web2.0时代以及发展到现在的web3.0，随着图片存储规模的增加，图片服务器的架构也在逐渐发生变化，以下主要论述三个阶段的图片服务器架构演进。</p>
<p>&nbsp;</p>
<p><strong> 初始阶段</strong></p>
<p><a href="http://blog.aliyun.com/wp-content/uploads/2014/06/17.png"><img class="alignnone  wp-image-968" alt="1" src="http://blog.aliyun.com/wp-content/uploads/2014/06/17.png" width="492" height="224" /></a></p>
<p>在介绍初始阶段的早期的小型图片服务器架构之前，首先让我们了解一下NFS技术，NFS是Network File System的缩写，即网络文件系统。NFS是由Sun开发并发展起来的一项用于在不同机器，不同操作系统之间通过网络互相分享各自的文件。NFS server也可以看作是一个FILE SERVER,用于在UNIX类系统之间共享文件，可以轻松的挂载(mount)到一个目录上，操作起来就像本地文件一样的方便。</p>
<p>&nbsp;</p>
<p>如果不想在每台图片服务器同步所有图片，那么NFS是最简单的文件共享方式。NFS是个分布式的客户机/服务器文件系统，NFS的实质在于用户间计算机的共享，用户可以联结到共享计算机并象访问本地硬盘一样访问共享计算机上的文件。具体实现思路是：</p>
<p>&nbsp;</p>
<p>1)所有前端web服务器都通过nfs挂载3台图片服务器export出来的目录，以接收web服务器写入的图片。然后[图片1]服务器挂载另外两台图片服务器的export目录到本地给apache对外提供访问。<br />
2） 用户上传图片<br />
用户通过Internet访问页面提交上传请求post到web服务器，web服务器处理完图片后由web服务器拷贝到对应的mount本地目录。<br />
3）用户访问图片<br />
用户访问图片时，通过[图片1]这台图片服务器来读取相应mount目录里边的图片。</p>
<p>&nbsp;</p>
<p>以上架构存在的问题：</p>
<p>1）性能：现有结构过度依赖nfs,当图片服务器的nfs服务器有问题时，可能影响到前端web服务器。NFS的问题主要是锁的问题. 很容易造成死锁, 只有硬件重启才能解决。尤其当图片达到一定的量级后，nfs会有严重的性能问题。<br />
2）高可用：对外提供下载的图片服务器只有一台，容易出现单点故障。<br />
3） 扩展性：图片服务器之间的依赖过多，而且横向扩展余地不够。<br />
4） 存储：web服务器上传热点不可控，造成现有图片服务器空间占用不均衡。<br />
5） 安全性：nfs方式对于拥有web服务器的密码的人来说，可以随意修改nfs里边的内容，安全级别不高。</p>
<p>&nbsp;</p>
<p>当然图片服务器的图片同步可以不采用NFS,也可以采用ftp或rsync，采用ftp这样的话每个图片服务器就都保存一份图片的副本，也起到了备份的作用。但是缺点是将图片ftp到服务器比较耗时，如果使用异步方式去同步图片的话又会有延时，不过一般的小图片文件也还好了。使用rsync同步，当数据文件达到一定的量级后，每次rsync扫描会耗时很久也会带来一定的延时性。</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p><strong>发展阶段</strong></p>
<p>&nbsp;</p>
<p><a href="http://blog.aliyun.com/wp-content/uploads/2014/06/23.png"><img class="alignnone  wp-image-969" alt="2" src="http://blog.aliyun.com/wp-content/uploads/2014/06/23.png" width="478" height="224" /></a></p>
<p>&nbsp;</p>
<p>当网站达到一定的规模后，对图片服务器的性能和稳定性有一定的要求后，上述NFS图片服务架构面临着挑战，严重的依赖NFS,而且系统存在单点机器容易出现故障，需要对整体架构进行升级。于是出现了上图图片服务器架构，出现了分布式的图片存储。</p>
<p>&nbsp;</p>
<p>其实现的具体思路如下：</p>
<p>1）用户上传图片到web服务器后，web服务器处理完图片，然后再由前端web服务器把图片post到到[图片1]、[图片2]&#8230;[图片N]其中的一个，图片服务器接收到post过来的图片，然后把图片写入到本地磁盘并返回对应成功状态码。前端web服务器根据返回状态码决定对应操作，如果成功的话，处理生成各尺寸的缩略图、打水印，把图片服务器对应的ID和对应图片路径写入DB数据库。<br />
2) 上传控制<br />
我们需要调节上传时，只需要修改web服务器post到的目的图片服务器的ID，就可以控制上传到哪台图片存储服务器,对应的图片存储服务器只需要安装nginx同时提供一个python或者php服务接收并保存图片，如果不想不想开启python或者php服务，也可以编写一个nginx扩展模块。</p>
<p>3) 用户访问流程<br />
用户访问页面的时候，根据请求图片的URL到对应图片服务器去访问图片。</p>
<p>如： http://imgN.xxx.com/image1.jpg</p>
<p>&nbsp;</p>
<p>此阶段的图片服务器架构，增加了负载均衡和分布式图片存储，能够在一定程度上解决并发访问量高和存储量大的问题。负载均衡在有一定财力的情况下可以考虑F5硬负载，当然也可以考虑使用开源的LVS软负载(同时还可开启缓存功能)。此时将极大提升访问的并发量，可以根据情况随时调配服务器。当然此时也存在一定的瑕疵，那就是可能在多台Squid上存在同一张图片，因为访问图片时可能第一次分到squid1，在LVS过期后第二次访问到squid2或者别的，当然相对并发问题的解决，此种少量的冗余完全在我们的允许范围之内。在该系统架构中二级缓存可以使用squid也可以考虑使用varnish或者traffic server，对于cache的开源软件选型要考率以下几点</p>
<p>&nbsp;</p>
<p>1）性能：varnish本身的技术上优势要高于squid，它采用了“Visual Page Cache”技术，在内存的利用上，Varnish比Squid具有优势，它避免了Squid频繁在内存、磁盘中交换文件，性能要比Squid高。varnish是不能cache到本地硬盘上的。还有强大的通过Varnish管理端口，可以使用正则表达式快速、批量地清除部分缓存。nginx是用第三方模块ncache做的缓冲，其性能基本达到varnish，但在架构中nginx一般作为反向（静态文件现在用nginx的很多，并发能支持到2万+）。在静态架构中，如果前端直接面对的是cdn活着前端了4层负载的话，完全用nginx的cache就够了。</p>
<p>&nbsp;</p>
<p>2）避免文件系统式的缓存，在文件数据量非常大的情况下，文件系统的性能很差，像squid,nginx的proxy_store,proxy_cache之类的方式缓存，当缓存的量级上来后，性能将不能满足要求。开源的traffic server直接用裸盘缓存，是一个不错的选择，国内大规模应用并公布出来的主要是淘宝，并不是因为它做的差，而是开源时间晚。Traffic Server 在 Yahoo 内部使用了超过 4 年，主要用于 CDN 服务，CDN 用于分发特定的HTTP 内容，通常是静态的内容如图片、JavaScript、CSS。当然使用leveldb之类的做缓存，我估计也能达到很好的效果。</p>
<p>&nbsp;</p>
<p>3）稳定性：squid作为老牌劲旅缓存，其稳定性更可靠一些，从我身边一些使用者反馈来看varnish偶尔会出现crash的情况。Traffic Server在雅虎目前使用期间也没有出现已知的数据损坏情况，其稳定性相对也比较可靠，对于未来我其实更期待Traffic Server在国内能够拥有更多的用户。</p>
<p align="left">        以上图片服务架构设计消除了早期的NFS依赖以及单点问题，时能够均衡图片服务器的空间，提高了图片服务器的安全性等问题，但是又带来一个问题是图片服务器的横向扩展冗余问题。只想在普通的硬盘上存储，首先还是要考虑一下物理硬盘的实际处理能力。是 7200 转的还是 15000 转的，实际表现差别就很大。至于文件系统选择<a href="https://www.google.com.hk/url?sa=t&amp;rct=j&amp;q=&amp;esrc=s&amp;source=web&amp;cd=4&amp;cad=rja&amp;ved=0CEQQFjAD&amp;url=http://blog.csdn.net/upnoa/article/details/6977480&amp;ei=Q4SlUo__DejD2QW80oCoAg&amp;usg=AFQjCNFP84MRoF2tGtGO2UB7kck4NcJJ1g&amp;bvm=bv.57752919,d.dGI">xfs、ext3、ext4还是reiserFs</a>，需要做一些性能方面的测试，从官方的一些测试数据来看，reiserFs更适合存储一些小图片文件。创建文件系统的时候 Inode 问题也要加以考虑，选择合适大小的 inode size ，因为Linux 为每个文件分配一个称为索引节点的号码inode，可以将inode简单理解成一个指针，它永远指向本文件的具体存储位置。一个文件系统允许的inode节点数是有限的，如果文件数量太多，即使每个文件都是0字节的空文件，系统最终也会因为节点空间耗尽而不能再创建文件，因此需要在空间和速度上做取舍，构造合理的文件目录索引。</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p><strong>云存储阶段</strong></p>
<p><a href="http://blog.aliyun.com/wp-content/uploads/2014/06/33.png"><img class="alignnone  wp-image-970" alt="3" src="http://blog.aliyun.com/wp-content/uploads/2014/06/33.png" width="494" height="241" /></a></p>
<p>&nbsp;</p>
<p>2011年李彦宏在百度联盟峰会上就提到过互联网的读图时代已经到来，图片服务早已成为一个互联网应用中占比很大的部分，对图片的处理能力也相应地变成企业和开发者的一项基本技能,图片的下载和上传速度显得更加重要，要想处理好图片，需要面对的三个主要问题是：大流量、高并发、海量存储。</p>
<p>&nbsp;</p>
<p>阿里云存储服务(OpenStorageService，简称OSS)，是阿里云对外提供的海量，安全，低成本，高可靠的云存储服务。用户可以通过简单的 REST接口，在任何时间、任何地点上传和下载数据，也可以使用WEB页面对数据进行管理。同时，OSS提供Java、Python、PHP SDK，简化用户的编程。基于OSS，用户可以搭建出各种多媒体分享网站、网盘、个人企业数据备份等基于大规模数据的服务。在以下图片云存储主要以阿里云的云存储OSS为切入点介绍，上图为OSS云存储的简单架构示意图。</p>
<p>&nbsp;</p>
<p>真正意义上的“云存储”，不是存储而是提供云服务，使用云存储服务的主要优势有以下几点：</p>
<p>1)用户无需了解存储设备的类型、接口、存储介质等。</p>
<p>2)无需关心数据的存储路径。</p>
<p>3)无需对存储设备进行管理、维护。</p>
<p>4)无需考虑数据备份和容灾</p>
<p>5)简单接入云存储，尽情享受存储服务。</p>
<h2></h2>
<h2></h2>
<h2></h2>
<p>&nbsp;</p>
<p>&nbsp;</p>
<h2><strong>架构模块组成</strong></h2>
<p><a href="http://blog.aliyun.com/wp-content/uploads/2014/06/41.png"><img class="alignnone  wp-image-971" alt="4" src="http://blog.aliyun.com/wp-content/uploads/2014/06/41.png" width="547" height="369" /></a></p>
<p>1）KV Engine</p>
<p>OSS中的Object源信息和数据文件都是存放在KV Engine上。在6.15的版本，V Engine将使用0.8.6版本，并使用为OSS提供的OSSFileClient。</p>
<p>&nbsp;</p>
<p>2）Quota</p>
<p>此模块记录了Bucket和用户的对应关系，和以分钟为单位的Bucket资源使用情况。Quota还将提供HTTP接口供Boss系统查询。</p>
<p>&nbsp;</p>
<p>3）安全模块</p>
<p>安全模块主要记录User对应的ID和Key，并提供OSS访问的用户验证功能。</p>
<h2></h2>
<h2></h2>
<p>&nbsp;</p>
<h2><strong>OSS术语名词汇</strong></h2>
<p>&nbsp;</p>
<p>1 ）Access Key ID &amp; Access Key Secret （API密钥）<br />
用户注册OSS时，系统会给用户分配一对Access Key ID &amp; Access Key Secret，称为ID对，用于标识用户，为访问OSS做签名验证。</p>
<p>&nbsp;</p>
<p>2） Service<br />
OSS提供给用户的虚拟存储空间，在这个虚拟空间中，每个用户可拥有一个到多个Bucket。</p>
<p>&nbsp;</p>
<p>3） Bucket<br />
Bucket是OSS上的命名空间；Bucket名在整个OSS中具有全局唯一性，且不能修改；存储在OSS上的每个Object必须都包含在某个Bucket中。一个应用，例如图片分享网站，可以对应一个或多个Bucket。一个用户最多可创建10个Bucket，但每个Bucket中存放的Object的数量和大小总和没有限制，用户不需要考虑数据的可扩展性。</p>
<p>4） Object<br />
在OSS中，用户的每个文件都是一个Object，每个文件需小于5TB。Object包含key、data和user meta。其中，key是Object的名字；data是Object的数据；user meta是用户对该object的描述。<br />
其使用方式非常简单,如下为java sdk：</p>
<p>OSSClient ossClient = new OSSClient(accessKeyId,accessKeySecret);</p>
<p>PutObjectResult result = ossClient.putObject(bucketname,          bucketKey, inStream,  new ObjectMetadata());</p>
<p>执行以上代码即可将图片流上传至OSS服务器上。</p>
<p>图片的访问方式也非常简单其url为：<a href="http://bucketname.oss.aliyuncs.com/bucketKey">http://bucketname.oss.aliyuncs.com/bucketKey</a></p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p><strong>分布式文件系统</strong></p>
<p>用分布式存储有几个好处，分布式能自动提供冗余，不需要我们去备份，担心数据安全，在文件数量特别大的情况下，备份是一件很痛苦的事情，rsync扫一次可能是就是好几个小时，还有一点就是分布式存储动态扩容方便。当然在国内的其他一些文件系统里，TFS（<a href="http://code.taobao.org/p/tfs/src/">http://code.taobao.org/p/tfs/src/</a>）和FASTDFS也有一些用户，但是TFS的优势更是针对一些小文件存储，主要是淘宝在用。另外FASTDFS在并发高于300写入的情况下出现性能问题，稳定性不够友好。OSS存储使用的是阿里云基于飞天5k平台自主研发的高可用，高可靠的分布式文件系统盘古。分布式文件系统盘古和Google的GFS类似，盘古的架构是Master-Slave主从架构，Master负责元数据管理，Sliave叫做Chunk Server，负责读写请求。其中Master是基于Paxos的多Master架构，一个Master死了之后，另外一个Master可以很快接过去，基本能够做到故障恢复在一分钟以内 。文件是按照分片存放，每个会分三个副本，放在不同的机架上，最后提供端到端的数据校验。</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p><strong>HAPROXY负载均衡</strong></p>
<p>基于haproxy的自动hash架构 ,这是一种新的缓存架构，由nginx作为最前端，代理到缓存机器。 nginx后面是缓存组，由nginx经过url hash后将请求分到缓存机器。<br />
这个架构方便纯squid缓存升级，可以在squid的机器上加装nginx。 nginx有缓存的功能，可以将一些访问量特大的链接直接缓存在nginx上，就不用经过多一次代理的请求，能够保证图片服务器的高可用、高性能。比如favicon.ico和网站的logo。 负载均衡负责OSS所有的请求的负载均衡，后台的http服务器故障会自动切换，从而保证了OSS的服务不间断。</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p><strong>CDN</strong></p>
<p>阿里云CDN服务是一个遍布全国的分布式缓存系统，能够将网站文件（如图片或JavaScript代码文件）缓存到全国多个城市机房中的服务器上，当一个用户访问你的网站时，会就近到靠近TA的城市的服务器上获取数据，这样最终用户访问你的服务速度会非常快。</p>
<p>阿里云CDN服务在全国部署超过100个节点，能提供给用户优良的网络加速效果。当网站业务突然爆发增长时，无需手忙脚乱地扩容网络带宽，使用CDN服务即可轻松应对。和OSS服务一样，使用CDN，需要先在aliyun.com网站上开通CDN服务。开通后，需要在网站上的管理中心创建你的distribution（即分发频道），每个distribution由两个必须的部分组成：distribution ID和源站地址。</p>
<p>使用阿里云OSS和CDN可以非常方便的针对每个bucket进行内容加速，因为每个bucket对应一个独立的二级域名，针对每个文件进行CDN删除，简单、经济地解决服务的存储和网络问题，毕竟大多数网站或应用的存储和网络带宽多半是被图片或视频消耗掉的。</p>
<p>从整个业界来看，最近这样的面向个人用户的云存储如国外的DropBox和Box.net非常受欢迎，国内的云存储目前比较不错的主要有七牛云存储和又拍云存储。</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p><strong>上传下载分而治之</strong></p>
<p>图片服务器的图片下载比例远远高于上传比例，业务逻辑的处理也区别明显，上传服器对图片重命名，记录入库信息，下载服务器对图片添加水印、修改尺寸之类的动态处理。从高可用的角度，我们能容忍部分图片下载失败，但绝不能有图片上传失败，因为上传失败，意味着数据的丢失。上传与下载分开，能保证不会因下载的压力影响图片的上传，而且还有一点，下载入口和上传入口的负载均衡策略也有所不同。上传需要经过Quota Server记录用户和图片的关系等逻辑处理，下载的逻辑处理如果绕过了前端缓存处理，穿透后端业务逻辑处理，需要从OSS获取图片路径信息。近期阿里云会推出基于CDN就近上传的功能，自动选择离用户最近的CDN节点，使得数据的上传下载速度均得到最优化。相较传统IDC，访问速度提升数倍。</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p><strong>图片防盗链处理</strong></p>
<p>如果服务不允许防盗链，那么访问量会引起带宽、服务器压力等问题。比较通用的解决方案是在nginx或者squid反向代理软件上添加refer ACL判断，OSS也提供了基于refer的防盗链技术。当然OSS也提供了更为高级的URL签名防盗链，其其实现思路如下：</p>
<p>&nbsp;</p>
<p>首先，确认自己的bucket权限是private，即这个bucket的所有请求必须在签名认证通过后才被认为是合法的。然后根据操作类型、要访问的bucket、要访问的object以及超时时间，动态地生成一个经过签名的URL。通过这个签名URL，你授权的用户就可以在该签名URL过期时间前执行相应的操作。</p>
<p>&nbsp;</p>
<p>签名的Python代码如下：</p>
<p>h=hmac.new(&#8220;OtxrzxIsfpFjA7SwPzILwy8Bw21TLhquhboDYROV&#8221;, &#8220;GET\n\n\n1141889120\n/oss-example/oss-api.jpg&#8221;,sha);</p>
<p>urllib.quote_plus (base64.encodestring(h.digest()).strip());</p>
<p>&nbsp;</p>
<p>其中method可以是PUT、GET、HEAD、DELETE中的任意一种；最后一个参数“timeout”是超时的时间，单位是秒。一个通过上面Python方法，计算得到的签名URL为：</p>
<p>http://oss-example.oss-cn-hangzhou.aliyuncs.com/oss-api.jpg?OSSAccessKeyId=44CF9590006BF252F707&#038;Expires=1141889120&#038;Signature=vjbyPxybdZaNmGa%2ByT272YEAiv4%3D</p>
<p>&nbsp;</p>
<p>通过这种动态计算签名URL的方法，可以有效地保护放在OSS上的数据，防止被其他人盗链。</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p><strong>图片编辑处理API</strong></p>
<p>对于在线图片的编辑处理，GraphicsMagick（GraphicsMagick(<a href="http://www.graphicsmagick.org/">http://www.graphicsmagick.org/</a>)）对于从事互联网的技术人员应该不会陌生。GraphicsMagick是从 ImageMagick 5.5.2 分支出来的，但是现在他变得更稳定和优秀，GM更小更容易安装、GM更有效率、GM的手册非常丰富GraphicsMagick的命令与ImageMagick基本是一样的。</p>
<p>&nbsp;</p>
<p>GraphicsMagick 提供了包括裁、缩放、合成、打水印、图像转换、填充等非常丰富的接口API,其中的开发包SDK也非常丰富，包括了JAVA(im4java)、C、C++、Perl、PHP、Tcl、Ruby等的调用，支持超过88中图像格式，包括重要的DPX、GIF、JPEG、JPEG-2000、PNG、PDF、PNM和TIFF，GraphicsMagick可以再绝大多数的平台上使用，Linux、Mac、Windows都没有问题。但是独立开发这些图片处理服务，对服务器的IO要求相对要高一些，而且目前这些开源的图片处理编辑库，相对来说还不是很稳定，笔者在使用GraphicsMagick 的时候就遇到了tomcat 进程crash情况，需要手动重启tomcat服务。</p>
<p>&nbsp;</p>
<p>阿里云目前已经对外开放图片处理API,包括了大多数常用处理解决方案：缩略图、打水印、文字水印、样式、管道等。开发者可以非常方便的使用如上图片处理方案，希望越来越多的开发者能够基于OSS开放出更多优秀的产品。</p>