### spark学习
* Spark修炼之道（进阶篇）——Spark入门到精通：第四节 Spark编程模型（一)
* https://blog.csdn.net/lovehuangjiaju/article/details/48580863
* 论文
* https://www2.eecs.berkeley.edu/Pubs/TechRpts/2011/EECS-2011-82.pdf

本节内容及部分图片来自： 
http://blog.csdn.net/book_mmicky/article/details/25714419 
http://blog.csdn.net/yirenboy/article/details/47441465 
这两篇文件对Spark的运行架构原理进行了比较深入的讲解，写得非常好，建议大家认真看一下，在此向作者致敬！


### 安装模式
* https://blog.csdn.net/songhao22/article/details/79069983

Spark最主要资源管理方式按排名为Hadoop Yarn, Apache Standalone 和Mesos。在单机使用时，Spark还可以采用最基本的local模式。

Spark On Mesos模式，这是很多公司采用的模式，官方推荐这种模式（当然，原因之一是血缘关系）

###
- http://www.powerxing.com/spark-quick-start-guide/
- https://blog.csdn.net/red_stone1/article/details/71330101
- https://blog.csdn.net/microsoft2014/article/details/54572502
- https://www.jianshu.com/p/55a7fcd79f0e

### 其它
- https://www.linuxidc.com/Linux/2017-10/147220.htm
- https://blog.csdn.net/huochen1994/article/details/79395934
- http://www.aboutyun.com/thread-20808-1-1.html
- http://www.360doc.com/content/16/1209/20/29770038_613374182.shtml
- http://www.aboutyun.com/forum.php?mod=viewthread&tid=24432&fromguid=hot
- https://blog.csdn.net/jianghuxiaojin/article/details/51452593
- https://blog.csdn.net/github_33934628/article/details/73835140

博客汇总
- http://blog.51cto.com/lqding/1769311

spark的安装：

从网站上下载 jdk,scala,spark,并tar xvf 展开到相应的目录，设置以下环境变量：

##### jdk
- http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html

##### scala
- https://www.scala-lang.org/download/

##### spark
- http://spark.apache.org/downloads.html

##### 修改/etc/profile
```c
export GOPATH=/go
export GOROOT=/usr/local/go
export PATH=$PATH:$GOROOT/bin:$GOPATH/bin

#export JAVA_HOME=/usr/lib/java/jdk-10.0.1
export JAVA_HOME=/opt/java/jdk1.8.0_171
export JRE_HOME=${JAVA_HOME}/jre 
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib 
export PATH=${JAVA_HOME}/bin:${JRE_HOME}/bin:$PATH 

export SCALA_HOME=/opt/scala/scala-2.12.6
export PATH=${SCALA_HOME}/bin:$PATH 

export SPARK_HOME=/opt/spark/spark-2.3.1-bin-hadoop2.7
export PATH=${SPARK_HOME}/bin:$PATH 

export SBT_HOME=/opt/scala/sbt
export PATH=${SBT_HOME}/bin:$PATH 
```
stb是一个shell程序：
```c++
root@iZ23psatkqsZ:/bin# cat /opt/scala/sbt/bin/sbt
# SBT_OPTS="-Xms512M -Xmx1536M -Xss1M -XX:+CMSClassUnloadingEnabled -XX:MaxPermSize=256M"
# SBT_OPTS="-Xms512M -Xmx512M -Xss1M -XX:+CMSClassUnloadingEnabled -XX:MaxPermSize=256M"
#SBT_OPTS=""
java $SBT_OPTS -jar /opt/scala/sbt/bin/sbt-launch.jar "$@"
root@iZ23psatkqsZ:/bin#
```

### nc 基本使用
- https://blog.csdn.net/wangqingchuan92/article/details/79666885
```c
apt-get -y install netcat-traditional 

server:
nc.traditional -l -p 10000 -e /bin/bash 
client:
 nc.traditional 121.199.28.62 10000
 
 server:
 nc.traditional -l -p 10000
 client:
 nc.traditional 121.199.28.62 10000
```

### 解密SparkStreaming另类实验及SparkStreaming本质解析
第1课：SparkStreaming 三板斧之一：解密SparkStreaming另类实验及SparkStreaming本质解析
- https://www.cnblogs.com/tom-lee/p/5452086.html
spark自带的PageView测试用例
- https://blog.csdn.net/sinat_18497785/article/details/52311410

### 官方例子
- http://spark.apache.org/examples.html

```scala
# spark-shell
scala> val count = sc.parallelize(1 to 5).filter { _ =>
     |   val x = math.random
     |   val y = math.random
     |   x*x + y*y < 1
     | }.count()
count: Long = 4  

scala> val count = sc.parallelize(1 to 5).filter { _ =>
        val x = math.random
        val y = math.random
        x*x + y*y < 1
      }.count()




scala> println(s"Pi is roughly ${4.0 * count / 5 S}")
<console>:26: error: value S is not a member of Double
       println(s"Pi is roughly ${4.0 * count / 5 S}")
                                                 ^

scala> 

scala> println(s"Pi is roughly ${4.0 * count / 5 S}")
<console>:26: error: value S is not a member of Double
       println(s"Pi is roughly ${4.0 * count / 5 S}")
                                                 ^

scala> 

scala> println(s"Pi is roughly ${4.0 * count / 5}")
Pi is roughly 3.2

scala> 

scala> println(s"Pi is roughly ${4.0 * count / 5}")
Pi is roughly 3.2

scala> 
```

### 快速开始
- http://spark.apache.org/docs/latest/quick-start.html

### 独立模式
```js
:/opt/spark/spark-2.3.1-bin-hadoop2.7/sbin# ./start-master.sh -i 192.168.66.254
:/opt/spark/spark-2.3.1-bin-hadoop2.7/sbin# ./start-slave.sh spark://192.168.66.254:7077
:/opt/spark/spark-2.3.1-bin-hadoop2.7/sbin# spark-shell --master spark://192.168.66.254:7077
```
这时候，可以查看logs目录下的文件，看master开启的是哪个监听端口，然后在 浏览器打开 192.168.66.254:8081
```js
:/opt/spark/spark-2.3.1-bin-hadoop2.7/logs
```
#### 提交任务
官方文档：

- http://spark.apache.org/docs/latest/submitting-applications.html

```js
:/opt/spark/spark-2.3.1-bin-hadoop2.7# ./bin/spark-submit --class org.apache.spark.examples.SparkPi --master spark://192.168.66.254:7077 /opt/spark/spark-2.3.1-bin-hadoop2.7/examples/jars/spark-examples_2.11-2.3.1.jar
```
在 http://192.168.66.254:8081/# 上可以看到 相应的 master slave Executor的相关信息。

参考：

各模式下运行spark自带实例SparkPi
- https://blog.csdn.net/yt_sports/article/details/50424522

Spark-shell和Spark-Submit的使用
- https://www.cnblogs.com/zmdandy/p/6255937.html

##### 查看jobs
- http://192.168.66.254:4040/jobs/

##### 在普通用户下提交任务
```js
fileserv@:/opt/spark/spark-2.3.1-bin-hadoop2.7$ ./bin/spark-submit run-example --master spark://192.168.66.254:7077 sql.SparkSQLExample
```

###### Spark项目之电商用户行为分析大数据平台之（十二）Spark上下文构建及模拟数据生成
- https://www.cnblogs.com/qingyunzong/category/1219125.html
###### Spark学习之路 （二十八）分布式图计算系统
- https://www.cnblogs.com/qingyunzong/category/1202252.html
###### 使用Flume+Kafka+SparkStreaming进行实时日志分析
- https://blog.csdn.net/trigl/article/details/70237981
###### 【Spark五十二】Spark Streaming整合Flume-NG一
- http://bit1129.iteye.com/blog/2184467
