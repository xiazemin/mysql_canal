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

\|参数名字	\|参数说明	\|默认值\|

\| ------------- \|:-------------😐 -----😐

\|canal.destinations\|	当前server上部署的instance列表\|	无

\|canal.conf.dir\|	conf/目录所在的路径	\|…/conf

\|canal.auto.scan\|	开启instance自动扫描如果配置为true，canal.conf.dir目录下的instance配置变化会自动触发：a. instance目录新增： 触发instance配置载入，lazy为true时则自动启动b. instance目录删除：卸载对应instance配置，如已启动则进行关闭c. instance.properties文件变化：reload instance配置，如已启动自动进行重启操作 \|	true

\|canal.auto.scan.interval	\|instance自动扫描的间隔时间，单位秒\|	5

\|canal.instance.global.mode\|	全局配置加载方式	\|spring

\|canal.instance.global.lazy\|	全局lazy模式	\|false

\|canal.instance.global.manager.address\|	全局的manager配置方式的链接信息\|	无

\|canal.instance.global.spring.xml\|	全局的spring配置方式的组件文件	\|classpath:spring/memory-instance.xml \(spring目录相对于canal.conf.dir\)

\|canal.instance.example.mode canal.instance.example.lazy canal.instance.example.spring.xml…	\| instance级别的配置定义，如有配置，会自动覆盖全局配置定义模式命名规则：canal.instance.{name}.xxx	\|无

\|canal.instance.tsdb.spring.xml\|	v1.0.25版本新增,全局的tsdb配置方式的组件文件	\|classpath:spring/tsdb/h2-tsdb.xml \(spring目录相对于canal.conf.dir\)



common参数定义，比如可以将instance.properties的公用参数，抽取放置到这里，这样每个instance启动的时候就可以共享. 【instance.properties配置定义优先级高于canal.properties】



