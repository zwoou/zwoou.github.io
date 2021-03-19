# ActiveMQ

[TOC]

## 1. 基本概念

1. 消息传送模型

- 点对点(Point to Point)模型
- 发布/订阅(Pub/Sub)模型

2. 基本组件

- Broker （消息代理)
- Producer(消息生产者)
- Consumer(消息消费者)
- Topic(主题)
- Queue(队列)
- Message(消息)



## 2. 持久化策略

### JDBC持久化方式



## 3. 消息转发模式

两种消息转发模式:**PERSISTENT(持久化)**和**NON_PERSISTENT(非持久化)**

默认持久化

两种设置方式,第一种使用setDeliveryMode方法 第二种在send方法里设置.

## 4. 消息事务(Transaction)

ActiveMQ支持的事务有两种

- JMS Transaction: 使用session 接口的commit和rollback控制
- XA Transaction: 



## 5. 延迟消息

1. 在配置文件添加
   vi activemq.xml # 在第40行添加 schedulerSupport="true"
   
   ```xml
     <broker xmlns="http://activemq.apache.org/schema/core" brokerName="localhost" dataDirectory="${activemq.data}" schedulerSupport="true">
   ```
   
   
   
2. 代码配置

```java
    // 获取连接工厂
    ConnectionFactory connectionFactory = jmsMessagingTemplate.getConnectionFactory();
    Connection connection = null;
    Session session =null;
    MessageProducer producer =null;
    try {
        // 获取连接
        connection = connectionFactory.createConnection();
        connection.start();
        // 获取session, true 开启事务,false关闭事务
        session = connection.createSession(Boolean.TRUE, Session.AUTO_ACKNOWLEDGE);
        // 创建一个消息队列
        producer = session.createProducer(destination);
        // 持久化消息
        producer.setDeliveryMode(JmsProperties.DeliveryMode.PERSISTENT.getValue());
        ObjectMessage message = session.createObjectMessage(data);
        //设置延迟时间
        message.setLongProperty(ScheduledMessage.AMQ_SCHEDULED_DELAY, time);
        // 发送消息
        producer.send(message);
        log.info("发送延迟消息：{}", data);
        session.commit();
    } catch (JMSException e) {
        e.printStackTrace();
    }finally {
        try {
            if (producer != null) {
                producer.close();
            }
            if (session != null) {
                session.close();
            }
            if (connection != null) {
                connection.close();
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
```

## 6. 消息重试机制

1. 连接工厂配置

   ```java
   ActiveMQConnectionFactory factory = new ActiveMQConnectionFactory(environment.getProperty("spring.activemq.broker-url"));
   factory.setRedeliveryPolicy(redeliveryPolicy());
   ```

   

2. 延迟机制 [http://activemq.apache.org/redelivery-policy.html](http://activemq.apache.org/redelivery-policy.html)

   ```java
       @Bean
       public RedeliveryPolicy redeliveryPolicy() {
           RedeliveryPolicy redeliveryPolicy = new RedeliveryPolicy();
           //是否在每次尝试重新发送失败后,增长这个等待时间
           redeliveryPolicy.setUseExponentialBackOff(true);
           //重发次数,默认为6次
           redeliveryPolicy.setMaximumRedeliveries(3);
           //重发时间间隔,默认为1秒 ！！！！
           redeliveryPolicy.setInitialRedeliveryDelay(5000);
           //第一次失败后重新发送之前等待2000毫秒,第二次失败再等待2000 * 2毫秒,这里的2就是value
           redeliveryPolicy.setBackOffMultiplier(8);
           //是否避免消息碰撞
           redeliveryPolicy.setUseCollisionAvoidance(false);
           //设置重发最大拖延时间-1 表示没有拖延只有UseExponentialBackOff(true)为true时生效
           redeliveryPolicy.setMaximumRedeliveryDelay(-1);
           return redeliveryPolicy;
       }
   ```

3. 下列情况会重发消息 [http://activemq.apache.org/message-redelivery-and-dlq-handling.html](http://activemq.apache.org/message-redelivery-and-dlq-handling.html)
   1. A transacted session is used and `rollback()` is called.
   2. A transacted session is closed before `commit()` is called.
   3. A session is using `CLIENT_ACKNOWLEDGE` and `Session.recover()` is called.
   4. A client connection times out (perhaps the code being executed takes longer than the configured time-out period).



