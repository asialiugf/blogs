ubuntu下go语言安装

https://golang.org/dl/
https://www.golangtc.com/download
https://studygolang.com/articles/2620
http://www.runoob.com/go/go-environment.html


root # wget https://www.golangtc.com/static/go/1.9.2/go1.9.2.linux-amd64.tar.gz
wget https://redirector.gvt1.com/edgedl/go/go1.9.2.src.tar.gz
 wget https://redirector.gvt1.com/edgedl/go/go1.9.2.linux-amd64.tar.gz


 查看CPU
 riddle@asialine:~$ cat /proc/cpuinfo | grep name | cut -f2 -d: | uniq -c
      8  Intel(R) Core(TM) i7-6770HQ CPU @ 2.60GHz

https://www.cnblogs.com/mafeng/p/6558941.html

```
环境变量设置如下：
$export GOPATH=$GOPATH:$HOME/gopy
$ cd
$ mkdir gopy
$ cd gopy
$ mkdir bin
$ mkdir pkg
$ mkdir src
$ cd src
$ mkdir usr
$ cd usr
$ mkdir hello
$ cd hello
$ > kkk.py
$ cat kkk.py
riddle@asiamiao:~/gopy/src/usr/hello$ cat ttt.go
package main

import "fmt"

func main() {
        fmt.Printf("Hello, world.\n")
}
riddle@asiamiao:~/gopy/src/usr/hello$

riddle@asiamiao:~/gopy$ ll
total 16
drwxrwxr-x 2 riddle riddle 4096 Nov 25 21:32 bin/
drwxrwxr-x 2 riddle riddle 4096 Nov 25 21:26 pkg/
drwxrwxr-x 3 riddle riddle 4096 Nov 25 21:30 src/
riddle@asiamiao:~/gopy$

编译如下：
-----------在 hello目录下，执行 go install
riddle@asiamiao:~/gopy$ cd src/usr/hello
riddle@asiamiao:~/gopy/src/usr/hello$ ll
total 4
-rw-rw-r-- 1 riddle riddle 75 Nov 25 21:30 ttt.go
riddle@asiamiao:~/gopy/src/usr/hello$
riddle@asiamiao:~/gopy/src/usr/hello$ go install
riddle@asiamiao:~/gopy/src/usr/hello$
riddle@asiamiao:~/gopy/src/usr/hello$ cd ../..
riddle@asiamiao:~/gopy/src$ cd ..
riddle@asiamiao:~/gopy$ cd bin
riddle@asiamiao:~/gopy/bin$
riddle@asiamiao:~/gopy/bin$ ll
total 1828
-rwxrwxr-x 1 riddle riddle 1868367 Nov 25 21:43 hello*
riddle@asiamiao:~/gopy/bin$
-------------或者在任何目录下执行go install usr/hello
riddle@asiamiao:~/gopy/bin$ go install usr/hello
riddle@asiamiao:~/gopy/bin$
riddle@asiamiao:~/gopy/bin$
riddle@asiamiao:~/gopy/bin$ cd ..
riddle@asiamiao:~/gopy$ cd ..
riddle@asiamiao:~$ go install usr/hello
riddle@asiamiao:~$
riddle@asiamiao:~$
riddle@asiamiao:~$ cd
riddle@asiamiao:~$ go install usr/hello
riddle@asiamiao:~$
riddle@asiamiao:~$
riddle@asiamiao:~$ cd gopy/pkg
riddle@asiamiao:~/gopy/pkg$ go install usr/hello
riddle@asiamiao:~/gopy/pkg$
这时候，在~/gopy/bin下，会有一个 hello 的可执行文件产生。
riddle@asiamiao:~/gopy/pkg$ cd ../bin
riddle@asiamiao:~/gopy/bin$ ll
total 1828
-rwxrwxr-x 1 riddle riddle 1868367 Nov 25 21:43 hello*
riddle@asiamiao:~/gopy/bin$ ./hello
Hello, world.
riddle@asiamiao:~/gopy/bin$
```

```
对于包的编译，也和上面一样，可以在
riddle@asiamiao:~/gopy/src/usr/util$ ll
total 4
-rw-rw-r-- 1 riddle riddle 317 Nov 25 21:51 reverse.go
riddle@asiamiao:~/gopy/src/usr/util$
riddle@asiamiao:~/gopy$
riddle@asiamiao:~/gopy$ go build usr/util
riddle@asiamiao:~/gopy$ go install usr/util
riddle@asiamiao:~/gopy$

riddle@asiamiao:~/gopy/pkg$ cd
riddle@asiamiao:~$ go build usr/util
riddle@asiamiao:~$ go install usr/util
riddle@asiamiao:~$
riddle@asiamiao:~/gopy/pkg/linux_amd64/usr$ ll
total 4
-rw-rw-r-- 1 riddle riddle 2202 Nov 25 21:58 util.a
riddle@asiamiao:~/gopy/pkg/linux_amd64/usr$

```

## 包名和目录名不同的情况：
```
riddle@asiamiao:~/gopy/src/usr/hello$ vi *
package main

import (
 "fmt"
 "usr/util"
 "usr/ccc"
)

func main() {
    fmt.Printf("Hello, world.\n")
    fmt.Printf( "%d\n", util.Add(3,4) )
    fmt.Printf( "%d\n", bbb.Add(3,4) )
}
~
注意：以上 import 的 usr/ccc 只是import的 usr/这个目录下的ccc.a这个文件。
这个文件中的包的名字，其实叫bbb，所以在main中引用的是 bbb.Add(3,4)
我们可以看到这个文件是如何生成的:
riddle@asiamiao:~/gopy/src/usr$ cd ccc
riddle@asiamiao:~/gopy/src/usr/ccc$ ll
total 4
-rw-rw-r-- 1 riddle riddle 210 Nov 25 22:12 add.go
riddle@asiamiao:~/gopy/src/usr/ccc$ cat *
// Package stringutil contains utility functions for working with strings.
package  bbb

// Reverse returns its argument string reversed rune-wise left to right.
func Add(a int, b int) int {
    return (a+b)
}
riddle@asiamiao:~/gopy/src/usr/ccc$
riddle@asiamiao:~/gopy/src/usr/ccc$
以上可以看到，在这个目录下的add.go中，包的名字叫做bbb!!!

```
####结论：
- import语句中包含的是目录名
- 包名不必和目录名一致
- import的时候要写目录名，引用的时候要写包名
- 一个src/xxx/目录下的多个.go文件，不能有两个包名


NATS之gnatcd初体验
http://www.jianshu.com/p/27429fde5911
月

####golang生成动态库
```
riddle@asiamiao:~/gopy/test$ ll
total 1388
-rwxrwxr-x 1 riddle riddle      45 Nov 26 00:30 build.sh*
-rw-rw-r-- 1 riddle riddle      99 Nov 25 21:08 main.go
drwxrwxr-x 3 riddle riddle    4096 Nov 26 00:33 pkg/
drwxrwxr-x 2 riddle riddle    4096 Nov 26 00:15 __pycache__/
-rw-rw-r-- 1 riddle riddle    1332 Nov 26 00:29 sum.h
-rw-rw-r-- 1 riddle riddle 1393600 Nov 26 00:29 sum.so
-rw-rw-r-- 1 riddle riddle      90 Nov 26 00:29 test.py
riddle@asiamiao:~/gopy/test$
riddle@asiamiao:~/gopy/test$
riddle@asiamiao:~/gopy/test$
riddle@asiamiao:~/gopy/test$
riddle@asiamiao:~/gopy/test$
riddle@asiamiao:~/gopy/test$ cat test.py
#!/usr/bin/env python
import ctypes

lib = ctypes.CDLL('./sum.so')
print (lib.Sum(7, 11))
riddle@asiamiao:~/gopy/test$
riddle@asiamiao:~/gopy/test$ cat main.go
package main

import "C"

//export Sum
func Sum(a, b int) int {
    return a + b
}

func main() {}
riddle@asiamiao:~/gopy/test$ cat build.sh
go build -v -x -buildmode=c-shared -o sum.so
riddle@asiamiao:~/gopy/test$
riddle@asiamiao:~/gopy/test$ python test.py
18
riddle@asiamiao:~/gopy/test$
```

快速教程：使用Cython来扩展Python/NumPy库
http://www.jianshu.com/p/1918e580581d
用Cython编译Python的C扩展
http://blog.csdn.net/bluebelfast/article/details/22670561



##常用包
github.com/larspensjo/config 读取ini配置文件

#### python 链接 GO生成的动态库

##### go语言：
```
riddle@asiamiao:~/gopy/test$
riddle@asiamiao:~/gopy/test$ cat main.go
package main

import "C"

//export Sum
func Sum(a, b int) int {
    return a + b
}

func main() {
}
riddle@asiamiao:~/gopy/test$
```
以上要注意：必须是 package main,并且有 func main(){} 这一名句。

##### python程序：
```
riddle@asiamiao:~/gopy/test$
riddle@asiamiao:~/gopy/test$ cat test.py
#!/usr/bin/env python
import ctypes

lib = ctypes.CDLL('./sum.so')
print (lib.Sum(7, 11))
riddle@asiamiao:~/gopy/test$
```
#####　编译方法
```
riddle@asiamiao:~/gopy/test$ cat build.sh
go build -v -x -buildmode=c-shared -o sum.so
riddle@asiamiao:~/gopy/test$
```

#### golang调用C动态库
https://studygolang.com/topics/1742
golang以插件的方式加载golang动态库
https://studygolang.com/articles/11068

#### go struct 继承
```
riddle@asiamiao:~/gopy/src/int$
riddle@asiamiao:~/gopy/src/int$ vi int.go
package main

import (
    "log"
)

type B struct{
     inner
     i int
     j int
     k int
}
type inner struct {
    ss4 string
}

func (b B) foo() {
    b.i = 1
    b.j = 2
    log.Printf("%d...",b.k)
}

func (b B) foo1() {
    b.i = 1
    b.j = 2
    b.k = 9999
    log.Printf("%d...",b.k)
}

type A struct {
    B
}
func (a A) foo() {
    a.k = 2987
    a.B.k = 3488
    a.B.foo()
    log.Printf("%d =========",a.k)
}

func main() {
    kk:=new(A)
    var a A
    var b B
    kk.k= 100
    b.k = 99
    a.foo()
    b.foo()
    b.foo1()
    a.foo1()
    kk.foo()
"int.go" 51L, 517C written
riddle@asiamiao:~/gopy/src/int$
riddle@asiamiao:~/gopy/src/int$ go build int.go
riddle@asiamiao:~/gopy/src/int$ ./int
2017/11/27 17:31:52 3488...
2017/11/27 17:31:52 3488 =========
2017/11/27 17:31:52 99...
2017/11/27 17:31:52 9999...
2017/11/27 17:31:52 9999...
2017/11/27 17:31:52 3488...
2017/11/27 17:31:52 3488 =========
riddle@asiamiao:~/gopy/src/int$
riddle@asiamiao:~/gopy/src/int$
```
#### Go语言学习笔记（四）结构体struct & 接口Interface & 反射reflect
https://www.cnblogs.com/suoning/p/7145458.html
【GoLang笔记】浅析Go语言Interface类型的语法行为及用法
https://studygolang.com/articles/2652
Go语言Interface漫谈
http://www.infoq.com/cn/articles/go-interface-talk/

## 用GO实现的改进的观察者模式
https://studygolang.com/articles/4139

【译】如何使用 Golang 中的 Go-Routines 写出高性能的代码
https://studygolang.com/articles/11734?fr=sidebar

MQTT入门篇
https://zhuanlan.zhihu.com/p/20888181
高性能，简单，方便的开源服务器网络库
https://github.com/davyxu/cellnet


测试一下
再测试一下
