canalClient+canal.deployer-1.1.0配合MySQL5.7时，数据库权限问题

2018年08月27日 09:33:15 loveyou1314ever 阅读数：196 标签： ３  更多

个人分类： Canal 基础篇

问题描述：

2018-08-24 17:02:24.611 \[destination = example , address = /127.0.0.1:3306 , EventParser\] ERROR c.a.o.c.p.inbound.mysql.rds.RdsBinlogEventParserProxy - dump address /127.0.0.1:3306 has an error, retrying. caused by

com.alibaba.otter.canal.parse.exception.CanalParseException: java.io.IOException: ErrorPacket \[errorNumber=1142, fieldCount=-1, message=SHOW VIEW command denied to user 'canal'@'localhost' for table 'actor\_info', sqlState=42000, sqlStateMarker=\#\]

with command: show create table \`sakila\`.\`actor\`;show create table \`sakila\`.\`actor\_info\`;show create table \`sakila\`.\`address\`;show create table \`sakila\`.\`category\`;show create table \`sakila\`.\`city\`;show create table \`sakila\`.\`country\`;show create table \`sakila\`.\`customer\`;show create table \`sakila\`.\`customer\_list\`;show create table \`sakila\`.\`film\`;show create table \`sakila\`.\`film\_actor\`;show create table \`sakila\`.\`film\_category\`;show create table \`sakila\`.\`film\_list\`;show create table \`sakila\`.\`film\_text\`;show create table \`sakila\`.\`inventory\`;show create table \`sakila\`.\`language\`;show create table \`sakila\`.\`nicer\_but\_slower\_film\_list\`;show create table \`sakila\`.\`payment\`;show create table \`sakila\`.\`rental\`;show create table \`sakila\`.\`sales\_by\_film\_category\`;show create table \`sakila\`.\`sales\_by\_store\`;show create table \`sakila\`.\`staff\`;show create table \`sakila\`.\`staff\_list\`;show create table \`sakila\`.\`store\`;

Caused by: java.io.IOException: ErrorPacket \[errorNumber=1142, fieldCount=-1, message=SHOW VIEW command denied to user 'canal'@'localhost' for table 'actor\_info', sqlState=42000, sqlStateMarker=\#\]

with command: show create table \`sakila\`.\`actor\`;show create table \`sakila\`.\`actor\_info\`;show create table \`sakila\`.\`address\`;show create table \`sakila\`.\`category\`;show create table \`sakila\`.\`city\`;show create table \`sakila\`.\`country\`;show create table \`sakila\`.\`customer\`;show create table \`sakila\`.\`customer\_list\`;show create table \`sakila\`.\`film\`;show create table \`sakila\`.\`film\_actor\`;show create table \`sakila\`.\`film\_category\`;show create table \`sakila\`.\`film\_list\`;show create table \`sakila\`.\`film\_text\`;show create table \`sakila\`.\`inventory\`;show create table \`sakila\`.\`language\`;show create table \`sakila\`.\`nicer\_but\_slower\_film\_list\`;show create table \`sakila\`.\`payment\`;show create table \`sakila\`.\`rental\`;show create table \`sakila\`.\`sales\_by\_film\_category\`;show create table \`sakila\`.\`sales\_by\_store\`;show create table \`sakila\`.\`staff\`;show create table \`sakila\`.\`staff\_list\`;show create table \`sakila\`.\`store\`;

```
at com.alibaba.otter.canal.parse.driver.mysql.MysqlQueryExecutor.queryMulti\(MysqlQueryExecutor.java:109\) ~\[canal.parse.driver-1.1.0.jar:na\]

at com.alibaba.otter.canal.parse.inbound.mysql.MysqlConnection.queryMulti\(MysqlConnection.java:107\) ~\[canal.parse-1.1.0.jar:na\]

at com.alibaba.otter.canal.parse.inbound.mysql.tsdb.DatabaseTableMeta.dumpTableMeta\(DatabaseTableMeta.java:175\) ~\[canal.parse-1.1.0.jar:na\]

at com.alibaba.otter.canal.parse.inbound.mysql.tsdb.DatabaseTableMeta.rollback\(DatabaseTableMeta.java:129\) ~\[canal.parse-1.1.0.jar:na\]

at com.alibaba.otter.canal.parse.inbound.mysql.AbstractMysqlEventParser.processTableMeta\(AbstractMysqlEventParser.java:91\) ~\[canal.parse-1.1.0.jar:na\]

at com.alibaba.otter.canal.parse.inbound.AbstractEventParser$3.run\(AbstractEventParser.java:188\) ~\[canal.parse-1.1.0.jar:na\]

at java.lang.Thread.run\(Unknown Source\) \[na:1.8.0\_181\]
```

-------------------------------分割线------------------------------------------------------

在执行canal.deployer-1.1.0源码中的example示例时，文档中的

通过对数据库MYSQL的canal进行权限设置，

CREATE USER canal IDENTIFIED BY 'canal';

GRANT SELECT, REPLICATION SLAVE, REPLICATION CLIENT ON \*.\* TO 'canal'@'%';

-- GRANT ALL PRIVILEGES ON \*.\* TO 'canal'@'%' ;

FLUSH PRIVILEGES;

然而，这对于MySQL５.７版本而言，需要的赋权限还是不够的，会出现上面的问题。所以需要更多权限授予

GRANT ALL ON \*.\* TO 'canal'@'%';

flush privileges;

这样就可以解决上面的权限不足问题。

mysql&gt; 2018-11-05T10:13:41.638582Z 49 \[Note\] Aborted connection 49 to db: 'unconnected' user: 'canal' host: 'localhost' \(Got an error reading communication packets\)

2018-11-05T10:13:41.686713Z 50 \[Note\] Start binlog\_dump to master\_thread\_id\(50\) slave\_server\(1778384897\), pos\(mysql-bin.000003, 2115\)

mysql&gt; GRANT ALL PRIVILEGES ON \*.\* TO 'canal'@'%' ;

Query OK, 0 rows affected \(0.01 sec\)

mysql&gt; FLUSH PRIVILEGES;

Query OK, 0 rows affected \(0.02 sec\)

mysql&gt; insert into didiorder\(order\_id,passenger\_id\) values\(12345678,34567\)\G

Query OK, 1 row affected \(0.02 sec\)



