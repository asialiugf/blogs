### 参考
- https://www.cnblogs.com/hei12138/p/7805475.html
- https://blog.csdn.net/Mr_Hou2016/article/details/79484032

### 下载
- http://kafka.apache.org/downloads

### 安装 
直接 tar xvf 展开 在 /opt/kafka目录下

### 配置

2.3.       配置
　　在kafka解压目录下下有一个config的文件夹，里面放置的是我们的配置文件

　　consumer.properites 消费者配置，这个配置文件用于配置于2.5节中开启的消费者，此处我们使用默认的即可

　　producer.properties 生产者配置，这个配置文件用于配置于2.5节中开启的生产者，此处我们使用默认的即可

　　server.properties kafka服务器的配置，此配置文件用来配置kafka服务器，目前仅介绍几个最基础的配置

broker.id 申明当前kafka服务器在集群中的唯一ID，需配置为integer,并且集群中的每一个kafka服务器的id都应是唯一的，我们这里采用默认配置即可
listeners 申明此kafka服务器需要监听的端口号，如果是在本机上跑虚拟机运行可以不用配置本项，默认会使用localhost的地址，如果是在远程服务器上运行则必须配置，例如：
　　　　　　　　　　listeners=PLAINTEXT:// 192.168.66.253:9092。并确保服务器的9092端口能够访问

   zookeeper.connect 申明kafka所连接的zookeeper的地址 ，需配置为zookeeper的地址，由于本次使用的是kafka高版本中自带zookeeper，使用默认配置即可

　　　　　　　　　　zookeeper.connect=localhost:2181


### 运行
```js
root@develop:/opt/kafka/kafka_2.11-1.1.0/bin# ./zookeeper-server-start.sh ../config/zookeeper.properties
root@develop:/opt/kafka/kafka_2.11-1.1.0/bin# ./bin/kafka-server-start.sh ./config/server.properties
lgf@develop:/opt/kafka/kafka_2.11-1.1.0$ ./bin/kafka-console-consumer.sh --bootstrap-server PLAINTEXT://192.168.66.253:9092 --topic test --from-beginning  
lgf@develop:/opt/kafka/kafka_2.11-1.1.0$ ./bin/kafka-console-producer.sh --broker-list PLAINTEXT://192.168.66.253:9092 --topic test
```
