### interface{} 是一个类型
```go
func test(a interface{}) interface{} {
        var y int = 5
        fmt.Println(a)
        // return a
        return y
}
```
上面test()这个函数的参数是任意类型，返回也是任意类型。

* 参数可以是 int ， 返回一个 string
* 也可以参数可以是 string， 返回一个 int

```go
[GD-0103-0121@rjsys ~/go/src/interfacetest]$ cat *.go
package main

import "fmt"

func test(a interface{}) interface{} {
        var y int = 5
        fmt.Println(a)
        // return a
        return y
}

func main() {
        fmt.Println("vim-go")
        var x interface{} = 0

        var y string = "test!!!"

        x = test(x)
        fmt.Println(x)

        x = test(y)
        fmt.Println(x)
}
[GD-0103-0121@rjsys ~/go/src/interfacetest]$
[GD-0103-0121@rjsys ~/go/src/interfacetest]$ ./interfacetest.exe
vim-go
0
5
test!!!
5
[GD-0103-0121@rjsys ~/go/src/interfacetest]$

```
