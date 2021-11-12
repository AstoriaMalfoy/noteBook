# Apache Log4j 

&nbsp;&nbsp;&nbsp;&nbsp; apache log4j 组件是目前最常用的日志组件,其具有出色的性能和灵活的配置以及丰富的功能,并且在业务有特殊需求时候还可以使用自定义的组件来代替框架中的组件

## log4l组件介绍:

log4j 主要有以下三个组件
* Logger : 日志记录器,负责记录日志信息,实际在代码中调用,与代码耦合.
* Appender : 日志输出器,负责将日志信息写入不同的位置,例如控制台,文件,远程主机等.
* Layout : 日志格式化, 主要负责日志信息的格式化
  
一个logger可以对应多个appender,一个appender只能对应一个layout

## log4j各个组件之间的调用关系
1: 客户端将日志信息通过logger传入log4j中

2: 将日志信息封装成 LoggingEvent 对象并且传入 Appender .

3: 在Appender中调用 Filter 对日志信息进行过滤,调用Layout对日志信息进行格式化,然后进行输出.



## 实现自定义Appender
Appender的生命周期

Appender 的基类: `AppenderSkeleton`

该类是一个抽象类

``` java
public abstract class AppenderSkeleton implements Appender, OptionHandler {
    protected abstract void append(LoggingEvent var1);
}
```

其中抽象方法append为打印日志的核心方法,另外如果继承AppenderSkeleton还需要实现`close()`方法和`requiresLayout()`方法.

* ` append() ` :    打印日志的核心方法
* ` activateOptions() ` : 初始化资源加载 
* ` close() ` : 释放资源
* `requiresLayout()` : 是否需要按格式输出文本
  


