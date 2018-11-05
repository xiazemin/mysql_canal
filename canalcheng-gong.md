~/canal$sh  bin/startup.sh

 2018-11-05T09:41:13.258987Z 25 \[Note\] Access denied for user 'canal'@'localhost' \(using password: YES\)

2018-11-05T09:41:30.950832Z 26 \[Note\] Access denied for user 'canal'@'localhost' \(using password: YES\)

2018-11-05T09:41:43.909682Z 27 \[Note\] Access denied for user 'canal'@'localhost' \(using password: YES\)

2018-11-05T09:41:51.031516Z 29 \[Note\] Aborted connection 29 to db: 'unconnected' user: 'canal' host: 'localhost' \(Got an error reading communication packets\)

2018-11-05T09:41:51.040019Z 30 \[Note\] Start binlog\_dump to master\_thread\_id\(30\) slave\_server\(1778384897\), pos\(mysql-bin.000003, 4\)

2018-11-05T09:42:08.246899Z 28 \[Note\] Aborted connection 28 to db: 'unconnected' user: 'canal' host: 'localhost' \(Got an error reading communication packets\)

2018-11-05T09:42:09.066008Z 31 \[Note\] Aborted connection 31 to db: 'unconnected' user: 'canal' host: 'localhost' \(Got an error reading communication packets\)

2018-11-05T09:42:09.092720Z 32 \[Note\] Start binlog\_dump to master\_thread\_id\(32\) slave\_server\(1778384897\), pos\(mysql-bin.000003, 123\)

2018-11-05T09:42:24.100252Z 33 \[Note\] Aborted connection 33 to db: 'unconnected' user: 'canal' host: 'localhost' \(Got an error reading communication packets\)

2018-11-05T09:42:33.894501Z 35 \[Note\] Aborted connection 35 to db: 'unconnected' user: 'canal' host: 'localhost' \(Got an error reading communication packets\)

2018-11-05T09:42:33.903580Z 36 \[Note\] Start binlog\_dump to master\_thread\_id\(36\) slave\_server\(1778384897\), pos\(mysql-bin.000003, 4\)

$tail -f logs/example/example.log

2018-11-05 17:42:24.103 \[Thread-6\] INFO  c.a.otter.canal.instance.core.AbstractCanalInstance - stop successful....

2018-11-05 17:42:32.720 \[main\] INFO  c.a.o.c.i.spring.support.PropertyPlaceholderConfigurer - Loading properties file from class path resource \[canal.properties\]

2018-11-05 17:42:32.723 \[main\] INFO  c.a.o.c.i.spring.support.PropertyPlaceholderConfigurer - Loading properties file from class path resource \[example/instance.properties\]

2018-11-05 17:42:33.004 \[main\] WARN  o.s.beans.GenericTypeAwarePropertyDescriptor - Invalid JavaBean property 'connectionCharset' being accessed! Ambiguous write methods found next to actually used \[public void com.alibaba.otter.canal.parse.inbound.mysql.AbstractMysqlEventParser.setConnectionCharset\(java.nio.charset.Charset\)\]: \[public void com.alibaba.otter.canal.parse.inbound.mysql.AbstractMysqlEventParser.setConnectionCharset\(java.lang.String\)\]

2018-11-05 17:42:33.076 \[main\] INFO  c.a.o.c.i.spring.support.PropertyPlaceholderConfigurer - Loading properties file from class path resource \[canal.properties\]

2018-11-05 17:42:33.077 \[main\] INFO  c.a.o.c.i.spring.support.PropertyPlaceholderConfigurer - Loading properties file from class path resource \[example/instance.properties\]

2018-11-05 17:42:33.458 \[main\] ERROR com.alibaba.druid.pool.DruidDataSource - testWhileIdle is true, validationQuery not set

2018-11-05 17:42:33.829 \[main\] INFO  c.a.otter.canal.instance.spring.CanalInstanceWithSpring - start CannalInstance for 1-example

2018-11-05 17:42:33.843 \[main\] INFO  c.a.otter.canal.instance.core.AbstractCanalInstance - start successful....

2018-11-05 17:42:33.892 \[destination = example , address = /127.0.0.1:3306 , EventParser\] WARN  c.a.o.c.p.inbound.mysql.rds.RdsBinlogEventParserProxy - prepare to find start position just show master status



