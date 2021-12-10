# JMH
JMH即Java Microbenchmark Harness , 是对代码进行微基准测试的一套API.而代码微基准测试就是针对方法层面进行测试,精度可以到达毫秒级别,当我们已知热点函数,需要对热点函数进行不断优化的时候,就可以使用JMH对结果进行定量的分析.

典型的应用常见还有:
* 已经知道了热点函数,需要对热点函数进行优化的时候
* 想要定量的知道某个函数需要执行多长时间,以及执行时间和输入n的相关性
* 一种函数可能有两个不同的实现,进行方案对比的时候.


# JMH使用方法
>首先需要导入pom的坐标依赖

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

