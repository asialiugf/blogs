atom go语言
打造atom成为golang开发神器
http://blog.csdn.net/sweetvvck/article/details/50333327

Golang下载
https://www.golangtc.com/download
Go语言圣经（中文版）
http://books.studygolang.com/gopl-zh/
http://www.gopl.io/

Golang号称高并发，但高并发时性能不高
https://zhuanlan.zhihu.com/p/21514693
go语言开发证券实时行情转码接口（多个坑)
https://studygolang.com/articles/4415
nats消息服务
http://nats.io/documentation/
Go凭什么击败C++成为证券期货行情系统的首选语言
https://studygolang.com/articles/10587?fr=sidebar
GO语言的开源库
https://www.cnblogs.com/chu888chu888/p/4293711.html
一个有关Golang变量作用域的坑
http://tonybai.com/2015/01/13/a-hole-about-variable-scope-in-golang/
我们如何用Go来处理每分钟100万复杂请求的场景
https://studygolang.com/articles/8621

Go语言使用sort包对任意类型元素的集合进行排序的方法
http://www.jb51.net/article/60893.htm

Go语言异步服务器框架原理和实现
https://www.cnblogs.com/niniwzw/archive/2013/08/05/3238225.html

##### Golang-TCP异步框架Tao分析
https://studygolang.com/articles/10909
```
Go语言可以对每一个网络连接创建三个goroutine。

1. readLoop()负责读取数据并反序列化成消息。
2. writeLoop()负责序列化消息并发送二进制字节流。
3. handleLoop()负责调用消息处理函数。
```
Go语言中异步拆分io.Reader
https://studygolang.com/articles/7864
Go语言构建千万级在线的高并发消息推送系统实践(来自360公司)
http://www.52im.net/thread-848-1-1.html


##### NATS安装
一种开源的分布式消息系统Nats
http://blog.csdn.net/chszs/article/details/50996484

NATS连线协议详解
http://blog.csdn.net/chszs/article/details/50997449

Go语言下使用 nats 消息机制
http://shanshanpt.github.io/2016/05/05/go-nats.html


在linux普通用户下：
- go get github.com/nats-io/gnatsd
- 配置export GOPATH=$HOME/go
- cd ~/go/src/github.com/nats-io/gnatsd
- go build

在~/go/src/github.com/nats-io/gnatsd目录下，产生一个gnatsd的可执行文件。
在~/go/src/github.com/nats-io/gnatsd目录下执行
go test ./...
进行测试。

### NATS之gnatcd初体验
http://www.jianshu.com/p/27429fde5911
```
riddle@asiamiao:~/gopy/src/github.com/nats-io/nats$ ll
total 220
drwxrwxr-x 2 riddle riddle  4096 Nov 26 12:17 bench/
-rw-rw-r-- 1 riddle riddle  3551 Nov 26 12:17 context.go
-rw-rw-r-- 1 riddle riddle  7374 Nov 26 12:17 enc.go
drwxrwxr-x 4 riddle riddle  4096 Nov 26 12:17 encoders/
-rw-rw-r-- 1 riddle riddle  6753 Nov 26 12:17 enc_test.go
drwxrwxr-x 2 riddle riddle  4096 Nov 26 20:59 examples/
-rw-rw-r-- 1 riddle riddle  6053 Nov 26 12:17 example_test.go
-rw-rw-r-- 1 riddle riddle  1083 Nov 26 12:17 LICENSE
-rw-rw-r-- 1 riddle riddle 75686 Nov 26 12:17 nats.go
-rw-rw-r-- 1 riddle riddle 35113 Nov 26 12:17 nats_test.go
-rw-rw-r-- 1 riddle riddle  3114 Nov 26 12:17 netchan.go
-rw-rw-r-- 1 riddle riddle  9314 Nov 26 12:17 parser.go
drwxrwxr-x 2 riddle riddle  4096 Nov 26 13:07 pub/
-rw-rw-r-- 1 riddle riddle 10136 Nov 26 12:17 README.md
drwxrwxr-x 2 riddle riddle  4096 Nov 26 12:17 scripts/
-rw-rw-r-- 1 riddle riddle   187 Nov 26 12:17 staticcheck.ignore
drwxrwxr-x 2 riddle riddle  4096 Nov 26 13:05 sub/
drwxrwxr-x 3 riddle riddle  4096 Nov 26 12:17 test/
-rw-rw-r-- 1 riddle riddle   882 Nov 26 12:17 timer.go
-rw-rw-r-- 1 riddle riddle   393 Nov 26 12:17 timer_test.go
-rw-rw-r-- 1 riddle riddle   946 Nov 26 12:17 TODO.md
drwxrwxr-x 2 riddle riddle  4096 Nov 26 12:17 util/
riddle@asiamiao:~/gopy/src/github.com/nats-io/nats$
riddle@asiamiao:~/gopy/src/github.com/nats-io/nats$ go build

riddle@asiamiao:~/gopy/src/github.com/nats-io/nats$

这样服务器就正常运行了。

下面在开两个终端，我们用Golang的客户端nats（https://github.com/nats-io/nats.git）来做个pub-sub的例子。首先`go get github.com/nats-io/nats` 进行安装。

然后分别建立两个目录：

mkdir pub sub
接着分别将https://github.com/nats-io/examples/nats-pub.go复制到pub目录，https://github.com/nats-io/examples/nats-sub.go复制到sub目录。然后修改这两个文件，将最开始的：

// +build ignore
删除后，分别在两个目录下执行

go build
现在客户端就建立好了，这个时候先在sub目录下订阅一条消息：

./sub  -s nats://localhost:4222 -t abc
Listening on [abc]
然后再在pub下面进行发布：

$./pub  -s nats://localhost:4222 abc msg_abc
Published [abc] : 'msg_abc'
此时，回望sub那边：

2016/04/24 14:27:53 [#1] Received on [abc]: 'msg_abc'
这样也就完成了一次消息的发布和订阅了。

作者：CZ_Golang
链接：http://www.jianshu.com/p/27429fde5911
來源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

-----------
在examples目录下，执行 go build
会产生一个examples的可执行文件

riddle@asiamiao:~/gopy/src/github.com/nats-io/nats/examples$ ./examples
Usage: nats-bench [-s server (nats://localhost:4222)] [--tls] [-np NUM_PUBLISHERS] [-ns NUM_SUBSCRIBERS] [-n NUM_MSGS] [-ms MESSAGE_SIZE] [-csv csvfile] <subject>
riddle@asiamiao:~/gopy/src/github.com/nats-io/nats/examples$
riddle@asiamiao:~/gopy/src/github.com/nats-io/nats/examples$
riddle@asiamiao:~/gopy/src/github.com/nats-io/nats/examples$
riddle@asiamiao:~/gopy/src/github.com/nats-io/nats/examples$ ./examples -s nats://localhost:4222  -np 2 -n 1000 -ms 500 abc
Starting benchmark [msgs=1000, msgsize=500, pubs=2, subs=0]
Pub stats: 117,206 msgs/sec ~ 55.89 MB/sec
 [1] 66,929 msgs/sec ~ 31.91 MB/sec (500 msgs)
 [2] 100,063 msgs/sec ~ 47.71 MB/sec (500 msgs)
 min 66,929 | avg 83,496 | max 100,063 | stddev 16,567 msgs
riddle@asiamiao:~/gopy/src/github.com/nats-io/nats/examples$ ./examples -s nats://localhost:4222  -np 2 -n 100 -ms 500 abc
Starting benchmark [msgs=100, msgsize=500, pubs=2, subs=0]
Pub stats: 24,790 msgs/sec ~ 11.82 MB/sec
 [1] 22,548 msgs/sec ~ 10.75 MB/sec (50 msgs)
 [2] 31,077 msgs/sec ~ 14.82 MB/sec (50 msgs)
 min 22,548 | avg 26,812 | max 31,077 | stddev 4,264 msgs
riddle@asiamiao:~/gopy/src/github.com/nats-io/nats/examples$ ./examples -s nats://localhost:4222  -np 2 -n 10 -ms 500 abc
Starting benchmark [msgs=10, msgsize=500, pubs=2, subs=0]
Pub stats: 7,603 msgs/sec ~ 3.63 MB/sec
 [1] 7,815 msgs/sec ~ 3.73 MB/sec (5 msgs)
 [2] 12,732 msgs/sec ~ 6.07 MB/sec (5 msgs)
 min 7,815 | avg 10,273 | max 12,732 | stddev 2,458 msgs
riddle@asiamiao:~/gopy/src/github.com/nats-io/nats/examples$


```

### python-nats client
```
riddle@asiamiao:~/gopy/src/github.com/nats-io/asyncio-nats/examples$
riddle@asiamiao:~/gopy/src/github.com/nats-io/asyncio-nats/examples$ python client.py
[Received on 'discover']: hello
[Received on 'discover']: world
[Request on 'help _INBOX.69791186444c2749c5551881']: help please
[Response]: b'I can help!'
[Duration]: 0:00:00.007054


Disconnected.
riddle@asiamiao:~/gopy/src/github.com/nats-io/asyncio-nats/examples$
```



### gnatsd操作如下：

The gnatsd binary can be used to send these signals to running NATS servers using the -sl flag:

// Reload server configuration
gnatsd -sl reload

// Reopen log file for log rotation
gnatsd -sl reopen

// Stop the server
gnatsd -sl stop
If there are multiple gnatsd processes running, specify a PID:

gnatsd -sl stop=<pid>

怎么学习golang
https://www.zhihu.com/question/23486344
https://github.com/Unknwon/the-way-to-go_ZH_CN/blob/master/eBook/directory.md
坑
http://devs.cloudimmunity.com/gotchas-and-common-mistakes-in-go-golang/index.html#opening_braces

Golang中goroutine的调度器详解
https://studygolang.com/articles/10115
如何使用 GRPC+HTTP 打造微服务
https://studygolang.com/articles/11708

beego
Hugo
http://www.gohugo.org/post/

封装golang websocket
https://studygolang.com/articles/11698

初生牛犊不怕虎 golang入坑系列
https://studygolang.com/articles/11673
golang错题集
https://studygolang.com/articles/11632
https://i6448038.github.io/2017/10/14/golang-mistakes/?hmsr=studygolang.com&utm_medium=studygolang.com&utm_source=studygolang.com

golang精华资源
https://studygolang.com/articles/11672
可视化学习Go并发编程
https://studygolang.com/articles/11613

golang常见的几种并发模型框架
http://blog.csdn.net/smartfox80/article/details/77603481

服务化实战之 dubbo、dubbox、motan、thrift、grpc等RPC框架比较及选型
http://blog.csdn.net/liubenlong007/article/details/54692241
如何实现支持数亿用户的长连消息系统 | Golang高并发案例
https://studygolang.com/articles/5076
http://blog.sina.com.cn/s/blog_9be3b8f10101c1o0.html

非侵入式接口 代码重用

性能优化实战：百万级WebSockets和Go语言
https://segmentfault.com/a/1190000011162605
wewbsocket
https://godoc.org/github.com/gorilla/websocket
iris

非侵入式接口

Go语言异步服务器框架原理和实现
https://www.cnblogs.com/niniwzw/archive/2013/08/05/3238225.html

golang中怎么处理socket长连接？
https://www.zhihu.com/question/22925358

##### Go 语言包管理
https://gopm.io/
https://godoc.org/
https://golang.org/pkg/


https://github.com/alibaba

##### ****go-量化
https://github.com/qerio
https://github.com/shaoguang123/go-ctp/tree/master/go-ctp
https://github.com/GONGCHEN1003/Golang-CTP


##### ****NATS pub sub  goroutine执行问题

```go
riddle@asiamiao:~/gopy/src/github.com/nats-io/nats/pub$ cat nats-pub.go
// Copyright 2012-2016 Apcera Inc. All rights reserved.

package main

import (
	"flag"
	"fmt"
	"github.com/nats-io/go-nats"
	"log"
	"runtime"
	"time"
)

// NOTE: Use tls scheme for TLS, e.g. nats-pub -s tls://demo.nats.io:4443 foo hello

func printMsg(m *nats.Msg, i int) {
	log.Printf("[#%d] ----------------   fuck!! -----------  Received on [%s]: '%s'\n", i, m.Subject, string(m.Data))
}

func usage() {
	log.Fatalf("Usage: nats-pub [-s server (%s)] <subject> <msg> \n", nats.DefaultURL)
}

func pubb(subj string, msg []byte, nc *nats.Conn) {
	log.Printf(" ---------------fuck!   eceived on")
	go fmt.Printf("%s\n", subj)
	nc.Publish(subj, msg)
	nc.Flush()

	if err := nc.LastError(); err != nil {
		log.Fatal(err)
	} else {
		log.Printf("Published [%s] : '%s'\n", subj, msg)
	}
	time.Sleep(10)
}

func test() {
	log.Printf("Published------------------------- \n")
}

func main() {
	runtime.GOMAXPROCS(2) //设置执行的线程CPU数量
	var urls = flag.String("s", nats.DefaultURL, "The nats server URLs (separated by comma)")

	//log.SetFlags(0)
	flag.Usage = usage
	flag.Parse()

	args := flag.Args()
	if len(args) < 2 {
		usage()
	}

	nc, err := nats.Connect(*urls)
	if err != nil {
		log.Fatal(err)
	}
	defer nc.Close()

	subj, msg := args[0], []byte(args[1])
	//----------------------
	i := 0
	go nc.Subscribe(subj, func(msg *nats.Msg) {
		i += 1
		printMsg(msg, i)
	})
	nc.Flush()
	time.Sleep(10)
	//----------------------

	go nc.Publish(subj, msg)
	go nc.Flush()

	pubb(subj, msg, nc)
	// go test()

	x := runtime.NumGoroutine()
	log.Printf("runtime.NumGoroutine(): '%d'\n", x)

	//nc.Publish(subj, msg)
	//nc.Flush()

	if err := nc.LastError(); err != nil {
		log.Fatal(err)
	} else {
		log.Printf("Published [%s] : '%s'\n", subj, msg)
	}

	time.Sleep(10000000000) //还需要主进程待一会儿，否则主进行先结束了，协程就不会被执行了。
	log.Printf("---- after time.Sleep()")
	for {
	}
}

riddle@asiamiao:~/gopy/src/github.com/nats-io/nats/pub$
```
修改了nats的pub.go，使其即能pub，也能sub，并且，他们用的是同一个TCP连接。
```
```
