2018-11-06 11:19:10.120 \[main\] WARN  o.s.beans.GenericTypeAwarePropertyDescriptor - Invalid JavaBean property 'connectionCharset' being accessed! Ambiguous write methods found next to actually used \[public void com.alibaba.otter.canal.parse.inbound.mysql.AbstractMysqlEventParser.setConnectionCharset\(java.lang.String\)\]: \[public void com.alibaba.otter.canal.parse.inbound.mysql.AbstractMysqlEventParser.setConnectionCharset\(java.nio.charset.Charset\)\]

2018-11-06 11:19:10.556 \[main\] ERROR com.alibaba.druid.pool.DruidDataSource - testWhileIdle is true, validationQuery not set

show variables like 'character\_set\_%';

+--------------------------+------------------------------------------------------+

\| Variable\_name            \| Value                                                \|

+--------------------------+------------------------------------------------------+

\| character\_set\_client     \| utf8                                                 \|

\| character\_set\_connection \| utf8                                                 \|

\| character\_set\_database   \| utf8                                                 \|

\| character\_set\_filesystem \| binary                                               \|

\| character\_set\_results    \| utf8                                                 \|

\| character\_set\_server     \| utf8                                                 \|

\| character\_set\_system     \| utf8                                                 \|

\| character\_sets\_dir       \| /usr/local/Cellar/mysql/5.7.13/share/mysql/charsets/ \|

+--------------------------+------------------------------------------------------+

8 rows in set \(0.00 sec\)

alter database test character set utf8;

Query OK, 1 row affected \(0.01 sec\)

2018-11-06T03:21:20.058881Z 166 \[Note\] Aborted connection 166 to db: 'unconnected' user: 'canal' host: 'localhost' \(Got an error reading communication packets\)

2018-11-06T03:21:20.090171Z 167 \[Note\] Start binlog\_dump to master\_thread\_id\(167\) slave\_server\(1234\), pos\(mysql-bin.000003, 13543\)



2018-11-06 11:21:20.053 \[destination = example , address = /127.0.0.1:3306 , EventParser\] WARN  c.a.o.c.p.inbound.mysql.rds.RdsBinlogEventParserProxy - find start position : EntryPosition\[included=false,journalName=mysql-bin.000003,position=13543,serverId=1,gtid=&lt;null&gt;,timestamp=1541472770000\]

2018-11-06 11:21:20.102 \[MultiStageCoprocessor-other-example-0\] WARN  c.a.otter.canal.parse.inbound.mysql.tsdb.MemoryTableMeta - parse faield : alter database test character set utf8

com.alibaba.fastsql.sql.parser.ParserException: syntax error, error in :' set utf8', expect =, actual null, pos 38, line 1, column 35, token IDENTIFIER utf8

	at com.alibaba.fastsql.sql.parser.SQLParser.printError\(SQLParser.java:363\) ~\[fastsql-2.0.0\_preview\_644.jar:2.0.0\_preview\_644\]

	at com.alibaba.fastsql.sql.parser.SQLParser.accept\(SQLParser.java:371\) ~\[fastsql-2.0.0\_preview\_644.jar:2.0.0\_preview\_644\]

	at com.alibaba.fastsql.sql.dialect.mysql.parser.MySqlStatementParser.alterTableCharacter\(MySqlStatementParser.java:5415\) ~\[fastsql-2.0.0\_preview\_644.jar:2.0.0\_preview\_644\]

	at com.alibaba.fastsql.sql.dialect.mysql.parser.MySqlStatementParser.parseAlterDatabase\(MySqlStatementParser.java:5870\) ~\[fastsql-2.0.0\_preview\_644.jar:2.0.0\_preview\_644\]

	at com.alibaba.fastsql.sql.dialect.mysql.parser.MySqlStatementParser.parseAlter\(MySqlStatementParser.java:3899\) ~\[fastsql-2.0.0\_preview\_644.jar:2.0.0\_preview\_644\]

	at com.alibaba.fastsql.sql.parser.SQLStatementParser.parseStatementList\(SQLStatementParser.java:271\) ~\[fastsql-2.0.0\_preview\_644.jar:2.0.0\_preview\_644\]

	at com.alibaba.fastsql.sql.SQLUtils.parseStatements\(SQLUtils.java:525\) ~\[fastsql-2.0.0\_preview\_644.jar:2.0.0\_preview\_644\]





