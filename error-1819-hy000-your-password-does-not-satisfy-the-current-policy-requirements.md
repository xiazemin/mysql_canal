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

这个其实与validate\_password\_policy的值有关。

validate\_password\_policy有以下取值：

Policy    Tests Performed

0 or LOW    Length

1 or MEDIUM    Length; numeric, lowercase/uppercase, and special characters

2 or STRONG    Length; numeric, lowercase/uppercase, and special characters; dictionary file

默认是1，即MEDIUM，所以刚开始设置的密码必须符合长度，且必须含有数字，小写或大写字母，特殊字符。

有时候，只是为了自己测试，不想密码设置得那么复杂，譬如说，我只想设置root的密码为123456。

必须修改两个全局参数：

首先，修改validate\_password\_policy参数的值

mysql&gt; set global validate\_password\_policy=0;

Query OK, 0 rows affected \(0.00 sec\)

这样，判断密码的标准就基于密码的长度了。这个由validate\_password\_length参数来决定。

复制代码

mysql&gt; select @@validate\_password\_length;

+----------------------------+

\| @@validate\_password\_length \|

+----------------------------+

\|                          8 \|

+----------------------------+

1 row in set \(0.00 sec\)

复制代码

validate\_password\_length参数默认为8，它有最小值的限制，最小值为：

validate\_password\_number\_count

* validate\_password\_special\_char\_count

* \(2 \* validate\_password\_mixed\_case\_count\)

其中，validate\_password\_number\_count指定了密码中数据的长度，validate\_password\_special\_char\_count指定了密码中特殊字符的长度，validate\_password\_mixed\_case\_count指定了密码中大小字母的长度。

这些参数，默认值均为1，所以validate\_password\_length最小值为4，如果你显性指定validate\_password\_length的值小于4，尽管不会报错，但validate\_password\_length的值将设为4。如下所示：

复制代码

mysql&gt; select @@validate\_password\_length;

+----------------------------+

\| @@validate\_password\_length \|

+----------------------------+

\|                          8 \|

+----------------------------+

1 row in set \(0.00 sec\)

mysql&gt; set global validate\_password\_length=1;

Query OK, 0 rows affected \(0.00 sec\)

mysql&gt; select @@validate\_password\_length;

+----------------------------+

\| @@validate\_password\_length \|

+----------------------------+

\|                          4 \|

+----------------------------+

1 row in set \(0.00 sec\)

复制代码

如果修改了validate\_password\_number\_count，validate\_password\_special\_char\_count，validate\_password\_mixed\_case\_count中任何一个值，则validate\_password\_length将进行动态修改。

复制代码

mysql&gt; select @@validate\_password\_length;

+----------------------------+

\| @@validate\_password\_length \|

+----------------------------+

\|                          4 \|

+----------------------------+

1 row in set \(0.00 sec\)

mysql&gt; select @@validate\_password\_mixed\_case\_count;

+--------------------------------------+

\| @@validate\_password\_mixed\_case\_count \|

+--------------------------------------+

\|                                    1 \|

+--------------------------------------+

1 row in set \(0.00 sec\)

mysql&gt; set global validate\_password\_mixed\_case\_count=2;

Query OK, 0 rows affected \(0.00 sec\)

mysql&gt; select @@validate\_password\_mixed\_case\_count;

+--------------------------------------+

\| @@validate\_password\_mixed\_case\_count \|

+--------------------------------------+

\|                                    2 \|

+--------------------------------------+

1 row in set \(0.00 sec\)

mysql&gt; select @@validate\_password\_length;

+----------------------------+

\| @@validate\_password\_length \|

+----------------------------+

\|                          6 \|

+----------------------------+

1 row in set \(0.00 sec\)

复制代码

当然，前提是validate\_password插件必须已经安装，MySQL5.7是默认安装的。

那么如何验证validate\_password插件是否安装呢？可通过查看以下参数，如果没有安装，则输出将为空。

复制代码

mysql&gt; SHOW VARIABLES LIKE 'validate\_password%';

+--------------------------------------+-------+

\| Variable\_name                        \| Value \|

+--------------------------------------+-------+

\| validate\_password\_dictionary\_file    \|       \|

\| validate\_password\_length             \| 6     \|

\| validate\_password\_mixed\_case\_count   \| 2     \|

\| validate\_password\_number\_count       \| 1     \|

\| validate\_password\_policy             \| LOW   \|

\| validate\_password\_special\_char\_count \| 1     \|

+--------------------------------------+-------+

6 rows in set \(0.00 sec\)

最简单办法

\#username/password，需要改成自己的数据库信息

canal.instance.dbUsername = canal



canal.instance.dbPassword = canal12345678

