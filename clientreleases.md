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



