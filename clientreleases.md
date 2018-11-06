create database test;

use test;

Database changed

CREATE TABLE \`xdual\` \( \`ID\` int\(11\) NOT NULL AUTO\_INCREMENT, \`X\` timestamp NOT NULL DEFAULT CURRENT\_TIMESTAMP, PRIMARY KEY \(\`ID\`\)  \) ENGINE=InnoDB AUTO\_INCREMENT=3 DEFAULT CHARSET=utf8;

Query OK, 0 rows affected \(0.03 sec\)

insert into xdual\(id,x\) values\(null,now\(\)\);

Query OK, 1 row affected \(0.01 sec\)

$wget [https://github.com/alibaba/canal/releases/download/canal-1.1.1/canal.example-1.1.1.tar.gz](https://github.com/alibaba/canal/releases/download/canal-1.1.1/canal.example-1.1.1.tar.gz)

mkdir   client

tar -zxvf canal.example-1.1.1.tar.gz

sh bin/init.sh -p 3306 -h 127.0.0.1

Can not find instance config file: /Users/didi/canal/client/bin/../conf/example/instance.properties

$mkdir conf/example

11:38:56-didi@bogon:~/canal/client$cp ../conf/example/instance.properties conf/example/

canal.instance.filter.black.regex=

\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#

Above is your instance config file, check if the settings are correct

$sh bin/startup.sh

cd to /Users/didi/canal/client/bin for workaround relative path

LOG CONFIGURATION : /Users/didi/canal/client/bin/../conf/logback.xml

client mode : Simple

测试

mysql&gt; insert into xdual\(id,x\) values\(null,now\(\)\);

Query OK, 1 row affected \(0.02 sec\)

$  tail -f logs/example/entry.log

================&gt; binlog\[mysql-bin.000003:14523\] , executeTime : 1541475018000\(2018-11-06 11:30:18\) , gtid : \(\) , delay : 623693ms

================&gt; binlog\[mysql-bin.000003:15159\] , executeTime : 1541475847000\(2018-11-06 11:44:07\) , gtid : \(\) , delay : 824ms

BEGIN ----&gt; Thread id: 137

----------------&gt; binlog\[mysql-bin.000003:15289\] , name\[test,xdual\] , eventType : INSERT , executeTime : 1541475847000\(2018-11-06 11:44:07\) , gtid : \(\) , delay : 824 ms

ID : 137    type=int\(11\)    update=true

X : 2018-11-06 11:44:07    type=timestamp    update=true

---

END ----&gt; transaction id: 2115

================&gt; binlog\[mysql-bin.000003:15333\] , executeTime : 1541475847000\(2018-11-06 11:44:07\) , gtid : \(\) , delay : 825ms

成功



