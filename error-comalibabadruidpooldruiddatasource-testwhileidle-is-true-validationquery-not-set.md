ERROR com.alibaba.druid.pool.DruidDataSource - testWhileIdle is true, validationQuery not set

启动canal

到bin目录下启动canal server

./startup.sh

异常 ERROR testWhileIdle is true, validationQuery not set

查看运行日志 logs/example/example.log,发现如下错误:

ERROR com.alibaba.druid.pool.DruidDataSource - testWhileIdle is true, validationQuery not set

查看canal github仓库issues发现[https://github.com/alibaba/canal/issues/494](https://github.com/alibaba/canal/issues/494)

打开conf/canal.properties,注释掉:

\#canal.instance.tsdb.spring.xml=classpath:spring/tsdb/h2-tsdb.xml

2018-11-06 11:30:02.222 \[main\] WARN  o.s.beans.GenericTypeAwarePropertyDescriptor - Invalid JavaBean property 'connectionCharset' being accessed! Ambiguous write methods found next to actually used \[public void com.alibaba.otter.canal.parse.inbound.mysql.AbstractMysqlEventParser.setConnectionCharset\(java.nio.charset.Charset\)\]: \[public void com.alibaba.otter.canal.parse.inbound.mysql.AbstractMysqlEventParser.setConnectionCharset\(java.lang.String\)\]



