wget [https://github.com/alibaba/canal/releases/download/canal-1.1.0/canal.kafka-1.1.0.tar.gz](https://github.com/alibaba/canal/releases/download/canal-1.1.0/canal.kafka-1.1.0.tar.gz)

tar -zxvf canal.kafka-1.1.0.tar.gz

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

```
topic: mytopic
```

2018-11-06T06:29:32.765547Z 205 \[Note\] Aborted connection 205 to db: 'unconnected' user: 'canal' host: 'localhost' \(Got an error reading communication packets\)

2018-11-06T06:29:32.772654Z 206 \[Note\] Start binlog\_dump to master\_thread\_id\(206\) slave\_server\(12345\), pos\(mysql-bin.000003, 4\)

