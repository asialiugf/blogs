```go

[GD-0103-0121@rjsys ~/go/src/interf]$ cat a.go
package main

import (
        "fmt"
)

func Any(v interface{}) {

        if v2, ok := v.(string); ok {
                println(v2)
        } else if v3, ok2 := v.(int); ok2 {
                println(v3)
        }
}

func main() {
        Any(2)
        Any("666")

        var x interface{}
        var m string = "heeel"
        x = int(5)
        x = m
        //switch t := t.(type) {
        switch t := x.(type) {
        default:
                fmt.Printf("unexpected type %T", t) // %T prints whatever type t has
        case bool:
                fmt.Printf("boolean %t\n", t) // t has type bool
        case int:
                fmt.Printf("integer %d\n", t) // t has type int
        case *bool:
                fmt.Printf("pointer to boolean %t\n", *t) // t has type *bool
        case *int:
                fmt.Printf("pointer to integer %d\n", *t) // t has type *int
        case string:
                fmt.Printf("string %s\n", t) // t has type *int
        }
}
[GD-0103-0121@rjsys ~/go/src/interf]$

```
