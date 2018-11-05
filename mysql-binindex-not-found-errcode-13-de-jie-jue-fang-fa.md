020101 00:42:21  mysqld started

/usr/local/mysql/libexec/mysqld: File './mysql-bin.index' not found \(Errcode: 13\)

020101  0:42:21 \[ERROR\] Aborting



020101  0:42:21 \[Note\] /usr/local/mysql/libexec/mysqld: Shutdown complete



提示./mysql-bin.index无法找到（由于mysql开启了bin日志功能），到数据库根目录查看该文件是存在的，可能是文件权限的问题，查看了数据库根目录的权限是700，所有者和用户组都是root，可能是上次转移数据库的时候不小心修改了文件夹的权限。



解决方法：



chgrp -R mysql /usr/local/mysql/data && chown -R mysql /usr/local/mysql/data 



重新启动mysql  \[OK\]

