# RabbitMQ相关概念

![](https://raw.githubusercontent.com/AstoriaMalfoy/ImageRepository/main/2022/01/13-14-44-25-part1.rabbitMQ_rabbitMQ%E6%95%B4%E4%BD%93%E6%A8%A1%E5%9E%8B.png)

RabbitMQ整体上来说是一个生产者-消费者模型,主要负责接收,存储和转发消息,从计算机术语层面来说,RabbitMQ更像是一个交换机模型

**$\textcolor{blue}{Producer:生产者,就是投递消费的一方}$**

生产者生产消息并投递到RabbitMQ中.消息一般包含两个部分:$\textcolor{red}{消息体(payload)}$和$\textcolor{red}{标签(Label)}$.消息体中携带的是业务数据,而消息的标签主要是用来描述这条消息,例如交换器名称和路由键名称.生产者将消息交给RabbitMQ.RabbitMQ之后会根据标签将消息发送给感兴趣的消费者.

**$\textcolor{blue}{Consumer:消费者,就是接消息的一方}$**

消费者连接到RabbitMQ服务器,并订阅到队列上.消费消息实际上是消费消息的消息体(payload).消息在路由的过程中,消息的标签会被丢弃,并且存入到消息队列中的消息也只包含消息体,也就是说消费者不知道消息的生产者是谁,也不需要知道.

**$\textcolor{blue}{Broker:消息中间件的服务节点}$**

对于RabbitMQ来说,一个RabbitMQ Broker可以简单的看做一个RabbitMQ服务节点.
