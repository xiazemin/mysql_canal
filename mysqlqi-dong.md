cat ~/.my.conf

\[client\]

socket=/usr/local/var/mysql/mysql.sock

\[mysqld\]

socket=/usr/local/var/mysql/mysql.sock

\# log\_bin

log-bin = mysql-bin \#开启binlog

binlog-format = ROW \#选择row模式

server\_id = 1 \#配置mysql replication需要定义，不能喝canal的slaveId重复



 采用netstat -nlp查看mysql服务的状态

   命令行方式：

        开启  ./mysqld\_safe &

        关闭  mysqladmin -uroot shutdown

   rpm方式安装的

        开启  service mysql start

        关闭  service mysql stop

   在命令行启动mysql时，如不加"--console"，启动、关闭信息不在界面中显示，而是记录在安装目录下的data目录里，文件名一般是hostname.err,通过此文件查看mysql的控制台信息。

