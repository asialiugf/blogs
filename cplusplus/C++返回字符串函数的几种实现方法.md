### C++返回字符串函数的几种实现方法


#### C++返回字符串函数有四种方式：
1. 使用堆空间，返回申请的堆地址，注意释放
2. 函数参数传递指针，返回该指针
3. 返回函数内定义的静态变量（共享）
4. 返回全局变量
#### 1.使用堆空间，返回申请的堆地址，注意释放
其实就是要返回一个有效的指针，尾部变量退出后就无效了。 
使用分配的内存，地址是有效 
```c
char   *fun()   
{   
        char*   s = (char*)calloc(100,   sizeof(char*)   );   
        if   (s)   
                strcpy( s,"abc");   
        return   s;   
}
```
但这种方式需要注意，必须由使用将将返回的地址free掉 
#### 2.函数参数传递指针，返回该指针
将地址由入参传入 
```c
char*   fun(char*s)   
{   
        if (s)   
              strcpy(s,"abc");   
        return   s;   
}
```
这种方式调用都要注意给s分配的大小是足够。 
可以这样： 
```c
char*   fun(char*s,   int   len)   
{   
        if   (s)   
        {   
              strncpy(s,   "abc ",   len-1);   
              s[len-1]   =   0;   
        }   
        return   s;   
}
```
#### 3.使用局部静态变量 
```c
char*   fun()   
{   
        static   char   s[100];   
        strcpy(s,   "abc ");   
        return   s;   
}
```
这种方式需要注意，不要修改返回的这个字符串，由于是共享地址，对它的修改会反应到每个调用者的。可以这样： 
```c
const   char*   fun()   
{   
        static   char   s[100];   
        strcpy(s,   "abc ");   
        return   s;   
}
```
#### 4.使用全局变量 
```c
char   g_s[100];   
char*   fun()   
{   
        strcpy(g_s,   "abc ");   
        return   s;   
}
```
同样的，也要注意这个变量可存储的最大空间。

来源：http://blog.csdn.net/shenziheng1/article/details/53591497
