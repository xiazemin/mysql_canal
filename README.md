# [Introduction](https://github.com/alibaba/canal/wiki/QuickStart)

几点说明：\(mysql初始化\)

a. canal的原理是基于mysql binlog技术，所以这里一定需要开启mysql的binlog写入功能，建议配置binlog模式为row.



\*\*针对阿里云RDS账号默认已经有binlog dump权限,不需要任何权限或者binlog设置,可以直接跳过这一步\*\*

\[mysqld\]

log-bin=mysql-bin \#添加这一行就ok

binlog-format=ROW \#选择row模式

server\_id=1 \#配置mysql replaction需要定义，不能和canal的slaveId重复

b. canal的原理是模拟自己为mysql slave，所以这里一定需要做为mysql slave的相关权限.

CREATE USER canal IDENTIFIED BY 'canal';  

GRANT SELECT, REPLICATION SLAVE, REPLICATION CLIENT ON \*.\* TO 'canal'@'%';

-- GRANT ALL PRIVILEGES ON \*.\* TO 'canal'@'%' ;

FLUSH PRIVILEGES;

针对已有的账户可直接通过grant



其他场景的使用

基于canal的docker模式快速启动，参考：Docker QuickStart

如何将canal链接aliyun rds，参考：Aliyun RDS QuickStart

如果将canal消息直接投递给kafka/RocketMQ，参考：Canal-Kafka-RocketMQ-QuickStart

启动步骤：

1. 下载canal



直接下载



访问：https://github.com/alibaba/canal/releases ，会列出所有历史的发布版本包 下载方式，比如以1.0.17版本为例子：

wget https://github.com/alibaba/canal/releases/download/canal-1.0.17/canal.deployer-1.0.17.tar.gz

or



自己编译



git clone git@github.com:alibaba/canal.git

cd canal; 

mvn clean install -Dmaven.test.skip -Denv=release

编译完成后，会在根目录下产生target/canal.deployer-$version.tar.gz



2. 解压缩



mkdir /tmp/canal

tar zxvf canal.deployer-$version.tar.gz  -C /tmp/canal

解压完成后，进入/tmp/canal目录，可以看到如下结构：



drwxr-xr-x 2 jianghang jianghang  136 2013-02-05 21:51 bin

drwxr-xr-x 4 jianghang jianghang  160 2013-02-05 21:51 conf

drwxr-xr-x 2 jianghang jianghang 1.3K 2013-02-05 21:51 lib

drwxr-xr-x 2 jianghang jianghang   48 2013-02-05 21:29 logs

3. 配置修改



应用参数：



vi conf/example/instance.properties

\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#

\#\# mysql serverId

canal.instance.mysql.slaveId = 1234

\#position info，需要改成自己的数据库信息

canal.instance.master.address = 127.0.0.1:3306

canal.instance.master.journal.name =

canal.instance.master.position =

canal.instance.master.timestamp =





\#canal.instance.standby.address =

\#canal.instance.standby.journal.name =

\#canal.instance.standby.position =

\#canal.instance.standby.timestamp =





\#username/password，需要改成自己的数据库信息

canal.instance.dbUsername = canal



canal.instance.dbPassword = canal

canal.instance.defaultDatabaseName =

canal.instance.connectionCharset = UTF-8





\#table regex

canal.instance.filter.regex = .\..





\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#



说明：



canal.instance.connectionCharset 代表数据库的编码方式对应到java中的编码类型，比如UTF-8，GBK , ISO-8859-1

4. 准备启动



sh bin/startup.sh

5. 查看日志



vi logs/canal/canal.log

2013-02-05 22:45:27.967 \[main\] INFO  com.alibaba.otter.canal.deployer.CanalLauncher - \#\# start the canal server.

2013-02-05 22:45:28.113 \[main\] INFO  com.alibaba.otter.canal.deployer.CanalController - \#\# start the canal server\[10.1.29.120:11111\]

2013-02-05 22:45:28.210 \[main\] INFO  com.alibaba.otter.canal.deployer.CanalLauncher - \#\# the canal server is running now ......

具体instance的日志：



vi logs/example/example.log

2013-02-05 22:50:45.636 \[main\] INFO  c.a.o.c.i.spring.support.PropertyPlaceholderConfigurer - Loading properties file from class path resource \[canal.properties\]

2013-02-05 22:50:45.641 \[main\] INFO  c.a.o.c.i.spring.support.PropertyPlaceholderConfigurer - Loading properties file from class path resource \[example/instance.properties\]

2013-02-05 22:50:45.803 \[main\] INFO  c.a.otter.canal.instance.spring.CanalInstanceWithSpring - start CannalInstance for 1-example 

2013-02-05 22:50:45.810 \[main\] INFO  c.a.otter.canal.instance.spring.CanalInstanceWithSpring - start successful....

6. 关闭



sh bin/stop.sh

it's over.

