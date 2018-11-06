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



