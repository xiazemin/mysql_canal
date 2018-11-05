Can’t connect to local MySQL server through socket ‘/var/lib/mysql/mysql.sock’解决方法

启动mysql 报错：

ERROR 2002 \(HY000\): Can’t connect to local MySQL server through socket ‘/var/lib/mysql/mysql.sock’ \(2\)

1、先查看 /etc/rc.d/init.d/mysqld status 看看m y s q l 是否已经启动.

另外看看是不是权限问题.

2、确定你的mysql.sock是不是在那个位置，

mysql -u 你的mysql用户名 -p -S /var/lib/mysql/mysql.sock

3、试试：service mysqld start

4、如果是权限问题，则先改变权限 \#chown -R mysql:mysql /var/lib/mysql

\[root@localhost ~\]\# /etc/init.d/mysqld start

启动 MySQL： \[ 确定 \]

\[root@localhost ~\]\# mysql -uroot -p

ERROR 2002 \(HY000\): Can't connect to local MySQL server through socket '/var/lib/mysql/mysql.sock' \(2\)

原因是，/var/lib/mysql 的访问权限问题。

shell&gt; chown -R mysql:mysql /var/lib/mysql

接着启动服务器

shell&gt; /etc/init.d/mysql start

服务器正常启动后察看 /var/lib/mysql 自动生成mysql.sock文件。

1、直接使用“mysql”命令，不带主机名参数；

2、使用带了主机名“localhost”参数的“mysql -h localhost”命令；

3、使用带了主机名“127.0.0.1”参数的“mysql -h 127.0.0.1”命令。

1、\[root@lam7 opt\]\# mysql

ERROR 2002 \(HY000\): Can't connect to local MySQL server through socket '/var/lib/mysql/mysql.sock' \(2\)



2、\[root@lam7 opt\]\# mysql -h localhost

ERROR 2002 \(HY000\): Can't connect to local MySQL server through socket '/var/lib/mysql/mysql.sock' \(2\)





