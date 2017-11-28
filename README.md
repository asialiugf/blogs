cd ~/see
mkdir bin
cd ~/see/pgm
make

c语言编译
http://nethack4.org/blog/building-c.html

```
class Event(object):
    def __init__(self):
        print('enter into Event()')
        self.__handlers = set()

    def __iadd__(self, handler):
        print('__idadd__')
        self.__handlers.add(handler)
        return self

    def __isub__(self, handler):
        self.__handlers.discard(handler)
        return self

    def fire(self, *args, **kwargs):
        print(*args)
        for handler in self.__handlers:
            handler(*args, **kwargs)

    def clear(self):
        self.__init__()

    def clear_obj_handler(self, obj):
        self.__handlers = [h for h in self.__handlers if h.__self__ != obj]


kk = Event()
kk.fire(1,3)
```
```
package main

import (
	"fmt"
	"usr/ccc"
	"usr/util"
)

func main() {
	fmt.Printf("Hello, world.\n")
	fmt.Printf("%d\n", util.Add(3, 4))
	fmt.Printf("%d\n", bbb.Add(3, 4))
	fmt.Printf("%d\n", util.Sdd(3, 4))
}

```
