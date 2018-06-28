###
- http://www.powerxing.com/spark-quick-start-guide/
- https://blog.csdn.net/red_stone1/article/details/71330101
- https://blog.csdn.net/microsoft2014/article/details/54572502

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
