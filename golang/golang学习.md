* 文件名 （小写 + 下划线 ）   my_test.go
* 变量区分大小写
* 不能以数字开头
* _ 本身就是一个特殊的标识符，被称为空白标识符

### 【golang interface】
* 要实现interface内的所有方法
* 方法的reciver要区分指针 还是 值
* reciver 如果是指针， 在使用时，revicer 可以是值，也可以是指针， 但都当成 指针 来处理。 
* reciver 如果是值，   在使用时，revicer 可以是值，也可以是指针， 但都当成 值 来处理。
* reciver （ var a , b:=&a,  a.test() b.test() 这两种调用方法都行 ）
* interface 的 method 实现，对于reciver，值 还是 指针 要明确。
```go
type II interface {
    R () int
}

type A struct {
}

func (a A) R() int {  // 值
}

//------ 值
var x A
var i II
i = a  // ------ 值
```

```go
type II interface {
    R () int
}

type A struct {
}

func (a *A) R() int {  // 指针
}

//------ 指针
var x A
var i II
i = &a  // ------ 指针
```

* 深入理解 Go Interface https://sanyuesha.com/2017/07/22/how-to-understand-go-interface/
* golang技术随笔（一）深入理解interface  https://studygolang.com/articles/2268
* 【golang-GUI开发】qt之signal和slot（一）https://studygolang.com/articles/13692?fr=sidebar
* Golang的Interface是个什么鬼 https://studygolang.com/articles/4482
* 如何利用golang 反射值来定义一个变量 https://studygolang.com/articles/6529
* Go Data Structures: Interfaces https://research.swtch.com/interfaces
* Go语言interface详解_Golang https://yq.aliyun.com/ziliao/99596?spm=a2c4e.11155472.blogcont.17.124f14d32S9HqU
* 鸭子类型 https://zh.wikipedia.org/wiki/%E9%B8%AD%E5%AD%90%E7%B1%BB%E5%9E%8B
* 神奇的GO语言：空接口（interface） https://www.cnblogs.com/chase-wind/p/6094378.html
``` go
神奇的GO语言：空接口（interface）
　　对于go语言来说，设计最精妙的应该是interface了，直白点说interface是一组method的组合。至于更加详细的描述，本文不做介绍，今天谈谈空接口。

　　空interface(interface{})不包含任何的method，因此所有的类型都实现了空interface。空interface在我们需要存储任意类型的数值的时候相当有用，有点类似于C语言的void*类型。请看下面的代码：

package main
import (
    "fmt"
)
func main() {
    slice := make([]interface{}, 10)
    map1 := make(map[string]string)
    map2 := make(map[string]int)
    map2["TaskID"] = 1
    map1["Command"] = "ping"
    map3 := make(map[string]map[string]string)
    map3["mapvalue"] = map1
    slice[0] = map2
    slice[1] = map1
    slice[3] = map3
    fmt.Println(slice[0])
    fmt.Println(slice[1])
    fmt.Println(slice[3])
}
这段代码声明了一个空接口的slice，这意味着它的值可以是任意类型，然后我们声明了两个map，一个是map[string]string，一个是map[string]int，然后在声明一个map的map类型，将这三个类型赋值给slice，使得slice可以存贮各种不同类型的数据，想想看，一个可变数组中，存储了一个key为string类型，value为int类型的map，又存储了一个key为string类型，value为string类型的map，还存储了一个map的map，这对c/c++转go的程序员们来说是多么让人吃惊。
```
* 深入理解Go的interface  http://www.iteye.com/news/32486

### 【观察者模式 Go语言实现】
* Just for fun——go实现一下观察者模式 https://segmentfault.com/a/1190000011972032
* 观察者模式 Go语言实现 https://studygolang.com/articles/6804
* 用GO实现的改进的观察者模式 https://studygolang.com/articles/4139
### 【go restful】
* https://github.com/emicklei/go-restful
* golang restful 框架之 go-swagger emicklei https://www.jianshu.com/p/dbca3911419e
* go restful源码剖析-1 https://www.jianshu.com/p/b905d9701aea?utm_campaign=studygolang.com&utm_medium=studygolang.com&utm_source=studygolang.com
* go restful源码剖析-1 https://studygolang.com/articles/14523 
* go-restful实战与深入分析之使用篇 https://blog.csdn.net/u010278923/article/details/70482393

### 【指针】
* Golang 中的指针 - Pointer  https://www.cnblogs.com/jasonxuli/p/6802289.html
##### struct：
 对于函数（function），由函数的参数类型指定，传入的参数的类型不对会报错。
 func(指针），只能传指针， func(值），只能传值。
 
 对于method， 
 * reciver 如果是指针， 可以传值，也可以传指针， 但都当成 指针 来处理。
 * reciver 如果是值，   可以传值，也可以传指针， 但都当成 值   来处理。
 
 ### 【断言】
 * golang断言的使用（Type Assertion） https://studygolang.com/articles/3314
 
 
 
