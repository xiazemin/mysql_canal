[https://github.com/alibaba/canal/wiki/ClientExample](https://github.com/alibaba/canal/wiki/ClientExample)

直接使用canal.example工程

a. 首先启动Canal Server，可参见QuickStart

b.

可以在eclipse里，直接打开com.alibaba.otter.canal.example.SimpleCanalClientTest，直接运行

在工程的example目录下运行命令行：

mvn exec:java -Dexec.mainClass="com.alibaba.otter.canal.example.SimpleCanalClientTest"

下载example包: [https://github.com/alibaba/canal/releases，解压缩后，直接运行sh](https://github.com/alibaba/canal/releases，解压缩后，直接运行sh) startup.sh脚本

c. 触发数据变更 d. 在控制台或者logs中查看，可以看到如下信息 ：

================&gt; binlog\[mysql-bin.002579:508882822\] , name\[retl,xdual\] , eventType : UPDATE , executeTime : 1368607728000 , delay : 4270ms

-------&gt; before

ID : 1    update=false

X : 2013-05-15 11:43:42    update=false

-------&gt; after

ID : 1    update=false

X : 2013-05-15 16:48:48    update=true

从头创建工程

依赖配置：

&lt;dependency&gt;

```
&lt;groupId&gt;com.alibaba.otter&lt;/groupId&gt;

&lt;artifactId&gt;canal.client&lt;/artifactId&gt;

&lt;version&gt;1.1.0&lt;/version&gt;
```

&lt;/dependency&gt;

1. 创建mvn标准工程：

mvn archetype:create -DgroupId=com.alibaba.otter -DartifactId=canal.sample

maven3.0.5以上版本舍弃了create，使用generate生成项目

mvn archetype:generate -DgroupId=com.alibaba.otter -DartifactId=canal.sample

1. 修改pom.xml，添加依赖

4. 运行Client



首先启动Canal Server，可参见QuickStart



启动Canal Client后，可以从控制台从看到类似消息：



empty count : 1

empty count : 2

empty count : 3

empty count : 4

此时代表当前数据库无变更数据



5. 触发数据库变更



mysql&gt; use test;

Database changed

mysql&gt; CREATE TABLE \`xdual\` \(

    -&gt;   \`ID\` int\(11\) NOT NULL AUTO\_INCREMENT,

    -&gt;   \`X\` timestamp NOT NULL DEFAULT CURRENT\_TIMESTAMP,

    -&gt;   PRIMARY KEY \(\`ID\`\)

    -&gt; \) ENGINE=InnoDB AUTO\_INCREMENT=3 DEFAULT CHARSET=utf8 ;

Query OK, 0 rows affected \(0.06 sec\)

mysql&gt; insert into xdual\(id,x\) values\(null,now\(\)\);Query OK, 1 row affected \(0.06 sec\)



可以从控制台中看到：



empty count : 1

empty count : 2

empty count : 3

empty count : 4

================&gt; binlog\[mysql-bin.001946:313661577\] , name\[test,xdual\] , eventType : INSERT

ID : 4    update=true

X : 2013-02-05 23:29:46    update=true

