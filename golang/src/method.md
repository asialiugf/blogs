### Go语言method详解_Golang
* https://yq.aliyun.com/ziliao/99597
```go
[GD-0103-0121@rjsys ~/go/src/tt]$ cat a002.go
package main

import "fmt"

// 定义一堆常量，按顺序为 0， 1， 2， 3，4 （=iota)的功能
const (
        WHITE = iota
        BLACK
        BLUE
        RED
        YELLOW
)

type Color byte

type Box struct { // 定义一个Box 类型
        width, height, depth float64
        color                Color
}

type BoxList []Box //a slice of boxes

func (b Box) Volume() float64 { // 给Box 填加方法
        return b.width * b.height * b.depth
}

/*
下面
func (b *Box) SetColor(c Color) {}
这里的 (b *Box) 是 receiver，用指针，
现在让我们回过头来看看SetColor这个method，它的receiver是一个指向Box的指针，是的，你可以使用*Box。想想为啥要使用指针而不是Box本身呢？

我们定义SetColor的真正目的是想改变这个Box的颜色，如果不传Box的指针，那么SetColor接受的其实是Box的一个copy，也就是说method内对于颜色值的修改，其实只作用于Box的copy，而不是真正的Box。所以我们需要传入指针。

这里可以把receiver当作method的第一个参数来看，然后结合前面函数讲解的传值和传引用就不难理解


*/

func (b *Box) SetColor(c Color) { // 给Box 填加方法
        b.color = c
}

func (bl BoxList) BiggestColor() Color {
        v := 0.00
        k := Color(WHITE)
        k = Color(190)
        for _, b := range bl {
                if bv := b.Volume(); bv > v {
                        v = bv
                        k = b.color
                }
        }
        return k
}

func (bl BoxList) PaintItBlack() {
        for i, _ := range bl {
                bl[i].SetColor(BLACK)  // 这里 bl[i] 是指针？
        }
}

func (c Color) String() string {
        strings := []string{"WHITE", "BLACK", "BLUE", "RED", "YELLOW"} // 定义字符串数组
        return strings[c]
}

type IN int

func (i IN) echoo() {
    var nn int = int(i)
    fmt.Println(" I am a IN int!!")
    fmt.Printf(" I am a IN int!! %d\n", i)
    fmt.Printf(" I am a IN int!! %d\n", int(i))
    fmt.Printf(" I am a IN int!! %d\n", nn)
}


func main() {
        boxes := BoxList{
                Box{4, 4, 4, RED},
                Box{10, 10, 1, YELLOW},
                Box{1, 1, 20, BLACK},
                Box{10, 10, 1, BLUE},
                Box{10, 30, 1, WHITE},
                Box{20, 20, 20, YELLOW},
        }

        fmt.Printf("We have %d boxes in our set\n", len(boxes))
        fmt.Println("The volume of the first one is", boxes[0].Volume(), "cm³")
        fmt.Println("The color of the last one is", boxes[len(boxes)-1].color.String())
        fmt.Println("The biggest one is", boxes.BiggestColor().String())

        fmt.Println("Let's paint them all black")
        boxes.PaintItBlack()
        fmt.Println("The color of the second one is", boxes[1].color.String())

        fmt.Println("Obviously, now, the biggest one is", boxes.BiggestColor().String())

        var a IN
        a = IN(5)
        a.echoo()
}
[GD-0103-0121@rjsys ~/go/src/tt]$
```

### 自己定义的类型IN （type IN int）也可以绑定方法
```go
type IN int

func (i IN) echoo() {
        fmt.Println(" I am a IN int!!")
}

```
```go
        var a IN
        a = IN(5)
        a.echoo()
```

### 指针作为receiver

指针作为receiver

现在让我们回过头来看看SetColor这个method，它的receiver是一个指向Box的指针，是的，你可以使用*Box。想想为啥要使用指针而不是Box本身呢？

我们定义SetColor的真正目的是想改变这个Box的颜色，如果不传Box的指针，那么SetColor接受的其实是Box的一个copy，也就是说method内对于颜色值的修改，其实只作用于Box的copy，而不是真正的Box。所以我们需要传入指针。

这里可以把receiver当作method的第一个参数来看，然后结合前面函数讲解的传值和传引用就不难理解

这里你也许会问了那SetColor函数里面应该这样定义*b.Color=c,而不是b.Color=c,因为我们需要读取到指针相应的值。

你是对的，其实Go里面这两种方式都是正确的，当你用指针去访问相应的字段时(虽然指针没有任何的字段)，Go知道你要通过指针去获取这个值，看到了吧，Go的设计是不是越来越吸引你了。

也许细心的读者会问这样的问题，PaintItBlack里面调用SetColor的时候是不是应该写成(&bl[i]).SetColor(BLACK)，因为SetColor的receiver是*Box，而不是Box。

你又说对的，这两种方式都可以，因为Go知道receiver是指针，他自动帮你转了。

也就是说：

如果一个method的receiver是*T,你可以在一个T类型的实例变量V上面调用这个method，而不需要&V去调用这个method

类似的

如果一个method的receiver是T，你可以在一个*T类型的变量P上面调用这个method，而不需要 *P去调用这个method

所以，你不用担心你是调用的指针的method还是不是指针的method，Go知道你要做的一切，这对于有多年C/C++编程经验的同学来说，真是解决了一个很大的痛苦。



### method继承

前面一章我们学习了字段的继承，那么你也会发现Go的一个神奇之处，method也是可以继承的。如果匿名字段实现了一个method，那么包含这个匿名字段的struct也能调用该method。让我们来看下面这个例子:

复制代码 代码如下:
```go
package main
import "fmt"
type Human struct {
    name string
    age int
    phone string
}

type Student struct {
    Human //匿名字段
    school string
}

type Employee struct {
    Human //匿名字段
    company string
}

//在human上面定义了一个method
func (h *Human) SayHi() {
    fmt.Printf("Hi, I am %s you can call me on %s\n", h.name, h.phone)
}

func main() {
    mark := Student{Human{"Mark", 25, "222-222-YYYY"}, "MIT"}
    sam := Employee{Human{"Sam", 45, "111-888-XXXX"}, "Golang Inc"}

    mark.SayHi()
    sam.SayHi()
}
```
### method重写

上面的例子中，如果Employee想要实现自己的SayHi,怎么办？简单，和匿名字段冲突一样的道理，我们可以在Employee上面定义一个method，重写了匿名字段的方法。请看下面的例子

复制代码 代码如下:
```go
package main
import "fmt"
type Human struct {
    name string
    age int
    phone string
}

type Student struct {
    Human //匿名字段
    school string
}

type Employee struct {
    Human //匿名字段
    company string
}

//Human定义method
func (h *Human) SayHi() {
    fmt.Printf("Hi, I am %s you can call me on %s\n", h.name, h.phone)
}

//Employee的method重写Human的method
func (e *Employee) SayHi() {
    fmt.Printf("Hi, I am %s, I work at %s. Call me on %s\n", e.name,
        e.company, e.phone) //Yes you can split into 2 lines here.
}

func main() {
    mark := Student{Human{"Mark", 25, "222-222-YYYY"}, "MIT"}
    sam := Employee{Human{"Sam", 45, "111-888-XXXX"}, "Golang Inc"}

    mark.SayHi()
    sam.SayHi()
}
```

上面的代码设计的是如此的美妙，让人不自觉的为Go的设计惊叹！
通过这些内容，我们可以设计出基本的面向对象的程序了，但是Go里面的面向对象是如此的简单，没有任何的私有、公有关键字，通过大小写来实现(大写开头的为公有，小写开头的为私有)，方法也同样适用这个原则。

以上是云栖社区小编为您精心准备的的内容，在云栖社区的博客、问答、公众号、人物、课程等栏目也有的相关内容，欢迎继续使用右上角搜索按钮进行搜索go语言 method go语言和golang、golang go语言、go语言为什么叫golang、golang method、golang methodbyname，以便于您获取更多的相关知识。




