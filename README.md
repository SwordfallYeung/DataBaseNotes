# DataBaseNotes
记录一些关系型以及非关系型数据库的sql优化、查询、规范信息等等

# 关系型数据库建库建表规范
1.表达是与否概念的字段，必须使用is_xxx的方法命令，数据类型是unsigned tinyint(1表示是，0表示否)。<br/>
2.表名、字段名必须使用小写字母或数字，禁止出现数字开头，禁止两个下划线中间只出现数字。<br/>
3.表名不使用复数名词。<br/>
4.表必备三字段，id，gmt_create，gmt_modified<br/>
  说明：其中id必为主键，类型为bigint unsigned、单表时自增、步长为1。gmt_create,gmt_modified的类型均为datetime类型，前者现在时表示主动创建，后者过去分词表示被动更新。<br/>
5.表的命名最好是加上"业务名称_表的作用"

--------------------------------------------------------------------------------------------------------------------------------------

# MongoDB数据库
mongodb聚合规范写法：<br/>
参考资料：https://www.jianshu.com/p/e60d5cfbeb35

JAVA 处理 Spring data mongodb 时区问题 :<br/>
参考资料：https://my.oschina.net/xiaominmin/blog/1861590

★★★★★<br/>
在SPRING DATA MONGODB中使用聚合group by统计查询<br/>
参考资料：https://blog.csdn.net/u010084868/article/details/52622938

MongoDB 之 aggregate $group 巧妙运用 group多个字段：<br/>
https://blog.csdn.net/molashaonian/article/details/79402430<br/>
参考模板如下：
>db.getCollection('device').aggregate([<br/>
　　{$project: {deviceId: "$deviceId",lon: "$lon", lat: "$lat"}},<br/>
　　{$group: {_id: {<br/>
　　　　deviceId: "$deviceId",<br/>
　　　　lon: "$lon",<br/>
　　　　lat: "$lat"<br/>
　　}, <br/>
　　total:{ $sum: 1}}},<br/>
　　{$project: {_id: "$_id.deviceId",lon: "$_id.lon", lat: "$_id.lat", total: "$total"}}<br/>
])

★★★★★<br/>
mongodb的多表联查与与后续的数据处理（最多两张表关联，而且无法根据关联的那张表里的字段查询）<br/>
参考资料：https://blog.csdn.net/DDKii/article/details/81504805<br/>
https://blog.csdn.net/qq_39489635/article/details/77720789<br/>
参考模板如下：
> db.getCollection('imsiDevice').aggregate([ <br/>
　　{$match: {_id: ObjectId("5b8a0a43d15f8246565de010")}}, <br/>
　　{$lookup:{from:"place", localField:"placeId", foreignField:"_id", as: "places"}}, <br/>
　　{$unwind:"$places"}, <br/>
　　{$project: {deviceName:"$deviceName" ,placeName:"$places.placeName", provinceCode:"$places.provinceCode", <br/>
　　cityCode:"$places.cityCode", areaCode:"$places.areaCode", detailAddress:"$places.detailAddress"}}, <br/>
　　{$sort: {upTime: -1}}，<br/>
　　{$limit: 2}
　])

Spring-Data-Mongodb中的project()的用法<br/>
https://blog.csdn.net/hotdust/article/details/52605990

mongodb实战第二版pdf下载：<br/>
http://www.roadjava.com/s/spsb/gjzl/2018/05/mongodbszdebpdfxz.html

mongoDB的Criteria查询：多表联合查询（效率很低）<br/>
https://blog.csdn.net/EidenRitto/article/details/78521337

MongoDB的劣势：<br/>
1. 不支持事务；最新版本4.0以上已支持事务 <br/>
2. 不支持超过3个表的关联；<br/>
3. 不支持外表字段作为查询条件；<br/>

NoSql数据库使用半年后在设计上面的一些心得:<br/>
心得：①尽可能把一次展示所需的必要数据都存储到一起，②Mongodb不支持事务，所以务必在设计的时候考虑到这一点。核心业务数据尽可能通过结构设计做到数据插入的一致性。<br/>
https://www.cnblogs.com/AllenDang/p/3507821.html<br/>

mysql与mongo相比，事务与约束性更强。在处理较为价值较高的数据时，关系型数据库有它天然的优势，然而任何一种业务情况下，关系型数据库都不是银弹策略。任何抛开业务来谈论技术，都是在耍流氓。那么在什么情况下去选择mongo呢？<br/>
1. 数据价值不是重点<br/>
2. 属性查询需求较少<br/>
3. 数据之间约束较少<br/>
https://blog.csdn.net/cfl20121314/article/details/50734559<br/>

MongoDB进阶模式设计<br/>
http://www.mongoing.com/mongodb-advanced-pattern-design<br/>

《MongoDB实战》心得：<br/>
基于文档的数据模型可以表示丰富的、多层次的数据结构。它经常用来处理无须多表关联的工作。

根据MongoDB内嵌子文档增删改(一对多):<br/>
https://blog.csdn.net/walle167/article/details/51281199

mongodb数据库连接池（java版）<br/>
https://www.cnblogs.com/dmir/p/4780544.html

mongodb for java基本查询<br/>
https://www.cnblogs.com/zhoulf/p/4571647.html<br/>
mongodb多条件查询<br/>
https://www.cnblogs.com/sa-dan/p/6836055.html

mongodb更新文档<br/>
https://blog.csdn.net/u013066244/article/details/73849730

mongodb数组查询，针对key: [value1, value2] <br/>
https://blog.csdn.net/leshami/article/details/55049891

mongodb中查询返回指定字段<br/>
https://blog.csdn.net/u012086400/article/details/78652919

用java实现mongodb中查询返回指定字段<br/>
https://blog.csdn.net/albert0707/article/details/54098164

mongodb集群连接<br/>
https://blog.csdn.net/truong/article/details/74521636

mongodb大数据量分页查询的效率问题<br/>
https://blog.csdn.net/zhu_tianwei/article/details/44465415

Spark与mongodb整合完整版本<br/>
https://cloud.tencent.com/developer/article/1032526

Spark连接mongodb集群<br/>
https://piaosanlang.gitbooks.io/mongodb/content/03day/section99.html

Spark集成Mongodb的API<br/>
https://blog.csdn.net/txbsw/article/details/83377005

MongoDB下根据内嵌数组大小查询<br/>
https://blog.csdn.net/gao36951/article/details/40678875<br/>

Java MongoDB下根据数组大小进行查询的方法<br/>
https://blog.csdn.net/gao36951/article/details/40678875

mongodb find复杂条件查询 (or与and)<br/>
https://blog.csdn.net/tjbsl/article/details/80620303

mongodbexport 与 mongodbimport数据导入导出<br/>
https://www.cnblogs.com/qingtianyu2015/p/5968400.html

mongo中获取内嵌数组的长度<br/>
https://blog.csdn.net/luoduyu/article/details/78288258

MongoDB数据库某个字段求和<br/>
https://blog.csdn.net/andywa007/article/details/74950320

MongoDB数据库报错：<br/>
>com.mongodb.MongoQueryException: Query failed with error code 133 and error message 'Could not find host matching read preference { mode: "primary" } for set data' on server 192.168.31.230:39017<br/>
貌似是无法连接mongodb 的primary主服务器。<br/>

https://www.oschina.net/question/1428558_2159367

Spark从MongoDB库中抓取数据指定分区类型和分区大小<br/>
http://www.th7.cn/db/nosql/201706/242836.shtml<br/>
https://docs.mongodb.com/spark-connector/current/configuration/#cache-configuration

mongoDB 大数据量排序索引<br/>
http://orange2008.iteye.com/blog/1754168

mongoDB索引创建<br/>
http://blog.51cto.com/chenql/2071267

### MongoDB索引管理<br/>
创建索引： db.users.createIndex({name:1})<br/>
查看索引： getIndexes()方法可以用来查看集合的所有索引<br/>
http://blog.51cto.com/chenql/2071267

### mongoDB报“com.mongodb.MongoCursorNotFoundException，error code -5”
https://blog.csdn.net/zh0u_f/article/details/72897628<br/>
http://www.cnblogs.com/gaoze/p/7212436.html<br/>
https://blog.csdn.net/demon_LL/article/details/56843700?locationNum=12&fps=1<br/>

### mongodb 对 内嵌数据添加多个数据操作
https://blog.csdn.net/qq_20127333/article/details/51508863<br/>
https://blog.csdn.net/niclascage/article/details/47009989<br/>
https://blog.csdn.net/dream20nn/article/details/51306228?utm_source=blogxgwz1

### mongodb 根据条件删除
>db.getCollection('collisionTask').remove({_id:""})

### Monogodb副本集+切片搭建高可用
https://yq.aliyun.com/articles/617226<br/>
读写分离：https://blog.csdn.net/majinggogogo/article/details/51586409<br/>
https://blog.csdn.net/majinggogogo/article/details/51586409<br/>

### Mongodb 并发连接数过大会导致Mongodb节点挂掉
使用：ulimit -n 查看linux的并发连接数

### Mongodb 报 “连接时提示Connection reset by peers？”
https://yq.aliyun.com/articles/53771<br/>
https://yq.aliyun.com/articles/8461?spm=a2c4e.11153940.blogcont53771.6.48ea3d825H8ebr<br/>
https://www.cnblogs.com/archoncap/p/5883723.html

### 使用java Driver连接MongoDB切片+副本集群
目前有两种方式可供连接：<br/>
方式一：<br/>
> String mongodbUri = resource.getString("mongo.uri");<br/>
　String[] strings = mongodbUri.split(",");<br/>
　List<ServerAddress> list = new ArrayList<>();<br/>
　for (String host: strings){<br/>
　　String[] hosts = host.split(":");<br/>
　　list.add(new ServerAddress(hosts[0], Integer.parseInt(hosts[1])));<br/>
　}<br/>
　mongoClient = new MongoClient(list, myOptions);<br/>

方式二：<br/>
> String url = resource.getString("mongo.uri");
　MongoClientURI connectionString =  new MongoClientURI(url);
　mongoClient = new MongoClient(connectionString);

目前比较推荐的连接方式是方式二，可以参考Mongodb官网和阿里云社区介绍：<br/>
Mongodb官网介绍 replica set与sharded 连接方式：https://docs.mongodb.com/manual/reference/connection-string/<br/>
https://yq.aliyun.com/articles/8461?spm=a2c4e.11153940.blogcont53771.6.48ea3d825H8ebr

### Mongodb生产环境副本集的构成
生产环境通常是 2个副本 + 1个仲裁（即一主一从一仲裁）<br/>
http://blog.51cto.com/linuxblind/1709791

### Mongodb运维细节
https://cloud.tencent.com/developer/article/1027550

### Mongodb的oplogsize详解
https://www.cnblogs.com/Joans/p/7723554.html

### Mongodb的配置属性说明
http://www.cnblogs.com/zhoujinyi/p/3130231.html

### Mongodb备份与恢复
>./mongodump  --host 192.168.187.201:29050  --authenticationDatabase admin -d test -o /opt/app/mongodb/dump<br/>
./mongorestore  --host 192.168.187.201:29050  --authenticationDatabase admin -d test  /opt/app/mongodb/store<br/>
https://www.cnblogs.com/xiaotengyi/p/6393972.html<br/>

### Mongodb 启动报错:"/sys/kernel/mm/transparent_hugepage/enabled is 'always'"
https://blog.csdn.net/u013075468/article/details/51471033

### MongoDB numa系列问题二：WARNING: You are running on a NUMA machine.
https://www.cnblogs.com/xiaoit/p/4484343.html<br/>
http://bbs.51cto.com/thread-1139575-1.html<br/>
http://blog.51cto.com/xjsunjie/1412532

### MongoDB启动报警及解决方法：
https://blog.csdn.net/vkingnew/article/details/81703707

### MongoDB自动化部署参考
https://my.oschina.net/u/3036707/blog/786816<br/>
https://blog.csdn.net/huwei2003/article/details/44059797

### Mongodb副本集常用操作命令及原理
https://www.cnblogs.com/ivictor/p/6804408.html

### Mongodb分片+副本集群启动的顺序
集群环境：15个节点，3台机器，3个config，3个shard1，3个shard2，3个shard3，3个mongos，每台机器1个config、1个shard1、1个shard2、1个shard3、1个mongos<br/>
先是配置服务副本集启动，再是shard1分片副本集启动，接着是shard2分片副本集启动，后是shard3分片副本集启动，最后是mongos路由启动<br/>
<b>注意：<b/> 
  1. 不能先在机器node1上面一次性启动config、shard1、shard2、shard3，等node1都启动完后再在node2、node3上启动，这种做法是错误的，会导致node1上的4个服务始终在等待中。<br/>
  2. mongos要等所有机器的config、shard1、shard2、shard3全部启动起来，并且config、shard都已经初始化配置好，才能最终启动mongos，要不然启动报错<br/>
  3. 如果非要一次性在一台机器上启动config、shard1、shard2、shard3四个服务，需要三台机器同时都启动4个服务，最后再启动mongos<br/>

### Mongodb架构问题
再看看我们使用的mongodb java 驱动客户端 MongoClient(addresses)，这个可以传入多个mongos 的地址作为mongodb集群的入口，并且可以实现自动故障转移，但是负载均衡做的好不好呢？打开源代码查看：<br/>
它的机制是选择一个ping 最快的机器来作为所有请求的入口，如果这台机器挂掉会使用下一台机器。那这样。。。。肯定是不行的！万一出现双十一这样的情况所有请求集中发送到这一台机器，这台机器很有可能挂掉。一但挂掉了，按照它的机制会转移请求到下台机器，但是这个压力总量还是没有减少啊！下一台还是可能崩溃，所以这个架构还有漏洞！不过这个文章已经太长了，后续解决吧。<br/>
Spring集成mongodb可以实现负载均衡，在压力大时体现明显，平时压力小的时候体现不明显。

### Mongodb 连接数过高解决方法
https://my.oschina.net/u/1445816/blog/820000

### Mongodb报错："too many open files"
>ulimit -s 1024<br/>
pidof mongod<br/>
cat /proc/$(pidof mongod)/limits | grep stack | awk -F 'size' '{print int($NF)/1024}'<br/>
cat /proc/$(pidof mongod)/smaps | grep 10240 -A 10<br/>

https://www.cnblogs.com/unqiang/p/3748271.html

### Mongodb Linux内存VSS,RSS,PSS,USS解析
cat /proc/$(pidof mongod)/smaps | grep 10240 -A 10<br/>
https://www.cnblogs.com/skydty/p/7560566.html

### mongodb数据库分片问题，某一路由显示所有分片，但另一路由不显示分片
建议所有路由都连接一遍都分库分表，确保所有路由的分库分表状态都保持一致

### mongodb日志过大
https://docs.mongodb.com/manual/tutorial/rotate-log-files/<br/>
https://blog.csdn.net/jjwen/article/details/51352980<br/>

### mongodb性能优化
优化方向：<br/>
1. mongodb3.0引入了新的引擎WiredTiger，同时支持MMAPv1内存映射引擎和 inMemory
https://blog.csdn.net/happy_jijiawei/article/details/53737858

### MongoDB最佳实践-持续更新版
http://www.ywnds.com/?p=8656

### mongodb 性能测试
http://www.cnblogs.com/lovecindywang/archive/2011/03/02/1969324.htm

### mongodb 监控工具
开源的Ganglia、Zabbix、cacti、nagos，mongodb自带的mongostats/mongotop，MongoDB Monitoring Service<br/>
https://www.cnblogs.com/hy007x/p/7736403.html<br/>
https://www.cnblogs.com/ahaii/p/7146290.html?utm_source=itdadao&utm_medium=referral<br/>
https://blog.csdn.net/jianlong727/article/details/54586855<br/>
MongoDB 生态 – 可视化管理工具: http://www.mongoing.com/archives/3651<br/>
MMS监控：http://www.ywnds.com/?p=6430<br/>
MMS升级版-ops manager安装部署: https://blog.csdn.net/qq_15984019/article/details/82772452<br/>
OPS manager 硬件监控CPU，IO插件：https://docs.opsmanager.mongodb.com/v4.0/tutorial/configure-monitoring-munin-node/<br/>
CPU：http://www.ttlsa.com/mms/mms-useing-munin-node-monitor-hardware/<br/>
http://downloads.munin-monitoring.org/munin/stable/2.0.43/<br/>
https://fedoraproject.org/wiki/EPEL<br/>
https://linux.cn/article-6920-1.html<br/>

### mongodbtop mongodbstat mongosniff db.serverStatus db.stats详解
https://blog.csdn.net/wmj2004/article/details/79415892<br/>
https://blog.csdn.net/l1028386804/article/details/80009283<br/>
https://blog.csdn.net/zhu_tianwei/article/details/44514861<br/>
http://www.mongoing.com/docs/reference/ulimit.html<br/>

### mongodb explain慢查询分析
https://blog.csdn.net/wanght89/article/details/77647914

### linux硬件资源监控
https://blog.csdn.net/lhcshare/article/details/50844538<br/>
https://blog.faaoo.cn/yunwei/kftools/centos-red-hat-fedora-munin/<br/>
https://blog.csdn.net/qinglianluan/article/details/43054833<br/>
https://www.cnblogs.com/rond/p/3757804.html<br/>
添加动态图：https://www.cnblogs.com/rond/p/3777248.html<br/>
>报错：Can't load '/usr/lib64/perl5/vendor_perl/auto/RRDs/RRDs.so' for module RRDs: */lib64/libpango-1.0.so.0*: undefined symbol: g_log_structured_standard at /usr/lib64/perl5/DynaLoader.pm line 190.<br/>
解决方法：安装pango-devel pango依赖 yum install pango-devel<br/>
参考资料：https://www.cyberciti.biz/faq/howto-install-rrdtool-on-rhel-linux/

### mongodb 水平扩容
https://www.cnblogs.com/clsn/p/8214345.html<br/>
https://www.oschina.net/question/2668481_2157681<br/>
http://www.cnblogs.com/wufengtinghai/p/3832728.html<br/>

--------------------------------------------------------------------------------------------------------------------------------------

# HBase数据库
rowkey设计：<br/>
常用的rowkey设计为：salt (1 byte) + attr1_id (4 bytes) + timestamp (4 bytes) + attr2_id (4 bytes) <br/>
https://blog.csdn.net/chengyuqiang/article/details/79134549

rowkey避免热点：<br/>
https://my.oschina.net/lanzp/blog/477732

HBase二级索引：<br/>
https://blog.csdn.net/WYpersist/article/details/79830811<br/>
https://www.jianshu.com/p/0ccd187910e5
