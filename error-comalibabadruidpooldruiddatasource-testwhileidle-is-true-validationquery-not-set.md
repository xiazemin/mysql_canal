 ERROR com.alibaba.druid.pool.DruidDataSource - testWhileIdle is true, validationQuery not set

启动canal

到bin目录下启动canal server



./startup.sh

异常 ERROR testWhileIdle is true, validationQuery not set

查看运行日志 logs/example/example.log,发现如下错误:



ERROR com.alibaba.druid.pool.DruidDataSource - testWhileIdle is true, validationQuery not set

查看canal github仓库issues发现https://github.com/alibaba/canal/issues/494 

打开conf/canal.properties,注释掉:



\#canal.instance.tsdb.spring.xml=classpath:spring/tsdb/h2-tsdb.xml

client端例子

