### uWebsockets测试  
所在目录为wWebsockets/tests目录，这个目录，是直接从 https://github.com/uNetworking/uWebSockets  git下来的目录。
```
/home/riddle/uws/tests
```
目录下的主要有文件有：
```
-rwxrwxr-x 1 riddle riddle  20088 Dec 22 00:06 client
-rwxrwxr-x 1 riddle riddle  20088 Dec 22 00:06 client1
-rw-rw-r-- 1 riddle riddle   1175 Dec 21 23:46 client1.cpp
-rw-rw-r-- 1 riddle riddle   1169 Dec 21 23:45 client.cpp
-rwxrwxr-x 1 riddle riddle    328 Dec 21 23:47 m
-rwxrwxr-x 1 riddle riddle 159944 Dec 21 19:23 main
-rw-rw-r-- 1 riddle riddle  40762 Dec 21 19:23 main.cpp
-rw-rw-r-- 1 riddle riddle  40764 Dec 18 12:27 main.cpp.bak
-rw-rw-r-- 1 riddle riddle  22512 Dec 18 11:41 Makefile
-rwxrwxr-x 1 riddle riddle  21472 Dec 22 00:06 server
-rw-rw-r-- 1 riddle riddle   1056 Dec 21 23:10 server.cpp
-rw-rw-r-- 1 riddle riddle    904 Dec 12 20:43 uWebSockets.pro
```
有四个主要的程序
- server.cpp
- client.cpp
- client1.cpp
- m

具体内容如下：

在server.cpp里，虽然只定义了一个 uWS::Hub h; 但这个h,可以担任client和server两个角色。
可以看到，在程序的最后，即有 listen(server角色) 又有 connect(client角色)

### server.cpp:
``` c++

riddle@:~/uws/tests$ cat server.cpp
#include <uWS/uWS.h>
#include <iostream>

#pragma comment(lib, "uWS.lib")

int main()
{
    uWS::Hub h;

    // 客户端连上后发送hello
    h.onConnection([](uWS::WebSocket<uWS::CLIENT> *ws, uWS::HttpRequest req) {
        ws->send("Hello");
    });

    // 客户端打印接收到的消息
    h.onMessage([](uWS::WebSocket<uWS::CLIENT> *ws, char *message, size_t length, uWS::OpCode opCode) {
        char tmp[16];
        memcpy(tmp, message, length);
        tmp[length] = 0;
        printf("Client receive: %s\n", tmp);

        ws->close();
    });

    // 服务端接收到包后原封不动返回
    h.onMessage([](uWS::WebSocket<uWS::SERVER> *ws, char *message, size_t length, uWS::OpCode opCode) {
        char tmp[16];
        memcpy(tmp, message, length);
        tmp[length] = 0;
        printf("Server receive: %s\n", tmp);
        ws->send(message, length, opCode);
    });

    bool k = h.listen(3000) ;
    if(!k) {
        std::cout << " listen error !!" << std::endl;
    }
    h.connect("ws://localhost:3000");
    h.connect("ws://localhost:3000");  //这里也可以有多个连接，并且能和server连接上，但上面的 只有一个 onmessage(CLIENT) 能回应.
    h.connect("ws://localhost:3000");

    h.run();
}
riddle@:~/uws/tests$ 
```

### client1.cpp
在client里，如果只有 connect动作， 那么， 对于 诸如 onConnection([](uWS::WebSocket<uWS::CLIENT> *ws, uWS::HttpRequest req) 
这样的语句中，如果 将CLIENT改为SERVER，那不会有任何作用。
另外， 在这里lambda表达式里，可以捕获各种参数来进行操作。

``` c++
riddle@:~/uws/tests$ cat client1.cpp
#include <uWS/uWS.h>
#include <unistd.h>

#pragma comment(lib, "uWS.lib")

int main()
{
    uWS::Hub h;

    // 客户端连上后发送hello
    h.onConnection([](uWS::WebSocket<uWS::CLIENT> *ws, uWS::HttpRequest req) {
        while(1) {
            ws->send("buggg!!!!!!");
            //sleep(1);
            usleep(1000000);
                        break;
        }

    });

    // 客户端打印接收到的消息
    h.onMessage([](uWS::WebSocket<uWS::CLIENT> *ws, char *message, size_t length, uWS::OpCode opCode) {
        char tmp[16];
        memcpy(tmp, message, length);
        tmp[length] = 0;
        printf("Client receive: %s\n", tmp);
        ws->send(message, length, opCode);
        usleep(1000000);

        //ws->close();
    });

    // 服务端接收到包后原封不动返回
    /*
    h.onMessage([](uWS::WebSocket<uWS::SERVER> *ws, char *message, size_t length, uWS::OpCode opCode) {
        char tmp[16];
        memcpy(tmp, message, length);
        tmp[length] = 0;
        printf("Server receive: %s\n", tmp);
        ws->send(message, length, opCode);
    });
    */

    /*
    h.listen(3000);
    */
    h.connect("ws://localhost:3000");

    h.run();
}
riddle@:~/uws/tests$ 
```
### client.cpp
``` c++
riddle@:~/uws/tests$ cat client.cpp
#include <uWS/uWS.h>
#include <unistd.h>

#pragma comment(lib, "uWS.lib")

int main()
{
    uWS::Hub h;

    // 客户端连上后发送hello
    h.onConnection([](uWS::WebSocket<uWS::CLIENT> *ws, uWS::HttpRequest req) {
        while(1) {
            ws->send("Hello");
            //sleep(1);
            usleep(1000000);
                        break;
        }

    });

    // 客户端打印接收到的消息
    h.onMessage([](uWS::WebSocket<uWS::CLIENT> *ws, char *message, size_t length, uWS::OpCode opCode) {
        char tmp[16];
        memcpy(tmp, message, length);
        tmp[length] = 0;
        printf("Client receive: %s\n", tmp);
        ws->send(message, length, opCode);
        usleep(1000000);

        //ws->close();
    });

    // 服务端接收到包后原封不动返回
    /*
    h.onMessage([](uWS::WebSocket<uWS::SERVER> *ws, char *message, size_t length, uWS::OpCode opCode) {
        char tmp[16];
        memcpy(tmp, message, length);
        tmp[length] = 0;
        printf("Server receive: %s\n", tmp);
        ws->send(message, length, opCode);
    });
    */

    /*
    h.listen(3000);
    */
    h.connect("ws://localhost:3000");

    h.run();
}
riddle@:~/uws/tests$ 
```
### m的内容如下：
```
riddle@:~/uws/tests$ cat m
# g++ -std=c++11 -O3 -I ../src -fPIC -o main main.cpp -luWS -lssl -lz -lpthread
g++ -std=c++11 -O3 -I ../src -fPIC -o server server.cpp -luWS -lssl -lz -lpthread
g++ -std=c++11 -O3 -I ../src -fPIC -o client client.cpp -luWS -lssl -lz -lpthread
g++ -std=c++11 -O3 -I ../src -fPIC -o client1 client1.cpp -luWS -lssl -lz -lpthread
riddle@:~/uws/tests$ 
```
### 具体的操作如下：

打开三个窗口
第一个窗口 运行 ./server
第二个窗口 运行 ./client
第三个窗口 运行 ./client

#### server窗口显示如下：
```
riddle@:~/uws/tests$ ./server
Server receive: Hello
Client receive: Hello
Server receive: Hello
Server receive: Hello
Server receive: Hello
Server receive: Hello
Server receive: Hello
Server receive: buggg!!!!!!
Server receive: Hello
Server receive: buggg!!!!!!
Server receive: Hello
Server receive: buggg!!!!!!
Server receive: Hello
Server receive: buggg!!!!!!
Server receive: Hello
```
#### client1窗口显示如下：
```
riddle@:~/uws/tests$ ./client1
Client receive: buggg!!!!!!
Client receive: buggg!!!!!!
Client receive: buggg!!!!!!
Client receive: buggg!!!!!!
Client receive: buggg!!!!!!
Client receive: buggg!!!!!!
Client receive: buggg!!!!!!
Client receive: buggg!!!!!!
Client receive: buggg!!!!!!
```
#### client窗口显示如下：
```
riddle@:~/uws/tests$ ./client
Client receive: Hello
Client receive: Hello
Client receive: Hello
Client receive: Hello
Client receive: Hello
Client receive: Hello
Client receive: Hello
Client receive: Hello
Client receive: Hello
```
#### 结论
##### 一
一个server可以监听多个client发过来的请求
在这一个server内，只需要用一个
```c++
 h.onMessage([](uWS::WebSocket<uWS::SERVER> *ws, char *message, size_t length, uWS::OpCode opCode) {
```
就可以监听并可以回复多个请求。

##### 二
在server.cpp内部，也可通过
```c++
 h.connect("ws://localhost:3000");
```
以及：
```c++
h.onMessage([](uWS::WebSocket<uWS::CLIENT> *ws, char *message, size_t length, uWS::OpCode opCode) {
```
来实现内部的client 和 server通信

##### 注意
onmessage里面的 SERVER 以及 CLIENT 的区别
启动的顺序，必须是server先启动
在client程序中，只需要 
```c++
 h.connect("ws://localhost:3000");
```
不需要 listen动作。

-------- 分割线 ---------------------------

### 在控制台上运行

![image](https://github.com/asialiugf/blogs/blob/master/uWS/uws-test01.png)

先在linux机器上运行上面的 ./server，
在chrome浏览器,运行一个插件："simple websocket client" 输入url:   ws:://x.x.x.x:3000，并进行连接。
在chrome浏览器上按F12出现控制台，输入以下代码，可以看到，服务器可以收到 "tom"。

```javascript
ws = new WebSocket("ws://47.93.37.54:3000");
ws.onopen = function() {
    alert("连接成功");
    ws.send('tom');
    alert("给服务端发送一个字符串：tom");
};
ws.onmessage = function(e) {
    alert("收到服务端的消息：" + e.data);
};
```

