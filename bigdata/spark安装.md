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

博客汇总
- http://blog.51cto.com/lqding/1769311

spark的安装：

从网站上下载 jdk,scala,spark,并tar xvf 展开到相应的目录，设置以下环境变量：
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
