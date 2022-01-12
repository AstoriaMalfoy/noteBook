# RabbitMQ介绍

## RocketMQ的基本模型

### 1. 消息模型(Message Model)

RabbitMQ主要由`Producter`,`Broker`,`Consumer`三个部分组成,其中`Procucter`负责生产消息,`Consumer`负责消息消息,`Broker`负责存储消息.`Broker`在实际部署过程中对应一个服务器,每个`Broker`可以存储多个`Topic`的消息,每个`Topic`的消息也可以分片存储在不同的`Broker`.`Message Queue`用于存储消息的物理地址,每个`Topic`中的消息可以存储在多个`Message Queue`中.`ConsumerGroup`由多个`Consumer`实例构成.

### 2. 消息生产者(Producer)

负责生产消息,一般由业务系统负责生产消息.一个消息生产者会把业务应用系统中产生的消息发送到`broker`服务器.

RabbitMQ提供多种发送方式:

* 同步发送

* 异步发送

* 顺序发送

* 单向发送

* 
