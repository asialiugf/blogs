###  非指针实现
* 非指针实现， 参数可以是 是指针 也可以是值 
```go
[GD-0103-0121@rjsys ~/go/src/interf]$ cat a.go

package main

import (
        "fmt"
)

//---- package hello ------ begin --------
type II interface {
        test()
}

func hello(this II) {
        this.test()
}

//---- package hello ------ end --------

type Ma struct {
        name string
}

func (this Ma) test() {
        fmt.Printf("%s\n", this.name)
}

func main() {
        fmt.Printf("rr\n")
        var ma = Ma{"HHHHHHHHHH"}
        hello(ma)  // ok
        hello(&ma) // ok
}
[GD-0103-0121@rjsys ~/go/src/interf]$



[GD-0103-0121@rjsys ~/go/src/interf]$ ./interf.exe

rr
HHHHHHHHHH
HHHHHHHHHH
[GD-0103-0121@rjsys ~/go/src/interf]$


```

### 指针实现 
* 指针实现， 参数只能是指针。
```go

package main

import (
    "fmt"
)

//---- package hello ------ begin --------
type II interface {
    test()
}

func hello(this II) {
    this.test()
}

//---- package hello ------ end --------

type Ma struct {
    name string
}

func (this *Ma) test() {
    fmt.Printf("%s\n", this.name)
}

func main() {
    fmt.Printf("rr\n")
    var ma = Ma{"HHHHHHHHHH"}
    hello(ma)  // error !!
    hello(&ma) // ok
}

[GD-0103-0121@rjsys ~/go/src/interf]$ go build
# interf
.\a.go:29:7: cannot use ma (type Ma) as type II in argument to hello:
        Ma does not implement II (test method has pointer receiver)
[GD-0103-0121@rjsys ~/go/src/interf]$

```
