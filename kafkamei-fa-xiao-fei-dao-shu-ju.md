```
$vi conf/logback.xml

      &lt;root level="WARN"&gt;

            &lt;appender-ref ref="STDOUT"/&gt;

            &lt;appender-ref ref="CANAL-ROOT" /&gt;

    &lt;/root&gt;
```

改成

```
   &lt;root level="INFO"&gt;

            &lt;appender-ref ref="STDOUT"/&gt;

            &lt;appender-ref ref="CANAL-ROOT" /&gt;

    &lt;/root&gt;
```

看到

2018-11-07 11:42:34.354 \[pool-4-thread-1\] INFO  c.a.otter.canal.server.embedded.CanalServerWithEmbedded - getWithoutAck successfully, clientId:1001 batchSize:50  real size is 3 and result is \[batchId:2 , position:PositionRange\[start=LogPosition\[identity=LogIdentity\[sourceAddress=localhost/127.0.0.1:3306,slaveId=-1\],postion=EntryPosition\[included=false,journalName=mysql-bin.000003,position=26525,serverId=1,gtid=,timestamp=1541562153000\]\],ack=LogPosition\[identity=LogIdentity\[sourceAddress=localhost/127.0.0.1:3306,slaveId=-1\],postion=EntryPosition\[included=false,journalName=mysql-bin.000003,position=26817,serverId=1,gtid=,timestamp=1541562153000\]\],end=LogPosition\[identity=LogIdentity\[sourceAddress=localhost/127.0.0.1:3306,slaveId=-1\],postion=EntryPosition\[included=false,journalName=mysql-bin.000003,position=26817,serverId=1,gtid=,timestamp=1541562153000\]\]\]\]

2018-11-07 11:43:34.381 \[pool-4-thread-1\] INFO  c.a.otter.canal.server.embedded.CanalServerWithEmbedded - ack successfully, clientId:1001 batchId:2 position:PositionRange\[start=LogPosition\[identity=LogIdentity\[sourceAddress=localhost/127.0.0.1:3306,slaveId=-1\],postion=EntryPosition\[included=false,journalName=mysql-bin.000003,position=26525,serverId=1,gtid=,timestamp=1541562153000\]\],ack=LogPosition\[identity=LogIdentity\[sourceAddress=localhost/127.0.0.1:3306,slaveId=-1\],postion=EntryPosition\[included=false,journalName=mysql-bin.000003,position=26817,serverId=1,gtid=,timestamp=1541562153000\]\],end=LogPosition\[identity=LogIdentity\[sourceAddress=localhost/127.0.0.1:3306,slaveId=-1\],postion=EntryPosition\[included=false,journalName=mysql-bin.000003,position=26817,serverId=1,gtid=,timestamp=1541562153000\]\]\]

2018-11-07 11:43:34.381 \[pool-4-thread-1\] INFO  c.a.otter.canal.server.embedded.CanalServerWithEmbedded - ack successfully, clientId:1001 batchId:2 position:PositionRange\[start=LogPosition\[identity=LogIdentity\[sourceAddress=localhost/127.0.0.1:3306,slaveId=-1\],postion=EntryPosition\[included=false,journalName=mysql-bin.000003,position=26525,serverId=1,gtid=,timestamp=1541562153000\]\],ack=LogPosition\[identity=LogIdentity\[sourceAddress=localhost/127.0.0.1:3306,slaveId=-1\],postion=EntryPosition\[included=false,journalName=mysql-bin.000003,position=26817,serverId=1,gtid=,timestamp=1541562153000\]\],end=LogPosition\[identity=LogIdentity\[sourceAddress=localhost/127.0.0.1:3306,slaveId=-1\],postion=EntryPosition\[included=false,journalName=mysql-bin.000003,position=26817,serverId=1,gtid=,timestamp=1541562153000\]\]\]

