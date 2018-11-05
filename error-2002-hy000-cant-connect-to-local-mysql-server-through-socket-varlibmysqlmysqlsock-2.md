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

3、\[root@lam7 opt\]\# mysql -h 127.0.0.1 （PS：有些用户也会出现此问题）

ERROR 1045 \(28000\): Access denied for user 'root'@'localhost' \(using password: NO\)

　这是由于我们连接数据库使用的主机名参数为“localhost”，或者未使用主机名参数、服务器默认使用“localhost”做为主机名。 使用主机名参数为“localhost”连接mysql服务端时，mysql客户端会认为是连接本机，所以会尝试以socket文件方式进行连接\(socket文件连接方式，比“ip：端口”方式效率更高\)，这时根据配置文件“/etc/mysql.cnf”的路径，未找到相应的socket文件，就会引发此错误。



三、修复故障前准备：

1、看mysql服务是否在运行：

　　由于“socket”文件是由mysql服务运行时创建的，如果提示“ERROR 2002 \(HY000\): Can't connect to local MySQL server through socket '\*\*\*' \(2\)”，找不到“socket”文件，我们首先要确认的是mysql服务是否正在运行。



\#1：端口是否打开



\[root@lam7 opt\]\# lsof -i:3306

COMMAND PID USER FD TYPE DEVICE SIZE/OFF NODE NAME

mysqld 57436 mysql 17u IPv6 160456 0t0 TCP \*:mysql \(LISTEN\)



\#2：mysqld服务是否正在运行（小七这边用的是centos7，所以会提示使用“/bin/systemctl status mysqld.service”）

