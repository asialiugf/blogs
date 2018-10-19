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
### 实现
1. 定义interface 相关函数
2. 定义泛型调用函数，并使用 interface 的相关函数来实现 这个泛型函数。
3. 定义自己的类型
4. 用自己的类型，实现 interface定义的函数

然后，就可以在其它地方（比如main()）中来直接使用 泛型函数了。

```go
[GD-0103-0121@rjsys ~/go/src/interfacetest]$ cat t01.go
package main

import (
        "fmt"
)

// ----------  interface ------------
type TT interface {
        hello()
}

func Say(this TT) {
        this.hello()
}

// ----------  local1 type ------------
type SS string

func (this SS) hello() {

        fmt.Println("arHar:", this)
}

// ----------  local2 type ------------
type HS []string

func (this HS) hello() {
        for i := 0; i < len(this); i++ {
                fmt.Print(this[i], "  ")
        }
        fmt.Print("\n")
}

func (this HS) HS_Say() {
        Say(this)
}

// ----------  local3 type ------------
type IS []int

func (this IS) hello() {
        for i := 0; i < len(this); i++ {
                fmt.Print(this[i], "  ")
        }
        fmt.Print("\n")

}

// ----------  main ------------
func main() {
        var tt SS = "aaaaaaaaa"
        Say(tt)
        var hs HS = []string{"aaaa", "bbb", "ccc"}
        Say(hs)
        hs.HS_Say()
        var is IS = []int{1, 2, 45, 33}
        Say(is)
}
[GD-0103-0121@rjsys ~/go/src/interfacetest]$ ./interfacetest.exe
arHar: aaaaaaaaa
aaaa  bbb  ccc
aaaa  bbb  ccc
1  2  45  33
[GD-0103-0121@rjsys ~/go/src/interfacetest]$

```


