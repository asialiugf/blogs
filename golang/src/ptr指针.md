### 结论
* 普通函数
```go
func printFuncValue(p MyPoint) {}
func printFuncPointer(pp *MyPoint) {}
```
* 对于普通函数，参数是值，则只能传值进去。 值传递， 不改变 结构内部数据。
* 对于普通函数，参数是指针，则只能传指针进去。 指针传递， 会改变 结构内部数据。

```go
func (p MyPoint) printMethodValue() {}
func (p *MyPoint) printMethodPointer() {}
```
* 对于方法，参数是值，则只能传值进去。 值传递， 不改变 结构内部数据。
* 对于方法，参数是指针，则只能传指针进去。 指针传递， 会改变 结构内部数据。



```go
[GD-0103-0121@rjsys ~/go/src/ptr]$ cat a.go
package main

import "fmt"

type MyPoint struct {
        X int
        Y int
}

func printFuncValue(p MyPoint) {
        p.X = 1
        p.Y = 1
        fmt.Printf(" -> %v", p)
}

func printFuncPointer(pp *MyPoint) {
        pp.X = 1 // 实际上应该写做 (*pp).X，Golang 给了语法糖，减少了麻烦，但是也导致了 * 的不一致
        pp.Y = 1
        fmt.Printf(" -> %v", pp)
}

func (p MyPoint) printMethodValue() {
        p.X += 1
        p.Y += 1
        fmt.Printf(" -> %v", p)
}

// 建议使用指针作为方法（method：printMethodPointer）的接收者（receiver：*MyPoint），一是可以修改接收者的值，二是可以避免大对象的复制
func (p *MyPoint) printMethodPointer() {
        p.X += 1
        p.Y += 1
        fmt.Printf(" -> %v", p)
}

func (i int) kaka1() int {
        x := i + 1
        return int(x)
}

type UU int

func (i UU) kaka1() int {
        x := i + 1
        return int(x)
}
func (i *UU) kaka() int {
        var x int
        x = int(*i)
        return x
}

func main() {
        p := MyPoint{0, 0}
        pp := &MyPoint{0, 0}

        fmt.Printf("\n value to func(value): %v", p)
        printFuncValue(p)
        fmt.Printf(" --> %v", p)
        // Output: value to func(value): {0 0} -> {1 1} --> {0 0}
        //printFuncValue(pp) // cannot use pp (type *MyPoint) as type MyPoint in argument to printFuncValue
        //printFuncPointer(p) // cannot use p (type MyPoint) as type *MyPoint in argument to printFuncPointer

        fmt.Printf("\n pointer to func(pointer): %v", pp)
        printFuncPointer(pp)
        fmt.Printf(" --> %v", pp)
        // Output: pointer to func(pointer): &{0 0} -> &{1 1} --> &{1 1}

        fmt.Printf("\n value to method(value): %v", p)
        p.printMethodValue()
        fmt.Printf(" --> %v", p)
        // Output: value to method(value): {0 0} -> {1 1} --> {0 0}

        fmt.Printf("\n value to method(pointer): %v", p)
        p.printMethodPointer()
        fmt.Printf(" --> %v", p)
        // Output: value to method(pointer): {0 0} -> &{1 1} --> {1 1}

        fmt.Printf("\n pointer to method(value): %v", pp)
        pp.printMethodValue()
        fmt.Printf(" --> %v", pp)
        // Output: pointer to method(value): &{1 1} -> {2 2} --> &{1 1}

        fmt.Printf("\n pointer to method(pointer): %v", pp)
        pp.printMethodPointer()
        fmt.Printf(" --> %v", pp)
        // Output: pointer to method(pointer): &{1 1} -> &{2 2} --> &{2 2}

        p = MyPoint{0, 0}
        yy := p
        zz := &yy

        fmt.Println(" ")
        fmt.Printf(" yy --> %v\n", yy)
        fmt.Printf(" zz --> %v\n", zz)
        yy.printMethodValue()
        yy.printMethodValue()
        yy.printMethodValue()
        yy.printMethodValue()
        fmt.Println(" ")
        zz.printMethodValue()
        zz.printMethodValue()
        zz.printMethodValue()
        zz.printMethodValue()
        zz.printMethodValue()
        zz.printMethodValue()
        zz.printMethodValue()
        fmt.Println(" ")
        fmt.Printf("after value yy --> %v\n", yy)
        fmt.Printf("after value zz --> %v\n", zz)
        fmt.Println(" ")
        fmt.Printf(" yy --> %v\n", yy)
        fmt.Printf(" zz --> %v\n", zz)
        yy.printMethodPointer()
        yy.printMethodPointer()
        yy.printMethodPointer()
        yy.printMethodPointer()
        fmt.Println(" ")
        fmt.Printf(" yy --> %v\n", yy)
        zz.printMethodPointer()
        fmt.Println(" ")
        fmt.Printf("after point yy --> %v\n", yy)
        fmt.Printf("after point zz --> %v\n", zz)
        fmt.Println(" ")

        var t0 UU
        t0 = 6
        var y0 int
        t1 := &t0
        y0 = t0.kaka()
        fmt.Printf("after point zz --> %v\n", t0)
        fmt.Printf("after point zz --> %v\n", y0)
        y0 = t0.kaka1()
        fmt.Printf("after point zz --> %v\n", t0)
        fmt.Printf("after point zz --> %v\n", y0)
        y0 = t1.kaka1()
        fmt.Printf("after point zz --> %v\n", *t1)
        fmt.Printf("after point zz --> %v\n", y0)

}
[GD-0103-0121@rjsys ~/go/src/ptr]$

```
#### 运行结果
```go
[GD-0103-0121@rjsys ~/go/src/ptr]$ ./ptr.exe

 value to func(value): {0 0} -> {1 1} --> {0 0}
 pointer to func(pointer): &{0 0} -> &{1 1} --> &{1 1}
 value to method(value): {0 0} -> {1 1} --> {0 0}
 value to method(pointer): {0 0} -> &{1 1} --> {1 1}
 pointer to method(value): &{1 1} -> {2 2} --> &{1 1}
 pointer to method(pointer): &{1 1} -> &{2 2} --> &{2 2}
 yy --> {0 0}
 zz --> &{0 0}
 -> {1 1} -> {1 1} -> {1 1} -> {1 1}
 -> {1 1} -> {1 1} -> {1 1} -> {1 1} -> {1 1} -> {1 1} -> {1 1}
after value yy --> {0 0}
after value zz --> &{0 0}

 yy --> {0 0}
 zz --> &{0 0}
 -> &{1 1} -> &{2 2} -> &{3 3} -> &{4 4}
 yy --> {4 4}
 -> &{5 5}
after point yy --> {5 5}
after point zz --> &{5 5}

after point zz --> 6
after point zz --> 6
after point zz --> 6
after point zz --> 7
after point zz --> 6
after point zz --> 7
[GD-0103-0121@rjsys ~/go/src/ptr]$

```
