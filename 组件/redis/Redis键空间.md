# Redis的键空间通知

$\textcolor{purple}{Redis键空间通知允许客户端通过订阅某个频道,实现接收那些以某种方式改动了Redis数据集的事件}$

## 概述:

1. **Public/Subscribe**

Redis在2.0.0版本之后推出了Pub/Sub的指令,可以给Redis的特定消息发送消息,另一边Redis的特定频道取值,即是一个简单的消息队列

2. **Redis keyspace Notifications**

集合Redis的Pub/Sub指令,当Redis中触发了一些特定事件就向Redis中的Channel推送一条消息.

## 事件的类型

针对Redis数据空间的每个操作,键空间都会发送两类不通的通知事件

比如在0号数据库中,执行del key操作,将会触发两个消息,等价于执行下面两个publish命令:

```typescript
PUBLISH __keyspace@0__:mykey del
PUBLISH __keyevent@0__:del mykey
```

一个频道发布在0号数据库中,所有针对mykey的操作,这类事件,以keyspace为前缀,成为keyspace通知;

另一个频道发布在0号数据库中,针对所有del操作的键,这类事件,以keyevent为前缀,成为keyevent通知;

## 配置

因键空间通知需要消耗CPU资源,所以默认是关闭的,需要修改redis.config配置文件来或者通过CONFIG SET命令,设置notify-keyspace-events选项,来启用或者关闭该功能

配置详情

| 字符  | 意义                                      |
| --- | --------------------------------------- |
| K   | keyspace事件,事件以__keyspace@\<db>__为前缀进行发布 |
| E   | keyevent事件,事件以__keyevent@\<db>__为前缀进行发布 |
| g   | 一般性的,非特定类型的命令,比如del,expire,rename等      |
| $   | 字符串特定命令                                 |
| l   | 列表特定命令                                  |
| s   | 集合特定命令                                  |
| h   | 哈希特定命令                                  |
| z   | 有序集合特定命令                                |
| x   | 过期事件,每当有键被删除时候发送                        |
| e   | 驱逐(evict)时间,每当有键因为maxmemory政策而被删除时发送    |
| A   | 参数g$lshzxe的别名                           |

$\textcolor{red}{被输入的参数中至少要有一个K或者E,否则无论其余参数是什么,都不会有通知被发送}$



## 过期通知的发送时间

Redis使用两种方式删除过期的键

* 当一个键被访问,程序会对这个键进行检查,如果键已经过期,那么该键将被删除

* 底层系统会在后台逐渐的查找那并删除那些过期的键,从而处理那些已经过期,但是不会被访问到的键

当过期的键被上述两个流程中的任何一个发现,并且将键从数据库中删除的时候,Redis会立即产生一个expired通知.

Redis并不保证当生存周期变为0的键会被立刻删除,如果程序没有访问这个过期的键或者带有生存周期的键非常多的话那么在键的生存周期变为0直到键被真正的删除这中间,可能会有一段比较显著的时间间隔.

因此Redis产生expired通知的时间为键删除的时间,而不是生存周期变为0的时候

update 0  {"key":"AD_PAY_ORDER","itemkey":"adid","item":{"endTime":time},"cas":0}