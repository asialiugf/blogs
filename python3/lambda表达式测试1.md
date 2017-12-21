#### C++ lambda 测试

1、函数内定义函数
2、函数内返回函数 

先定义一个 函数，通过参数a,b 来确定一条直线 y = f(x) = ax+b 

然后 通过调用这个函数，输入不同的a,b, 产生不同的斜率的直线（函数）。

最后，调用 这个直线函数， 通过 x 来确定 y值。

另外还测试了一下  #define func auto ，可行。

#####  lambda使用的好处

1 当程序写着写着，突然觉得应该定义一个临时的小函数，来处理一下，这个小函数，还可能就近取当前的数据。
  这时候，lambda来了，你不用跑到main外，再申明，再定义，然后再回来接着写。

2 只要是在函数内要返回一个函数时，可以用它。

3 多线程处理中可以用到。

4 程序更简洁的需要。

注意，lambda有时间会要牺牲一些性能。


```
riddle@asiamiao:~/study/tp1$ cat tp02.cpp
#include <thread>
#include <iostream>
#include<stdio.h>

#define func auto 

int fun()
{
    auto f = [](std::string s1,std::string s2) {
        std::cout << s1 << s2;
    };
    f("hello ","world\n");
    auto f2 = [](std::string s1,std::string s2="hahah\n") {
        std::cout << s1 << s2;
    };
    f2("eric ");
}

func test()   // func is auto
{
    int local = 3;
    return [=](int x) {      // 相当于返回一个函数。
        return x + local ;
    };
}

int main()
{
    func f=[](int input) {
        int local=3;//这个local变量的生命周期，是在lambda声明末尾，还是整个main函数?
        return [=](int x) {
            return input+local+x;
        };
    };

    auto nn= []() {
        return 100;
    };
    std::cout << "nnnn  is:  " << nn() << std::endl;

    auto line = [](int a,int b) {
        return [a,b](int x) {     // [a,b] 可以改成[=] 表示值传递
            return  a*x + b ;
        };
    };
    auto line1 = line(3,4);
    auto line2 = line(5,6);
    std::cout << "line1 is:  " << line1(5) << std::endl;
    std::cout << "line1 is:  " << line2(5) << std::endl;

    auto f1=f(3);
    auto f2=f(4);

    auto m = test();
    int xx = m(3);
    std::cout << "xxxx is:  " << xx << std::endl;

    printf("%d,%d\n",f1(2),f2(2));

    int n1 = 500;
    int n2 = 600;
    std::thread t([&](int addNum) {
        n1 += addNum;
        n2 += addNum;
    },500);
    t.join();
    std::cout << n1 << ' ' << n2 << std::endl;

    fun();

}
```
编译方式：
```
g++ -o tp02 -std=c++14  tp02.cpp -lpthread
```
如果采用 c++11来编译，会报下面的错：

```
g++ -o tp02 -std=c++11  tp02.cpp -lpthread
```

```
tp02.cpp:19:11: error: ‘test’ function uses ‘auto’ type specifier without trailing return type
 func test()   // func is auto
           ^
tp02.cpp:19:11: note: deduced return type only available with -std=c++14 or -std=gnu++14

riddle@asiamiao:~/study/tp1$ 
```
