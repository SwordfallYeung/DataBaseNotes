# DataBaseNotes
记录一些数据库的sql优化、查询、规范信息等等

# 数据库建库建表规范
1.表达是与否概念的字段，必须使用is_xxx的方法命令，数据类型是unsigned tinyint(1表示是，0表示否)。<br/>
2.表名、字段名必须使用小写字母或数字，禁止出现数字开头，禁止两个下划线中间只出现数字。<br/>
3.表名不使用复数名词。<br/>
4.表必备三字段，id，gmt_create，gmt_modified<br/>
  说明：其中id必为主键，类型为bigint unsigned、单表时自增、步长为1。gmt_create,gmt_modified的类型均为datetime类型，前者现在时表示主动创建，后者过去         分词表示被动更新。<br/>
5.表的命名最好是加上"业务名称_表的作用"

# MongoDB数据库
mongodb聚合规范写法：<br/>
参考资料：https://www.jianshu.com/p/e60d5cfbeb35

JAVA 处理 Spring data mongodb 时区问题 :<br/>
参考资料：https://my.oschina.net/xiaominmin/blog/1861590
