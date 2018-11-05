$sudo /usr/local/bin/mysqld &

\[1\] 55999

2018-11-05T09:24:59.448966Z 0 \[ERROR\] Fatal error: Please read "Security" section of the manual to find out how to run mysqld as root!

2018-11-05T09:24:59.449001Z 0 \[ERROR\] Aborting

2018-11-05T09:24:59.449032Z 0 \[Note\] Binlog end

\[1\]+  Exit 1                  sudo /usr/local/bin/mysqld

此处 mysql是出于安全考虑，默认拒绝用root账号启动mysql服务。

解决方法：

1.通过在命令后面加上--user=root 进行强制使用root账号启动。这样是最快的。

cd /etc/init.d

mysqld --user=root

2.使用一个普通用户进行启动mysqld 。这个用户必须是属于mysqld用户组，且在my.cnf文件中。使用 vi /etc/my.cnf

加上user=mysql  进行指定mysql用户来启动mysql服务。这样是最好的。

$sudo mysqld --user=root &

