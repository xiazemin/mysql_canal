阿里开源的Canal进行Mysql binlog数据的抽取，另需开发一个数据转换工具将从binlog中解析出的数据转换成自带schema的json数据并写入kafka中。而使用maxwell可直接完成对mysql binlog数据的抽取和转换成自带schema的json数据写入到kafka中。

另外Maxwell作为kafka connector的话需要metric的东西也比较多，因此此处我的kafka Connect选择了Canal.

实现Canal作为kafka的生产者，kafka作为消费者，还需要一个中间件。github上有给出这个，地址：[https://github.com/sasou/syncClient](https://github.com/sasou/syncClient)

以下为运行步骤：

首先配置Canal，下载deploy的tar包，可单机环境。下载解压后进行如下配置

$ vim \[Canal path\]/conf/example/instance.properties

修改如下一行

canal.instance.master.journal.name = mysql-bin.000001 \#mysql主库链接时起始的binlog文件

canal.instance.master.position = 4 \#mysql主库链接时起始的binlog偏移量，可不设置

canal.instance.defaultDatabaseName = test \#mysql链接时默认schema,选择一个你的mysql中存在的数据库

开启mysql的binlog写入功能，并且配置binlog模式为row

canal的原理是基于mysql binlog技术，所以这里一定需要开启mysql的binlog写入功能，并且配置binlog模式为row.

$ vim /etc/mysql/my.cnf

\[mysqld\]

log-bin=mysql-bin \#添加这一行就ok

binlog-format=ROW \#选择row模式

server\_id=1 \#配置mysql replaction需要定义，不能和canal的slaveId重复

注意完成后一定要重启mysql服务

$ service mysql stop

$ service mysql start

在mysql中添加Canal用户的权限

canal的原理是模拟自己为mysql slave，所以这里一定需要做为mysql slave的相关权限

在mysql&gt;下输入：

CREATE USER canal IDENTIFIED BY 'canal';

GRANT SELECT, REPLICATION SLAVE, REPLICATION CLIENT ON \*.\* TO 'canal'@'%';

GRANT SELECT,REPLICATION SLAVE, REPLICATION CLIENT ON \*.\* TO canal\[@localhost\];

FLUSH PRIVILEGES;

针对已有的账户可通过grants查询权限：

show grants \*\*for\*\* 'canal';

下载github上的syncClient

下载并解压缩数据实时同步中间件syncClient。

根据自身情况修改/syncClient/bin/SysConfig.properties

比如我运行的是canal的实例是example，且kafka在本地，所以进行了如下修改：

debug=1

ip=127.0.0.1

port=11111

destination=example

username=

password=

filter=

\#kafka

kafkaIp=127.0.0.1

kafkaPort=9092

注意这里的username，password和canal配置中的dbusername,dbpassword的区别。Canal配置中的dbusername,dbpassword是指canal读取mysql的binlog时的用户和密码，默认是canal,canal。但是这里的username和password是指canal自身的用户和密码（即连接到这个数据实时同步中间件时的用户和密码），默认是空。

之后在bin目录下运行

sh start.sh即可看到

empty 0

empty 1

……

这样的输出

传入数据

开启kafka,并启动另一个终端对mysql中对数据库中的表做修改（至于是不是必须是我们之前设置的canal.instance.defaultDatabaseName = test这个test数据库，还待进一步验证），修改后就可以在start.sh那个终端下看到变化了。如果有错误，请尝试关闭防火墙$ sudo ufw stop

使用kafka查看变化

如果想使用kafka查看数据库的变化，可以在启动消费者端查看

$ cd \[the path to kafka\]

$ bin/kafka-console-consumer.sh -zookeeper localhost:2181--from-beginning --topic 数据库名\_表名 \#比如你修改的是test数据下的user表，则此处topic为test\_user

当然你也可以在kafka中查看是否有这个topic

bin/kafka-topics.sh --list --zookeeper localhost:2181

