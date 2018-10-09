```go

[GD-0103-0121@rjsys ~/go/src/slot]$ cd ..
[GD-0103-0121@rjsys ~/go/src]$ cat github.com/asialiugf/meta/meta.go
// Package meta provides signals and slots for the Observer pattern.
//
// Note: This is a proof of concept package, therefore you are not
// supposed to use it in production or any near. Public API of this
// package is likely to change (a lot).
//
// Signals and slots is a language construct introduced in Qt for
// communication between objects which makes it easy to implement
// the Observer pattern while avoiding boilerplate code. The concept
// is that GUI widgets can send signals containing event information
// which can be received by other controls using special functions,
// known as slots.
//
// Once a signal gets emitted, all the slots connected are being
// executed in the distinct goroutines. Value passed to the signal
// is available from the call context object.
package meta

// Signal is an adorable piece of a structure!
type Signal struct {
        cons []con

        // gets incremented on every connect
        conID Connection
}

type con struct {
        slot Slot
        cid  Connection
}

// Slot is a reciver function, usually a wrapper.
type Slot func(*Call)

// Call stands for the call context, handled in the slot.
type Call struct {
        Data interface{}
}

// Connection is a signal-slot connection descriptor, unique
// within the lifetime of the signal.
//
// You may use it to terminate existing connections.
type Connection int

// Connect attaches a new slot to the signal. It also does
// panic if any of the params given is equal to nil.
func Connect(sig *Signal, slot Slot) Connection {
        if sig == nil {
                panic("can't connect a nil signal")
        }

        if slot == nil {
                panic("can't connect a nil slot")
        }

        sig.conID++

        newConnection := con{
                slot: slot,
                cid:  sig.conID,
        }

        sig.cons = append(sig.cons, newConnection)

        return sig.conID
}

// Disconnect does the opposite to connect.
func Disconnect(sig *Signal, cid Connection) {
        if sig == nil {
                panic("can't disconnect a nil signal")
        }

        if cid <= 0 {
                panic("invalid connection id")
        }

        for i := 0; i < len(sig.cons); i++ {
                if sig.cons[i].cid == cid {
                        sig.cons = append(sig.cons[:i], sig.cons[i+1:]...)
                        return
                }
        }
}

// Emit executes all the connected slots with data given.
func (sig *Signal) Emit(data interface{}) {
        for i := 0; i < len(sig.cons); i++ {
                //go sig.cons[i].slot(&Call{Data: data})
                sig.cons[i].slot(&Call{Data: data})
        }
}
[GD-0103-0121@rjsys ~/go/src]$

```

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
