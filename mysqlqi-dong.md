cat ~/.my.conf

\[client\]

socket=/usr/local/var/mysql/mysql.sock

\[mysqld\]

socket=/usr/local/var/mysql/mysql.sock

\# log\_bin

log-bin = mysql-bin \#开启binlog

binlog-format = ROW \#选择row模式

server\_id = 1 \#配置mysql replication需要定义，不能喝canal的slaveId重复

