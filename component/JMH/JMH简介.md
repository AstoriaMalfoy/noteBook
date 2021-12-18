# JMH

JMH即Java Microbenchmark Harness , 是对代码进行微基准测试的一套API.而代码微基准测试就是针对方法层面进行测试,精度可以到达毫秒级别,当我们已知热点函数,需要对热点函数进行不断优化的时候,就可以使用JMH对结果进行定量的分析.

典型的应用常见还有:

* 已经知道了热点函数,需要对热点函数进行优化的时候
* 想要定量的知道某个函数需要执行多长时间,以及执行时间和输入n的相关性
* 一种函数可能有两个不同的实现,进行方案对比的时候.

# JMH使用方法

> 首先需要导入pom的坐标依赖

```xml
<properties>
        <jmh.version>1.21</jmh.version>
    </properties>


    <dependencies>
        <!-- JMH-->
        <dependency>
            <groupId>org.openjdk.jmh</groupId>
            <artifactId>jmh-core</artifactId>
            <version>${jmh.version}</version>
        </dependency>
        <dependency>
            <groupId>org.openjdk.jmh</groupId>
            <artifactId>jmh-generator-annprocess</artifactId>
            <version>${jmh.version}</version>
            <scope>provided</scope>
        </dependency>
    </dependencies>
```

# 注解说明

## @BenchmarkMode

基准测试类型

* Throughput : 整体吞吐量,例如 "1秒内能执行多少次调用"
* AverageTime : 调用的平均时间,例如"每次调用平均耗时XXX毫秒"
* SampleTime : 随机取样, 最终输出取样的结果,例如 "99%的调用在XXX毫秒内,99.99%的调用在XXX毫秒内"
* SingleShotTime : 以上三种默认一次的iterator是1s,唯有SingleShotTime是只运行一次.往往把warmUp设置为0,用于测试冷启动时的性能.
* ALL : 所有模式

## @Warmup

预热配置

在进行测试之前一般都需要进行预热,因为JVM中JIT机制的存在,当函数被多次调用之后,JVM会尝试将其编译为机器码运行从而提高运行速度

* iterations : 预热的次数
* time : 每次预热的时间
* timeUnit : 时间的单位 , 默认为秒
* batchSize : 批处理大小,每次调用几次方法

## @Measurement

量度配置

配置一些基本的测试参数

* iterations : 进行测试的轮次
* time : 每轮测试的时长
* timeUnit : 时长单位

## @Threads

每个进程中的测试现成,可以用在类或者方法上,一般选择为CPU*2 如果配置了Threads.max 则使用Runtime.getRuntime().availableProcessors()个线程

## @Fork

进行fork的次数,可以用在来或者方法上,如果fork数量是2的话,则Jmh会fork出两个进程进行测试

## @OutputTimeUnit

基准测试的时间类型,一般选择秒,毫秒,微秒

## @Benchmark

方法级别注释,表示该方法是需要进行benchmark的对象,用法和Junit的@Test类似

## @Param

参数级别注释,@Param可以用来指定某项参数的多种情况,特别适合用来测试一个函数在不同参数输入的情况下的性能.

## @Setup

方法级别注解,这个注解的作用就是在进行测试之前进行一些准备工作,比如对数据进行一些初始化之类的

## @TearDowm

方法级别注解,这个注解的作用就是在测试之后进行一些结束工作,比如关闭线程池,数据库链接等,主要用于资源的回收

> @SetUp主要实现测试之前的初始化工作,只能用在方法上,用法和Junit一样,使用该注解的时候必须定义@State注解

> @TearDown 主要实现测试完成之后的垃圾回收工作,只能用在方法上,用法和Junit一样,使用该注解必须定义@State注解

这两个注解都有一个Level的枚举值,有三个值,默认是Trial

* Trial : 在每次BenchMark的前后执行
* Iteration : 在每次BenchMark的iteration的前后执行
* Invocation : 每次调用BenchMark标记的方法前后都会执行

## @State

当使用@SetUp参数的时候,必须在类上加上这个参数,不然会提示无法运行

state用于声明某个类是一个"状态",然后接收一个Scope参数用来表示该状态的共享范围.因为BenchMark会需要一些表示状态的类,JMH
