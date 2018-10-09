```go

[GD-0103-0121@rjsys ~/go/src/slot]$
[GD-0103-0121@rjsys ~/go/src/slot]$ cat a.go
package main

import (
        "fmt"
        "github.com/asialiugf/meta"
        _ "sync"
        _ "time"
)

type Foo struct {
        // fields...

        Done meta.Signal
}

type Guy struct {
        Name string
}

func (mr Guy) Print(n int) {
        fmt.Printf("%s says: %d\n", mr.Name, n)
        //waiter.Done()
}

type Ha struct {
        Name string
}

func (this Ha) Print(n string) {
        fmt.Printf("%s says: %s\n", this.Name, "this is a test !!")
        //waiter.Done()
}

//var waiter sync.WaitGroup

func main() {
        var foo Foo

        johny := Guy{"Johny"}
        david := Guy{"David"}
        haha := Ha{"haha"}

        meta.Connect(&foo.Done, func(call *meta.Call) {
                if passed, ok := call.Data.(int); ok {
                        johny.Print(passed)
                }
        })

        meta.Connect(&foo.Done, func(call *meta.Call) {
                if passed, ok := call.Data.(int); ok {
                        david.Print(passed)
                }
        })

        meta.Connect(&foo.Done, func(call *meta.Call) {
                if _, ok := call.Data.(int); ok {
                        haha.Print("haha")
                }
        })

        //waiter.Add(2)

        //time.Sleep(3 * time.Second)

        // Emit notifies all the connected slots, by running
        // them in the distinct goroutines.
        foo.Done.Emit(42)
        fmt.Println("hhhhhhhhhh")

        //time.Sleep(1 * time.Second)
        //waiter.Wait()

        //
        // Johny says: 42
        // David says: 42
}
[GD-0103-0121@rjsys ~/go/src/slot]$

```
