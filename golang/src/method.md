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
