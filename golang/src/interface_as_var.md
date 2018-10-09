```go
[GD-0103-0121@rjsys ~/go/src/interf]$ cat a.go
package main

import "fmt"

var a interface {
        R() int
}

type X struct {
}

func (this X) R() int {
        fmt.Println("in X")
        return 0
}

func main() {
        var b X
        //var b int = 3
        a = b
        b.R()
        a.R()
        fmt.Println("test")

}
[GD-0103-0121@rjsys ~/go/src/interf]$
[GD-0103-0121@rjsys ~/go/src/interf]$ ./interf.exe
in X
in X
test
[GD-0103-0121@rjsys ~/go/src/interf]$

```
