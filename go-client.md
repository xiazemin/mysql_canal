[https://github.com/CanalClient/canal-go](https://github.com/CanalClient/canal-go)



$vi conf/canal.properties

\# tcp, kafka, RocketMQ

canal.serverMode = tcp

connector := client.NewSimpleCanalConnector\("127.0.0.1", 11111, "", "", "example", 60000, 60\*60\*1000\)

