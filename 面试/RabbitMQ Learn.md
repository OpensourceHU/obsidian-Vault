docker Compose File:
```yaml
version: "3.2"
services:
  rabbitmq:
    image: rabbitmq:3-management-alpine
    container_name: 'rabbitmq'
    ports:
        - 5672:5672
        - 15672:15672
    volumes:
        - ~/.docker-conf/rabbitmq/data/:/var/lib/rabbitmq
        - ~/.docker-conf/rabbitmq/log/:/var/log/rabbitmq
    networks:
        - rabbitmq_go_net

networks:
  rabbitmq_go_net:
    driver: bridge
```



使用方法  docker compose up

可能遇到的问题: 无法创建日志文件夹导致启动失败 参考: [rabbitmq-server fails to start when log directory does not exist · Issue #1151 · rabbitmq/rabbitmq-server (github.com)](https://github.com/rabbitmq/rabbitmq-server/issues/1151)

rabbitMQ的日志创建是起一个临时用户来创建文件夹, 可能会遇到权限问题   所以不能创建在需要root权限的路径下,自己改一个用户路径



启动后, 访问localhost的15672端口   用户名密码均为:guest   就进入了rabbitMQ的后台系统



[RabbitMQ Tutorials — RabbitMQ](https://www.rabbitmq.com/getstarted.html)  入门教程

相关的demo都在:



## 消息发送方式

总的说来其实只有两种  worker模式和publish/subscribe模式  只是publish/subscribe模式根据exchanger的不同,细分为fanout header topic三种

### worker模式

一个channel一个consumer,producer可以生产多条消息 但同一时刻只会有一个consumer消费某条消息

### publish/subscribe模式

#### fanout





## 常见的几个问题



手动确认: 

```java
// 需要把autoACK给取消
channel.basicConsume(QUEUE_NAME, false, defaultConsumer);
```

然后一定要记得  消费完消息后确认

```java
channel.basicAck(envelope.getDeliveryTag(), false);
```

