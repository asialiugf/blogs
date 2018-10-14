### 指针实现
```
[GD-0103-0121@rjsys ~/go/src/interf]$ cat a0.go
package main

import (
        "fmt"
)

type Power struct {
        age  int
        high int
        name string
}

//非 指针类型 实现
func (this *Power) String() string {
        return fmt.Sprintf("age:%d, high:%d, name:%s", this.age, this.high, this.name)
}

func main() {
        fmt.Println(" \n var j is value  ***\n")
        var j Power = Power{age: 15, high: 175, name: "Bruce"} //值
        j.String()
        fmt.Printf("%s\n", j)
        fmt.Println(j)
        fmt.Printf("%v\n", j)

        fmt.Println("***")

        fmt.Printf("%s\n", &j)
        fmt.Println(&j)
        fmt.Printf("%v\n", &j)

        fmt.Println(" \n var i is ptr  ***\n")
        var i *Power = &Power{age: 10, high: 178, name: "NewMan"} //指针类型

        i.String()
        fmt.Printf("%s\n", i)
        fmt.Println(i)
        fmt.Printf("%v\n", i)

        fmt.Println("***")

        fmt.Printf("%s\n", *i)
        fmt.Println(*i)
        fmt.Printf("%v\n", *i)
}
[GD-0103-0121@rjsys ~/go/src/interf]$ ./interf.exe

 var j is value  ***

{%!s(int=15) %!s(int=175) Bruce}
{15 175 Bruce}
{15 175 Bruce}
***
age:15, high:175, name:Bruce
age:15, high:175, name:Bruce
age:15, high:175, name:Bruce

 var i is ptr  ***

age:10, high:178, name:NewMan
age:10, high:178, name:NewMan
age:10, high:178, name:NewMan
***
{%!s(int=10) %!s(int=178) NewMan}
{10 178 NewMan}
{10 178 NewMan}
[GD-0103-0121@rjsys ~/go/src/interf]$

```
