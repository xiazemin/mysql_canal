修改配置

vi conf/example/instance.properties

\#数据库地址

canal.instance.master.address=192.168.56.104:3306

\#数据库用户密码

canal.instance.dbUsername = canal

canal.instance.dbPassword = canal

\#数据库字符集

canal.instance.connectionCharset=UTF-8

\#默认数据库

canal.instance.defaultDatabaseName =zgai\_db

vim /usr/local/canal/conf/canal.properties

\#canalid

canal.id= 1

\#canal地址

canal.ip=192.168.56.102

\#zookeeper地址

canal.zkServers=192.168.56.102:2181

vim /usr/local/canal/conf/kafka.yml

\#kafka地址

servers: 192.168.56.101:9092

\# canal的批次大小，单位 k

canalBatchSize: 50

\#topic

topic: mytopic

canal.properties

canal配置主要分为两部分定义：

instance列表定义 \(列出当前server上有多少个instance，每个instance的加载方式是spring/manager等\)

\|参数名字    \|参数说明    \|默认值\|

\| ------------- \|:-------------😐 -----😐

\|canal.destinations\|    当前server上部署的instance列表\|    无

\|canal.conf.dir\|    conf/目录所在的路径    \|…/conf

\|canal.auto.scan\|    开启instance自动扫描如果配置为true，canal.conf.dir目录下的instance配置变化会自动触发：a. instance目录新增： 触发instance配置载入，lazy为true时则自动启动b. instance目录删除：卸载对应instance配置，如已启动则进行关闭c. instance.properties文件变化：reload instance配置，如已启动自动进行重启操作 \|    true

\|canal.auto.scan.interval    \|instance自动扫描的间隔时间，单位秒\|    5

\|canal.instance.global.mode\|    全局配置加载方式    \|spring

\|canal.instance.global.lazy\|    全局lazy模式    \|false

\|canal.instance.global.manager.address\|    全局的manager配置方式的链接信息\|    无

\|canal.instance.global.spring.xml\|    全局的spring配置方式的组件文件    \|classpath:spring/memory-instance.xml \(spring目录相对于canal.conf.dir\)

\|canal.instance.example.mode canal.instance.example.lazy canal.instance.example.spring.xml…    \| instance级别的配置定义，如有配置，会自动覆盖全局配置定义模式命名规则：canal.instance.{name}.xxx    \|无

\|canal.instance.tsdb.spring.xml\|    v1.0.25版本新增,全局的tsdb配置方式的组件文件    \|classpath:spring/tsdb/h2-tsdb.xml \(spring目录相对于canal.conf.dir\)

common参数定义，比如可以将instance.properties的公用参数，抽取放置到这里，这样每个instance启动的时候就可以共享. 【instance.properties配置定义优先级高于canal.properties】

参数名字	参数说明	默认值

canal.id	每个canal server实例的唯一标识，暂无实际意义	1

canal.ip	canal	server绑定的本地IP信息，如果不配置，默认选择一个本机IP进行启动服务	无

canal.port	canal server提供socket服务的端口	11111

canal.zkServers	canal server链接zookeeper集群的链接信息 例子：10.20.144.22:2181,10.20.144.51:2181	无

canal.zookeeper.flush.period	canal持久化数据到zookeeper上的更新频率，单位毫秒	1000

canal.instance.memory.batch.mode	canal内存store中数据缓存模式 1. ITEMSIZE : 根据buffer.size进行限制，只限制记录的数量 2. MEMSIZE : 根据buffer.size \* buffer.memunit的大小，限制缓存记录的大小	MEMSIZE

canal.instance.memory.buffer.size	canal内存store中可缓存buffer记录数，需要为2的指数	16384

canal.instance.memory.buffer.memunit	内存记录的单位大小，默认1KB，和buffer.size组合决定最终的内存使用大小	1024

canal.instance.transactionn.size	最大事务完整解析的长度支持超过该长度后，一个事务可能会被拆分成多次提交到canal store中，无法保证事务的完整可见性	1024

canal.instance.fallbackIntervalInSeconds	canal发生mysql切换时，在新的mysql库上查找binlog时需要往前查找的时间，单位秒 说明：mysql主备库可能存在解析延迟或者时钟不统一，需要回退一段时间，保证数据不丢	60

canal.instance.detecting.enable	是否开启心跳检查	false

canal.instance.detecting.sql	心跳检查sql	insert into retl.xdual values\(1,now\(\)\) on duplicate key update x=now\(\) canal.instance.detecting.interval.time	心跳检查频率，单位秒	3

canal.instance.detecting.retry.threshold	心跳检查失败重试次数	3

canal.instance.detecting.heartbeatHaEnable	心跳检查失败后，是否开启自动mysql自动切换说明：比如心跳检查失败超过阀值后，如果该配置为true，canal就会自动链到mysql备库获取binlog数据	false

canal.instance.network.receiveBufferSize	网络链接参数，SocketOptions.SO\_RCVBUF	16384

canal.instance.network.sendBufferSize	网络链接参数，SocketOptions.SO\_SNDBUF	16384

canal.instance.network.soTimeout	网络链接参数，SocketOptions.SO\_TIMEOUT	30

canal.instance.filter.druid.ddl	是否使用druid处理所有的ddl解析来获取库和表名	true

canal.instance.filter.query.dcl	是否忽略dcl语句	false

canal.instance.filter.query.dml	是否忽略dml语句\(mysql5.6之后，在row模式下每条DML语句也会记录SQL到binlog中,可参考MySQL文档\)	false

canal.instance.filter.query.ddl	是否忽略ddl语句	false

canal.instance.filter.table.error	是否忽略binlog表结构获取失败的异常\(主要解决回溯binlog时,对应表已被删除或者表结构和binlog不一致的情况\)	false

canal.instance.filter.rows	是否dml的数据变更事件\(主要针对用户只订阅ddl/dcl的操作\)	false

canal.instance.filter.transaction.entry	是否忽略事务头和尾,比如针对写入kakfa的消息时，不需要写入TransactionBegin/Transactionend事件	false

canal.instance.binlog.format	支持的binlog format格式列表\(otter会有支持format格式限制\)	ROW,STATEMENT,MIXED

canal.instance.binlog.image	支持的binlog image格式列表\(otter会有支持format格式限制\)	FULL,MINIMAL,NOBLOB

canal.instance.get.ddl.isolation	ddl语句是否单独一个batch返回\(比如下游dml/ddl如果做batch内无序并发处理,会导致结构不一致\)	false

canal.instance.parser.parallel	是否开启binlog并行解析模式\(串行解析资源占用少,但性能有瓶颈, 并行解析可以提升近2.5倍+\)	true

canal.instance.parser.parallelBufferSize	binlog并行解析的异步ringbuffer队列\(必须为2的指数\)	256

canal.instance.tsdb.enable	是否开启tablemeta的tsdb能力	true

canal.instance.tsdb.dir	主要针对h2-tsdb.xml时对应h2文件的存放目录,默认conf/xx/h2.mv.db	

canal.instance.tsdb.url	jdbc url的配置\(h2的地址为默认值，如果是mysql需要自行定义\)	jdbc:h2: ${canal.instance.tsdb.dir} /h2;CACHE\_SIZE=1000;MODE=MYSQL;

canal.instance.tsdb.dbUsername	jdbc url的配置\(h2的地址为默认值，如果是mysql需要自行定义\)	canal

canal.instance.tsdb.dbPassword	jdbc url的配置\(h2的地址为默认值，如果是mysql需要自行定义\)	canal

canal.instance.rds.accesskey	aliyun账号的ak信息 \(如果不需要在本地binlog超过18小时被清理后自动下载oss上的binlog，可以忽略该值\)	无

canal.instance.rds.secretkey	aliyun账号的sk信息\(如果不需要在本地binlog超过18小时被清理后自动下载oss上的binlog，可以忽略该值\)	无

instance.properties

参数名字	参数说明	默认值

canal.instance.mysql.slaveId	mysql集群配置中的serverId概念，需要保证和当前mysql集群中id唯一 \(v1.1.x版本之后canal会自动生成，不需要手工指定\)	无

canal.instance.master.address	mysql主库链接地址	127.0.0.1:3306

canal.instance.master.journal.name	mysql主库链接时起始的binlog文件	无

canal.instance.master.position	mysql主库链接时起始的binlog偏移量	无

canal.instance.master.timestamp	mysql主库链接时起始的binlog的时间戳	无

canal.instance.gtidon	是否启用mysql gtid的订阅模式	false

canal.instance.master.gtid	mysql主库链接时对应的gtid位点	无

canal.instance.dbUsername	mysql数据库帐号	canal

canal.instance.dbPassword	mysql数据库密码	canal

canal.instance.defaultDatabaseName	mysql链接时默认schema	无

canal.instance.connectionCharset	mysql 数据解析编码	UTF-8

canal.instance.filter.regex	mysql 数据解析关注的表，Perl正则表达式.多个正则之间以逗号\(,\)分隔，转义符需要双斜杠\(\\) 常见例子： 1. 所有表：.\* or .\… 2. canal schema下所有表： canal\…\* 3. canal下的以canal打头的表：canal\.canal.\* 4. canal schema下的一张表：canal.test1 5. 多个规则组合使用：canal\…\*,mysql.test1,mysql.test2 \(逗号分隔\)	.\…

canal.instance.filter.black.regex	mysql 数据解析表的黑名单，表达式规则见白名单的规则	无

canal.instance.rds.instanceId	aliyun rds对应的实例id信息\(如果不需要在本地binlog超过18小时被清理后自动下载oss上的binlog，可以忽略该值\)	无

说明：

mysql链接时的起始位置

canal.instance.master.journal.name + canal.instance.master.position : 精确指定一个binlog位点，进行启动

canal.instance.master.timestamp : 指定一个时间戳，canal会自动遍历mysql binlog，找到对应时间戳的binlog位点后，进行启动

不指定任何信息：默认从当前数据库的位点，进行启动。\(show master status\)

\#\#\#\#\#mysql解析关注表定义

标准的Perl正则，注意转义时需要双斜杠：\

\#\#\#\#\#mysql链接的编码

目前canal版本仅支持一个数据库只有一种编码，如果一个库存在多个编码，需要通过filter.regex配置，将其拆分为多个canal instance，为每个instance指定不同的编码



