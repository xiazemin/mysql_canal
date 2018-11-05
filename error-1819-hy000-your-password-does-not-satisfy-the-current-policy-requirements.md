为了加强安全性，MySQL5.7为root用户随机生成了一个密码，在error log中，关于error log的位置，如果安装的是RPM包，则默认是/var/log/mysqld.log。



一般可通过log\_error设置



mysql&gt; select @@log\_error;

+---------------------+

\| @@log\_error         \|

+---------------------+

\| /var/log/mysqld.log \|

+---------------------+

1 row in set \(0.00 sec\)

可通过\# grep "password" /var/log/mysqld.log 命令获取MySQL的临时密码



2016-01-19T05:16:36.218234Z 1 \[Note\] A temporary password is generated for root@localhost: waQ,qR%be2\(5

