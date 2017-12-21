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

server.cpp:
``` c++

riddle@asiamiao:~/uws/tests$ cat server.cpp
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

    h.run();
}
riddle@asiamiao:~/uws/tests$ 
```

client1.cpp

``` c++
riddle@asiamiao:~/uws/tests$ cat client1.cpp
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
riddle@asiamiao:~/uws/tests$ 
```
client.cpp
``` c++
riddle@asiamiao:~/uws/tests$ cat client.cpp
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
riddle@asiamiao:~/uws/tests$ 
```
m的内容如下：
```
riddle@asiamiao:~/uws/tests$ cat m
# g++ -std=c++11 -O3 -I ../src -fPIC -o main main.cpp -luWS -lssl -lz -lpthread
g++ -std=c++11 -O3 -I ../src -fPIC -o server server.cpp -luWS -lssl -lz -lpthread
g++ -std=c++11 -O3 -I ../src -fPIC -o client client.cpp -luWS -lssl -lz -lpthread
g++ -std=c++11 -O3 -I ../src -fPIC -o client1 client1.cpp -luWS -lssl -lz -lpthread
riddle@asiamiao:~/uws/tests$ 
```


