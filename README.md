# DataBaseNotes
记录一些关系型以及非关系型数据库的sql优化、查询、规范信息等等

# 关系型数据库建库建表规范
1.表达是与否概念的字段，必须使用is_xxx的方法命令，数据类型是unsigned tinyint(1表示是，0表示否)。<br/>
2.表名、字段名必须使用小写字母或数字，禁止出现数字开头，禁止两个下划线中间只出现数字。<br/>
3.表名不使用复数名词。<br/>
4.表必备三字段，id，gmt_create，gmt_modified<br/>
  说明：其中id必为主键，类型为bigint unsigned、单表时自增、步长为1。gmt_create,gmt_modified的类型均为datetime类型，前者现在时表示主动创建，后者过去分词表示被动更新。<br/>
5.表的命名最好是加上"业务名称_表的作用"

# MongoDB数据库
mongodb聚合规范写法：<br/>
参考资料：https://www.jianshu.com/p/e60d5cfbeb35

JAVA 处理 Spring data mongodb 时区问题 :<br/>
参考资料：https://my.oschina.net/xiaominmin/blog/1861590

在SPRING DATA MONGODB中使用聚合统计查询<br/>
参考资料：https://blog.csdn.net/u010084868/article/details/52622938

★★★★★<br/>
mongodb的多表联查与与后续的数据处理（最多两张表关联，而且无法根据关联的那张表里的字段查询）<br/>
参考资料：https://blog.csdn.net/DDKii/article/details/81504805<br/>
https://blog.csdn.net/qq_39489635/article/details/77720789<br/>
参考模板如下：
> db.getCollection('device').aggregate([ <br/>
  　 　 {$lookup:{from:"place", localField:"placeId", foreignField:"_id", as: "places"}}, <br/>
 　　　{$unwind:"$places"}, <br/>
  　   {$project: {placeName:"$places.placeName", provinceCode:"$places.provinceCode", cityCode:"$places.cityCode",                            areaCode:"$places.areaCode", detailAddress:"$places.detailAddress"}} <br/>
  ])

mongod实战第二版pdf下载：<br/>
http://www.roadjava.com/s/spsb/gjzl/2018/05/mongodbszdebpdfxz.html

mongoDB的Criteria查询：多表联合查询（效率很低）<br/>
https://blog.csdn.net/EidenRitto/article/details/78521337

MongoDB的劣势：<br/>
1.不支持事务；<br/>
2.不支持超过3个表的关联；<br/>
3.不支持外表字段作为查询条件；<br/>

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

# HBase数据库
rowkey设计：<br/>
常用的rowkey设计为：salt (1 byte) + attr1_id (4 bytes) + timestamp (4 bytes) + attr2_id (4 bytes) <br/>
https://blog.csdn.net/chengyuqiang/article/details/79134549

rowkey避免热点：<br/>
https://my.oschina.net/lanzp/blog/477732

HBase二级索引：<br/>
https://blog.csdn.net/WYpersist/article/details/79830811<br/>
https://www.jianshu.com/p/0ccd187910e5
