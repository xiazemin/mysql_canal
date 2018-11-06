canal-kafka是把kafka作为客户端，嵌入到canal中，并且在canal基础上对源码进行了修改，以达到特定的实现canal到kafka的传送。

canal-kafka是阿里云最近更新的一个新的安装包。主要功能是实现canal与kafka的对接，实现海量的消息传输同步。在canal-kafka中，消息是以ByteString进行传输的，并且用户只能通过配置来指定一些kafka的配置，从某种程度上有一定的局限性，所以我们使用canal来自定义客户端kafka，会有更好的灵活性，但维护成本会更大，所以如何选择根据实际情况而定。

构建maven依赖

&lt;dependency&gt;

```
&lt;groupId&gt;com.alibaba.otter&lt;/groupId&gt;

&lt;artifactId&gt;canal.client&lt;/artifactId&gt;

&lt;version&gt;1.0.25&lt;/version&gt;
```

&lt;/dependency&gt;

&lt;dependency&gt;

```
&lt;groupId&gt;org.apache.kafka&lt;/groupId&gt;

&lt;artifactId&gt;kafka-clients&lt;/artifactId&gt;

&lt;version&gt;1.1.0&lt;/version&gt;
```

&lt;/dependency&gt;

SimpleCanalClient

package com.unigroup.client.canal;

import java.lang.reflect.InvocationTargetException;

import java.lang.reflect.Method;

import java.net.InetSocketAddress;

import java.util.List;

import com.alibaba.otter.canal.client.CanalConnector;

import com.alibaba.otter.canal.client.CanalConnectors;

import com.alibaba.otter.canal.protocol.CanalEntry.Entry;

import com.alibaba.otter.canal.protocol.Message;

import com.unigroup.core.canal.CanalToKG;

/\*\*

\* @Title: SimpleCanalClient.java

\* @Package com.unigroup.canal

\* @Description: canal單實例接口

\* @author 桃花惜春风

\* @date 2018年8月29日 上午11:56:09

\* @version V1.0

\*/

public class SimpleCanalClient {

```
private CanalConnector connector=null;



public SimpleCanalClient\(String ip,String port,String instance\) {



    // 创建链接

    connector = CanalConnectors.newSingleConnector\(new InetSocketAddress\(ip, Integer.parseInt\(port\)\),instance, "", ""\);

}

public List&lt;Entry&gt; execute\(int batchSize,Class&lt;?&gt; clazz \) throws InstantiationException, IllegalAccessException, NoSuchMethodException, SecurityException {



    //int batchSize = 1;

    int emptyCount = 0;

    Object obj = clazz.newInstance\(\);

    Method method = clazz.getMethod\("send",Message.class\);

    try {

        connector.connect\(\);

        // connector.subscribe\(".\*\\..\*"\);

        connector.subscribe\("test.test1"\);



        connector.rollback\(\);

        int totalEmptyCount = 120;

        while \(emptyCount &lt; totalEmptyCount\) {

            Message message = connector.getWithoutAck\(batchSize\); // 获取指定数量的数据

            long batchId = message.getId\(\);

            int size = message.getEntries\(\).size\(\);

            if \(batchId == -1 \|\| size == 0\) {

                emptyCount++;

                System.out.println\("empty count : " + emptyCount\);

                try {

                    Thread.sleep\(1000\);

                } catch \(InterruptedException e\) {

                }

            } else {

                emptyCount = 0;

                method.invoke\(obj, message\);            

            }

            connector.ack\(batchId\); // 提交确认



            // connector.rollback\(batchId\); // 处理失败, 回滚数据

        }



        System.out.println\("empty too many times, exit"\);

    } catch \(IllegalAccessException e\) {

        // TODO Auto-generated catch block

        e.printStackTrace\(\);

    } catch \(IllegalArgumentException e\) {

        // TODO Auto-generated catch block

        e.printStackTrace\(\);

    } catch \(InvocationTargetException e\) {

        // TODO Auto-generated catch block

        e.printStackTrace\(\);

    } finally {

        connector.disconnect\(\);

    }

    return null;

}
```

}

CanalKafkaProducer

package com.unigroup.kafka.producer;

import java.io.IOException;

import java.util.Properties;

import org.apache.kafka.clients.producer.KafkaProducer;

import org.apache.kafka.clients.producer.Producer;

import org.apache.kafka.clients.producer.ProducerRecord;

import org.apache.kafka.common.serialization.StringSerializer;

import org.slf4j.Logger;

import org.slf4j.LoggerFactory;

import com.alibaba.otter.canal.protocol.Message;

import com.unigroup.kafka.producer.KafkaProperties.Topic;

import com.unigroup.utils.MessageSerializer;

/\*\*

\* @Title: CanalKafkaProducer.java

\* @Package com.unigroup.kafka.producer

\* @Description:

\* @author 桃花惜春风

\* @date 2018年9月3日 上午11:53:35

\* @version V1.0

\*/

public class CanalKafkaProducer {

```
private static final Logger logger = LoggerFactory.getLogger\(CanalKafkaProducer.class\);



private Producer&lt;String, Message&gt; producer;



public void init\(KafkaProperties kafkaProperties\) {

    Properties properties = new Properties\(\);

    properties.put\("bootstrap.servers", kafkaProperties.getServers\(\)\);

    properties.put\("acks", "all"\);

    properties.put\("retries", kafkaProperties.getRetries\(\)\);

    properties.put\("batch.size", kafkaProperties.getBatchSize\(\)\);

    properties.put\("linger.ms", kafkaProperties.getLingerMs\(\)\);

    properties.put\("buffer.memory", kafkaProperties.getBufferMemory\(\)\);

    properties.put\("key.serializer", StringSerializer.class.getName\(\)\);

    properties.put\("value.serializer", MessageSerializer.class.getName\(\)\);

    producer = new KafkaProducer&lt;String, Message&gt;\(properties\);

}



public void stop\(\) {

    try {

        logger.info\("\#\# stop the kafka producer"\);

        producer.close\(\);

    } catch \(Throwable e\) {

        logger.warn\("\#\#something goes wrong when stopping kafka producer:", e\);

    } finally {

        logger.info\("\#\# kafka producer is down."\);

    }

}



public void send\(Topic topic, Message message\) throws IOException {



    ProducerRecord&lt;String, Message&gt; record;

    if \(topic.getPartition\(\) != null\) {

        record = new ProducerRecord&lt;String, Message&gt;\(topic.getTopic\(\), topic.getPartition\(\), null, message\);

    } else {

        record = new ProducerRecord&lt;String, Message&gt;\(topic.getTopic\(\), message\);

    }

    producer.send\(record\);

    if \(logger.isDebugEnabled\(\)\) {

        logger.debug\("send message to kafka topic: {} \n {}", topic.getTopic\(\), message.toString\(\)\);

    }

}
```

}

canalToKafkaServer

package com.unigroup.kafka.server;

import com.unigroup.client.canal.SimpleCanalClient;

import com.unigroup.kafka.producer.CanalKafkaProducer;

import com.unigroup.utils.GetProperties;

/\*\*

\* @Title: canal.java

\* @Package com.unigroup.kafka.server

\* @Description:

\* @author 桃花惜春风

\* @date 2018年9月3日 上午11:23:35

\* @version V1.0

\*/

public class canalToKafkaServer {

```
public static void execute\(\) {

    SimpleCanalClient simpleCanalClient = new SimpleCanalClient\(GetProperties.getValue\("MYSQL\_HOST"\),

            GetProperties.getValue\("MTSQL\_PORT"\), GetProperties.getValue\("INSTANCE"\)\);

    try {

        simpleCanalClient.execute\(1,CanalKafkaProducer.class\);

    } catch \(Exception e\) {

        e.printStackTrace\(\);

    } 

}
```

}

