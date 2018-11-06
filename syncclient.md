[https://github.com/sasou/syncClient](https://github.com/sasou/syncClient)

使用canal，将mysql的表数据实时同步到kafka、redis、elasticsearch、httpmq；

基本原理：

canal解析binlog的数据，由syncClient订阅，然后实时推送到kafka或者redis、elasticsearch、httpmq；如果kafka、redis、es、httpmq服务异常，syncClient会回滚操作；canal、kafka、redis、es、httpmq的异常退出，都不会影响数据的传输；

目录：

bin：已编译二进制项目，可以直接使用；

src：源代码；

配置说明：

\#common

system.debug=1 \# 是否开始调试：1未开启，0为关闭（线上运行请关闭）

\#canal server

canal.ip=127.0.0.1 \# canal 服务端 ip;

canal.port=11111 \# canal 服务端 端口：默认11111;

canal.destination=one \# canal 服务端项目（destinations），多个用逗号分隔，如：redis,kafka;

canal.username= \# canal 用户名：默认为空;

canal.password= \# canal 密码：默认为空;

\#redis plugin

redis.target\_type=redis \# 同步插件类型 kafka or redis、elasticsearch、httpmq redis.target\_ip= \# redis服务端 ip;

redis.target\_port= \# redis端口：默认6379;

redis.target\_deep= \# 同步到redis的队列名称规则：1、sync\_{项目名}{db}{table}; 2、sync\_{项目名}{db};1、sync{项目名}; 4、sync\_{db}\_{table}; 默认1；

\#kafka plugin

kafka.target\_type=kafka \# 同步插件类型 kafka

kafka.target\_ip= \# kafka服务端 ip;

kafka.target\_port= \# kafka端口：默认9092;

kafka.target\_deep= \# 同步到kafka的集合名称规则：1、sync\_{项目名}{db}{table}; 2、sync\_{项目名}{db};1、sync{项目名}; 4、sync\_{db}\_{table}; 默认1；

\#elasticsearch plugin

es.target\_type=elasticsearch \# 同步插件类型elasticsearch

es.target\_ip=10.5.3.66 \# es服务端 ip; es.target\_port= \# es端口：默认9200; es.target\_deep= \# 同步到es的index名称规则：1、sync\_{项目名}{db}{table}; 2、sync\_{项目名}{db};1、sync{项目名}; 4、sync\_{db}\_{table}; 默认1；

\#httpmq plugin

httpmq.target\_type=httpmq \# 同步插件类型 httpmq

httpmq.target\_ip=10.5.3.66 \# httpmq服务端 ip; httpmq.target\_port=1218 \# httpmq端口：默认 1218

httpmq.target\_deep= \# 同步到httpmq的队列名称规则：1、sync\_{项目名}{db}{table}; 2、sync\_{项目名}{db};1、sync{项目名}; 4、sync\_{db}\_{table}; 默认1；

wget  [https://github.com/sasou/syncClient/archive/1.1.2.zip](https://github.com/sasou/syncClient/archive/1.1.2.zip)

Error: Unable to access jarfile syncClient.jar

$java -jar bin/syncClient.jar

Caused by: java.lang.NumberFormatException: For input string: "null"

