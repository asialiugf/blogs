### template传函数

```c++
riddle@:~/codingtest/cpp$ cat *
g++ -std=c++11 -O3  -fPIC -o t1 t1.cpp
```

```c++
#include <iostream>

template <void C(int a, bool error)>
static void connect_cb(int b, bool status) {
    C(b, status < 0);
}

void abc(int a,bool b){
        if(b){
                std::cout << a << std::endl;
        }
}

int main(){
    connect_cb<abc>(25,1);
    return 0; 
}
riddle@asiamiao:~/codingtest/cpp$ 
```
