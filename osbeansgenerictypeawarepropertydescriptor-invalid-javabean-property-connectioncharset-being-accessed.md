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

