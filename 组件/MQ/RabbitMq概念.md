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

并且同步发送和异步发送都需要Broker返回确认消息,单项发送并不需要.

## 3. 消息消费者(Comsumer)

负责消费消息,一般是后台系统参与消息消费,一个消息消费者会通过Broker服务器拉取消息,并且其提供给应用程序,从用户角度提供了两种消费形式:

* 拉取式发送

* 推动式发送

## 4. 主题(Topoic)

表示一类消息的集合,每个主题包含若干条消息,每个消息只能属于一个主题,并且是进行消息定于的基本单位

## 5. 代理服务器(Broker Server)

消息的中转角色,负责进行$\textcolor{purple}{存储消息}$,$\textcolor{purple}{转发消息}$,代理服务器在消息系统中负责接收从生产者发送来的消息并存储,同时为消费者的拉取做准备.代理服务器也存储消息相关的元数据,包括消费者组,消息进度飘逸和主题和队列消息等.

## 6. 名字服务(Name Service)

名称服务充当路由消息的提供者,生产者或者消费者能够通过名字服务查找个主题相应的Broker IP列表,多个NameService实例组成集群,但是相互独立,中间没有信息交换

## 7. 拉取式消费(Pull Consumer)

Consumer消费的一种类型,应用通常调用Consumer的拉取方法从Broker服务器拉取下次,主动权是由应用控制,一旦获得了批量消息,应用就会启动相应消费流程

## 8. 推动式消费(Push Consumer)

Consumer消费的一种,该模式下Broker收到数据后会主动推送给消费端,该消费模式一般实时性较高.
