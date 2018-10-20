###



```[GD-0103-0121@rjsys ~/go/src/interfacetest/test2]$ cat t.go
package main

import (
        "fmt"
)

type Stringer interface {
        String() string
}

type Binary uint64

func (i Binary) String() string {
        return "akakak"
}

func (this *Binary) SetS() {

}

func (i Binary) Get() uint64 {
        return uint64(i)
}

func main() {
        b := Binary(200)
        s := Stringer(b)             // 将b  copy 给 s s是Stringer类型，是一个interface. 
        fmt.Println(s.String())
        var ss Stringer
        ss = b  // copy
        ss = s  // copy 
        b = b + 1
        var a int
        a = int(b)
        fmt.Println(ss.String())
        fmt.Println("aa:", ss)
        fmt.Println(a)
        fmt.Println(int(b))

        //a = int(ss)
        str, ok := ss.(Binary)         //  a = int(ss) 这样不行，只能 value, ok := ss.(tppe)
        if ok {
                a = int(str)
        } else {
        }

        fmt.Println(a)
}
[GD-0103-0121@rjsys ~/go/src/interfacetest/test2]$
[GD-0103-0121@rjsys ~/go/src/interfacetest/test2]$
[GD-0103-0121@rjsys ~/go/src/interfacetest/test2]$ ./test2.exe
akakak
akakak
aa: akakak
201
201
200
[GD-0103-0121@rjsys ~/go/src/interfacetest/test2]$

```
