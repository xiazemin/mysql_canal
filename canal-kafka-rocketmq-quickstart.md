canal 1.1.1版本之后, 默认支持将canal server接收到的binlog数据直接投递到MQ, 目前默认支持的MQ系统有:



kafka: https://github.com/apache/kafka

RocketMQ : https://github.com/apache/rocketmq

环境版本

操作系统：CentOS release 6.6 \(Final\)

java版本: jdk1.8

canal 版本: 请下载最新的安装包，本文以当前v1.1.1 的canal.deployer-1.1.1.tar.gz为例

MySQL版本 ：5.7.18

注意 ： 关闭所有机器的防火墙，同时注意启动可以相互telnet ip 端口

一、 安装zookeeper

参考：Zookeeper QuickStart



二、安装MQ

Kafka安装参考：Kafka QuickStart

RocketMQ安装参考：RocketMQ QuickStart

三、 安装canal.server

3.1 下载压缩包

到官网地址\(release\)下载最新压缩包,请下载 canal.deployer-latest.tar.gz



3.2 将canal.deployer 复制到固定目录并解压

mkdir -p /usr/local/canal

cp   canal.deployer-1.1.1.tar.gz   /usr/local/canal

tar -zxvf canal.deployer-1.1.1.tar.gz 

3.3 配置修改参数

a. 修改instance 配置文件 vi conf/example/instance.properties

\#  按需修改成自己的数据库信息

\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#

...

canal.instance.master.address=192.168.1.20:3306

\# username/password,数据库的用户名和密码

...

canal.instance.dbUsername = canal

canal.instance.dbPassword = canal

...

\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#



对应ip 地址的MySQL 数据库需进行相关初始化与设置, 可参考 Canal QuickStart



b. 修改canal 配置文件vi /usr/local/canal/conf/canal.properties

\# ...

\# 可选项: tcp\(默认\), kafka, RocketMQ

canal.serverMode = kafka

\# ...

其他详细参数可参考Canal AdminGuide



c. 修改mq配置文件vi /usr/local/canal/conf/mq.yml

\# kafka/rocketmq 集群配置: 192.168.1.117:9092,192.168.1.118:9092,192.168.1.119:9092 

servers: 192.168.1.117:9092

retries: 0

batchSize: 16384

lingerMs: 1

bufferMemory: 33554432



\# Canal的batch size, 默认50K, 由于kafka最大消息体限制请勿超过1M\(900K以下\)

canalBatchSize: 50

\# Canal get数据的超时时间, 单位: 毫秒, 空为不限超时

canalGetTimeout: 100

flatMessage: true



canalDestinations:

  - canalDestination: example

    topic: example

    partition: 1

\#  \#对应topic分区数量

\#  partitionsNum: 3

\#  partitionHash:

\#    \#库名.表名: 唯一主键

\#    mytest.person: id

mq.yml参数说明

参数名	参数说明	默认值

servers	kafka为bootstrap.servers 

rocketMQ中为nameserver列表	127.0.0.1:6667

retries	发送失败重试次数	0

batchSize	kafka为ProducerConfig.BATCH\_SIZE\_CONFIG 

rocketMQ无意义	16384

lingerMs	kafka为ProducerConfig.LINGER\_MS\_CONFIG 

rocketMQ无意义	1

bufferMemory	kafka为ProducerConfig.BUFFER\_MEMORY\_CONFIG 

rocketMQ无意义	33554432

producerGroup	kafa无意义 

rocketMQ为ProducerGroup名	Canal-Producer

canalBatchSize	获取canal数据的批次大小	50

canalGetTimeout	获取canal数据的超时时间	100

flatMessage	是否为json格式 

如果设置为false,对应MQ收到的消息为protobuf格式

需要通过CanalMessageDeserializer进行解码	true

CanalDestinations	通道定义，父级目录	--

CanalDestination	通道名称	无

topic	mq里的topic名	无

partition	单队列模式的分区下标，	1

partitionsNum	散列模式的分区数	无

partitionHash	散列规则定义 

库名.表名 : 唯一主键，比如mytest.person: id	无

3.4 启动

cd /usr/local/canal/

sh bin/startup.sh

3.5 查看日志

a.查看 logs/canal/canal.log



vi logs/canal/canal.log

b. 查看instance的日志：



vi logs/example/example.log

3.6 关闭

cd /usr/local/canal/

sh bin/stop.sh

3.7 MQ数据消费

canal.client下有对应的MQ数据消费的样例工程,包含数据编解码的功能



kafka模式: com.alibaba.otter.canal.client.running.kafka.CanalKafkaClientExample

rocketMQ模式: com.alibaba.otter.canal.client.running.rocketmq.CanalRocketMQClientExample

