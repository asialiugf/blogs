

template和std::function的性能对比测试

备注：
程序来源于网络加自我修改，如有侵权，请联系我删除。本程序在linux 下编译。环境如下：

Linux 4.4.0-62-generic #83-Ubuntu SMP Wed Jan 18 14:10:15 UTC 2017 x86_64 x86_64 x86_64 GNU/Linux
$ g++ --version
g++ (Ubuntu 5.4.0-6ubuntu1~16.04.5) 5.4.0 20160609
Copyright (C) 2015 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
一
看下面 注1 注2 注 3 ：

结论是 采用 temlate 来定义 函数 比用 std::function来定义函数，执行速度要快。

另外，下面还测试了：
将函数指针定义成一个类型，并用这个类型来定义一个vector，通过将 struct强制转成 char * 来进行不同函数的参数传递。

```
// $ cat func.cpp
#include <iostream>
#include <functional>
#include <string>
#include <chrono>
#include <vector>

template <typename F>
float calc1(F f)  {return -1.0f * f(3.3f) + 666.0f;}
float calc2(std::function<float(float)> f)  {return -1.0f * f(3.3f) + 666.0f;}
//  ----注 1 ---- 
// calc1函数，其参数是一个函数， 即以 f(float) 函数  为参数。
// calc2和calc1一样，只不过，一个是以template来定义，另一个用std::function来定义。

//template <typename X>
template <typename F>
int cal1(F f)
{
    return 3+f(10);
}
int cal2(std::function<int(int)> f)
{
    return 3+f(10);
}

typedef void(* EV)(char *) ;

typedef struct {
    int a;
} t1_t;

typedef struct {
    int a;
    double b;
} t2_t;

void t1(char *b)
{
    t1_t *x;
    x = (t1_t *) b;
    std::cout << "in t1 a:: " << x->a <<std::endl;
}

void t2(char *b)
{
    t2_t *x;
    x = (t2_t *) b;
    std::cout << "in t2 a:: " << x->a <<std::endl;
    std::cout << "in t2 b:: " << x->b <<std::endl;
}

int main()
{
    using namespace std::chrono;

    const auto tp1 = system_clock::now();
    for(int i = 0; i < 1e8; ++i) {
        calc1([](float arg) {
            return arg * 0.5f;
        });
    }
    // ---- 注 2 ----
    // 以上for循环中， 通过 lambda表示式生成一个匿名函数，给calc1调用。
    // 通过修改calc1为calc2来测试两次。再看 （注3）处的结果

    const auto tp2 = high_resolution_clock::now();
    const auto d = duration_cast<milliseconds>(tp2 - tp1);
    std::cout << d.count() << std::endl;   
    // ---- 注3----   输出运行时间
    // 测试的结果是：
    // 采用template方式，运行时间为0, 采用 std::function ，运行时间为 209
    // 结论： template 性能要比 std::function好。

    std::cout << calc1([](float arg) {
        return arg * 0.5f;
    }) << std::endl;
    std::cout << calc2([](float arg) {
        return arg * 0.5f;
    }) << std::endl;

    std::cout << cal1([](int arg) {
        return arg * 15;
    }) << std::endl;
    std::cout << cal2([](int arg) {
        return arg * 15;
    }) << std::endl;

    // --------------------

    std::vector<EV> evv ;
    evv.push_back(t1);
    evv.push_back(t2);

    t2_t ttt;
    ttt.a = 1;
    ttt.b =199.25;

    for(int i=0 ; i<evv.size(); i++) {
        evv[i]((char *)&ttt);
    }




    return 0;
}
```
编译方式：

```
g++ -o func -std=c++11  func.cpp -lpthread -O3
注：这里的-O3 也可以改成 -O2，本人没有研究区别，有知道的同学，可以留言或评论。
```


