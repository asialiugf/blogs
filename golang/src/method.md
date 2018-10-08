### 
```go
[GD-0103-0121@rjsys ~/go/src/tt]$ cat a002.go
package main

import "fmt"

const (
        WHITE = iota
        BLACK
        BLUE
        RED
        YELLOW
)

type Color byte

type Box struct {
        width, height, depth float64
        color                Color
}

type BoxList []Box //a slice of boxes

func (b Box) Volume() float64 {
        return b.width * b.height * b.depth
}

func (b *Box) SetColor(c Color) {
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
                bl[i].SetColor(BLACK)
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
