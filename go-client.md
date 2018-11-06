[https://github.com/CanalClient/canal-go](https://github.com/CanalClient/canal-go)

$vi conf/canal.properties

\# tcp, kafka, RocketMQ

canal.serverMode = tcp

connector := client.NewSimpleCanalConnector\("127.0.0.1", 11111, "", "", "example", 60000, 60\*60\*1000\)

===没有数据了===

===没有数据了===

================&gt; binlog\[mysql-bin.000003 : 19609\],name\[test,xdual\], eventType: INSERT

ID : 153  update= true

X : 2018-11-06 17:49:45  update= true

===没有数据了===

===没有数据了===

===没有数据了===

===没有数据了===

ls /otter/canal/destinations/example

\[running, cluster\]

~/kafka/server1/kafka\_2.10-0.8.2.1/bin$sh zookeeper-shell.sh 127.0.0.1:2181

stat /otter/canal/destinations/example/running



cZxid = 0xe0000016e

ctime = Tue Nov 06 18:04:34 CST 2018

mZxid = 0xe0000016e

mtime = Tue Nov 06 18:04:34 CST 2018

pZxid = 0xe0000016e

cversion = 0

dataVersion = 0

aclVersion = 0

ephemeralOwner = 0x166e40023d70020

dataLength = 54

numChildren = 0

