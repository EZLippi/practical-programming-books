<p>导读：Kristóf Kovács 是一位软件架构师和咨询顾问，他最近发布了一片对比各种类型NoSQL数据库的文章。</p>
<p>虽然SQL数据库是非常有用的工具，但经历了15年的一支独秀之后垄断即将被打破。这只是时间问题：被迫使用关系数据库，但最终发现不能适应需求的情况不胜枚举。</p>
<p>但是NoSQL数据库之间的不同，远超过两 SQL数据库之间的差别。这意味着软件架构师更应该在项目开始时就选择好一个适合的 NoSQL数据库。针对这种情况，这里对 <a href="http://cassandra.apache.org/">Cassandra</a>、<a href="http://www.mongodb.org/">Mongodb</a>、<a href="http://couchdb.apache.org/">CouchDB</a>、<a href="http://redis.io/">Redis</a>、 <a href="http://www.basho.com/Riak.html">Riak</a>、<a href="http://www.couchbase.org/membase">Membase</a>、<a href="http://neo4j.org/">Neo4j</a> 和 <a href="http://hbase.apache.org/">HBase</a> 进行了比较：</p>
<p>（编注1：NoSQL：是一项全新的数据库革命性运动，NoSQL的拥护者们提倡运用非关系型的数据存储。现今的计算机体系结构在数据存储方面要求具 备庞大的水平扩 展性，而NoSQL致力于改变这一现状。目前Google的 BigTable 和Amazon 的Dynamo使用的就是NoSQL型数据库。 参见<a href="http://zh.wikipedia.org/zh/NoSQL">NoSQL词条</a>。）</p>
<p>&nbsp;</p>
<p><strong>1. CouchDB</strong></p>
<ul>
<li>所用语言： Erlang</li>
<li>特点：DB一致性，易于使用</li>
<li>使用许可： Apache</li>
<li>协议： HTTP/REST</li>
<li>双向数据复制，</li>
<li>持续进行或临时处理，</li>
<li>处理时带冲突检查，</li>
<li>因此，采用的是master-master复制（见编注2）</li>
<li>MVCC &#8211; 写操作不阻塞读操作</li>
<li>可保存文件之前的版本</li>
<li>Crash-only（可靠的）设计</li>
<li>需要不时地进行数据压缩</li>
<li>视图：嵌入式 映射/减少</li>
<li>格式化视图：列表显示</li>
<li>支持进行服务器端文档验证</li>
<li>支持认证</li>
<li>根据变化实时更新</li>
<li>支持附件处理</li>
<li>因此， CouchApps（独立的 js应用程序）</li>
<li>需要 jQuery程序库</li>
</ul>
<p>&nbsp;</p>
<p><strong>最佳应用场景：</strong>适用于数据变化较少，执行预定义查询，进行数据统计的应用程序。适用于需要提供数据版本支持的应用程序。</p>
<p><strong>例如：</strong> CRM、CMS系统。 master-master复制对于多站点部署是非常有用的。</p>
<p>（编注2：master-master复制：是一种数据库同步方法，允许数据在一组计算机之间共享数据，并且可以通过小组中任意成员在组内进行数据更新。）</p>
<p>&nbsp;</p>
<p><strong>2. Redis</strong></p>
<ul>
<li>所用语言：C/C++</li>
<li>特点：运行异常快</li>
<li>使用许可： BSD</li>
<li>协议：类 Telnet</li>
<li>有硬盘存储支持的内存数据库，</li>
<li>但自2.0版本以后可以将数据交换到硬盘（注意， 2.4以后版本不支持该特性！）</li>
<li>Master-slave复制（见编注3）</li>
<li>虽然采用简单数据或以键值索引的哈希表，但也支持复杂操作，例如 ZREVRANGEBYSCORE。</li>
<li>INCR &amp; co （适合计算极限值或统计数据）</li>
<li>支持 sets（同时也支持 union/diff/inter）</li>
<li>支持列表（同时也支持队列；阻塞式 pop操作）</li>
<li>支持哈希表（带有多个域的对象）</li>
<li>支持排序 sets（高得分表，适用于范围查询）</li>
<li>Redis支持事务</li>
<li>支持将数据设置成过期数据（类似快速缓冲区设计）</li>
<li>Pub/Sub允许用户实现消息机制</li>
</ul>
<p>&nbsp;</p>
<p><strong>最佳应用场景：</strong>适用于数据变化快且数据库大小可遇见（适合内存容量）的应用程序。</p>
<p><strong>例如：</strong>股票价格、数据分析、实时数据搜集、实时通讯。</p>
<p>（编注3：Master-slave复制：如果同一时刻只有一台服务器处理所有的复制请求，这被称为 Master-slave复制，通常应用在需要提供高可用性的服务器集群。）</p>
<p>&nbsp;</p>
<p><strong>3. MongoDB</strong></p>
<ul>
<li>所用语言：C++</li>
<li>特点：保留了SQL一些友好的特性（查询，索引）。</li>
<li>使用许可： AGPL（发起者： Apache）</li>
<li>协议： Custom, binary（ BSON）</li>
<li>Master/slave复制（支持自动错误恢复，使用 sets 复制）</li>
<li>内建分片机制</li>
<li>支持 javascript表达式查询</li>
<li>可在服务器端执行任意的 javascript函数</li>
<li>update-in-place支持比CouchDB更好</li>
<li>在数据存储时采用内存到文件映射</li>
<li>对性能的关注超过对功能的要求</li>
<li>建议最好打开日志功能（参数 &#8211;journal）</li>
<li>在32位操作系统上，数据库大小限制在约2.5Gb</li>
<li>空数据库大约占 192Mb</li>
<li>采用 GridFS存储大数据或元数据（不是真正的文件系统）</li>
</ul>
<p>&nbsp;</p>
<p><strong>最佳应用场景：</strong>适用于需要动态查询支持；需要使用索引而不是 map/reduce功能；需要对大数据库有性能要求；需要使用 CouchDB但因为数据改变太频繁而占满内存的应用程序。</p>
<p><strong>例如：</strong>你本打算采用 MySQL或 PostgreSQL，但因为它们本身自带的预定义栏让你望而却步。</p>
<p>&nbsp;</p>
<p><strong>4. Riak</strong></p>
<ul>
<li>所用语言：Erlang和C，以及一些Javascript</li>
<li>特点：具备容错能力</li>
<li>使用许可： Apache</li>
<li>协议： HTTP/REST或者 custom binary</li>
<li>可调节的分发及复制(N, R, W)</li>
<li>用 JavaScript or Erlang在操作前或操作后进行验证和安全支持。</li>
<li>使用JavaScript或Erlang进行 Map/reduce</li>
<li>连接及连接遍历：可作为图形数据库使用</li>
<li>索引：输入元数据进行搜索（1.0版本即将支持）</li>
<li>大数据对象支持（ Luwak）</li>
<li>提供“开源”和“企业”两个版本</li>
<li>全文本搜索，索引，通过 Riak搜索服务器查询（ beta版）</li>
<li>支持Masterless多站点复制及商业许可的 SNMP监控</li>
</ul>
<p>&nbsp;</p>
<p><strong>最佳应用场景：</strong>适用于想使用类似 Cassandra（类似Dynamo）数据库但无法处理 bloat及复杂性的情况。适用于你打算做多站点复制，但又需要对单个站点的扩展性，可用性及出错处理有要求的情况。</p>
<p>例如：销售数据搜集，工厂控制系统；对宕机时间有严格要求；可以作为易于更新的 web服务器使用。</p>
<p><strong>5. Membase</strong></p>
<ul>
<li>所用语言： Erlang和C</li>
<li>特点：兼容 Memcache，但同时兼具持久化和支持集群</li>
<li>使用许可： Apache 2.0</li>
<li>协议：分布式缓存及扩展</li>
<li>非常快速（200k+/秒），通过键值索引数据</li>
<li>可持久化存储到硬盘</li>
<li>所有节点都是唯一的（ master-master复制）</li>
<li>在内存中同样支持类似分布式缓存的缓存单元</li>
<li>写数据时通过去除重复数据来减少 IO</li>
<li>提供非常好的集群管理 web界面</li>
<li>更新软件时软无需停止数据库服务</li>
<li>支持连接池和多路复用的连接代理</li>
</ul>
<p>&nbsp;</p>
<p><strong>最佳应用场景：</strong>适用于需要低延迟数据访问，高并发支持以及高可用性的应用程序</p>
<p>例如：低延迟数据访问比如以广告为目标的应用，高并发的 web 应用比如网络游戏（例如 Zynga）</p>
<p>&nbsp;</p>
<p><strong>6. Neo4j</strong></p>
<ul>
<li>所用语言： Java</li>
<li>特点：基于关系的图形数据库</li>
<li>使用许可： GPL，其中一些特性使用 AGPL/商业许可</li>
<li>协议： HTTP/REST（或嵌入在 Java中）</li>
<li>可独立使用或嵌入到 Java应用程序</li>
<li>图形的节点和边都可以带有元数据</li>
<li>很好的自带web管理功能</li>
<li>使用多种算法支持路径搜索</li>
<li>使用键值和关系进行索引</li>
<li>为读操作进行优化</li>
<li>支持事务（用 Java api）</li>
<li>使用 Gremlin图形遍历语言</li>
<li>支持 Groovy脚本</li>
<li>支持在线备份，高级监控及高可靠性支持使用 AGPL/商业许可</li>
</ul>
<p>&nbsp;</p>
<p><strong>最佳应用场景：</strong>适用于图形一类数据。这是 Neo4j与其他nosql数据库的最显著区别</p>
<p>例如：社会关系，公共交通网络，地图及网络拓谱</p>
<p>&nbsp;</p>
<p><strong>7. Cassandra</strong></p>
<ul>
<li>所用语言： Java</li>
<li>特点：对大型表格和 Dynamo支持得最好</li>
<li>使用许可： Apache</li>
<li>协议： Custom, binary (节约型)</li>
<li>可调节的分发及复制(N, R, W)</li>
<li>支持以某个范围的键值通过列查询</li>
<li>类似大表格的功能：列，某个特性的列集合</li>
<li>写操作比读操作更快</li>
<li>基于 Apache分布式平台尽可能地 Map/reduce</li>
<li>我承认对 Cassandra有偏见，一部分是因为它本身的臃肿和复杂性，也因为 Java的问题（配置，出现异常，等等）</li>
</ul>
<p>&nbsp;</p>
<p><strong>最佳应用场景：</strong>当使用写操作多过读操作（记录日志）如果每个系统组建都必须用 Java编写（没有人因为选用 Apache的软件被解雇）</p>
<p>例如：银行业，金融业（虽然对于金融交易不是必须的，但这些产业对数据库的要求会比它们更大）写比读更快，所以一个自然的特性就是实时数据分析</p>
<p>&nbsp;</p>
<p><strong>8. HBase</strong></p>
<p>（配合 ghshephard使用）</p>
<ul>
<li>所用语言： Java</li>
<li>特点：支持数十亿行X上百万列</li>
<li>使用许可： Apache</li>
<li>协议：HTTP/REST （支持 <a href="http://www.jobbole.com/entry.php/73">Thrift</a>，见编注4）</li>
<li>在 BigTable之后建模</li>
<li>采用分布式架构 Map/reduce</li>
<li>对实时查询进行优化</li>
<li>高性能 Thrift网关</li>
<li>通过在server端扫描及过滤实现对查询操作预判</li>
<li>支持 XML, Protobuf, 和binary的HTTP</li>
<li>Cascading, hive, and pig source and sink modules</li>
<li>基于 Jruby（ JIRB）的shell</li>
<li>对配置改变和较小的升级都会重新回滚</li>
<li>不会出现单点故障</li>
<li>堪比MySQL的随机访问性能</li>
</ul>
<p>&nbsp;</p>
<p><strong>最佳应用场景：</strong>适用于偏好BigTable:)并且需要对大数据进行随机、实时访问的场合。</p>
<p>例如： Facebook消息数据库（更多通用的用例即将出现）</p>
<p>编注4：Thrift 是一种接口定义语言，为多种其他语言提供定义和创建服务，<a href="http://blog.jobbole.com/73/">由Facebook开发并开源</a>。</p>
<p>当然，所有的系统都不只具有上面列出的这些特性。这里我仅仅根据自己的观点列出一些我认为的重要特性。与此同时，技术进步是飞速的，所以上述的内容肯定需要不断更新。我会尽我所能地更新这个列表。</p>
<p>&nbsp;</p>