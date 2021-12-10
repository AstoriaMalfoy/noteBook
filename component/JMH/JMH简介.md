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




