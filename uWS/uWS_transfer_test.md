### uWS_transfer_test.md 

uWS 的transfer 和 group的概念很相关。

#### 重要的概念

- 一个uWS::Hub 有一个client组和一个server组，它们在一个线程里。
- 一个uWS::Hub一个线程， 一个线程一个uWS::Hub
- 一个uWS::Hub一个事件循环。
- 一个socket只能属于一个组。
- 一个socket可以以多个组之间切换。

#### 另外在测试中出现的问题：

- 开启一个新线程时，要等它一会儿，让它完全生成后，再给它送数据。
- 在onMessage中收到的数据，(message）要谨慎处理，有可能会出segment falt!!
- 对于char buf[15]，这样的语句，一定要小心不要越界操作。

#### uWebsockets的解释如下：

####　Hub
The Hub is a shared resource blob, local to one tread and not thread-safe. It holds the event-loop, shared buffers and other shared resources.

The uWS::Hub consists of a client group and a server group. It also holds per-thread resources. One hub per thread, one thread per hub.

#### Group
The uWS::Group is a group of sockets and a set of shared event handlers. A socket can only belong to one group at any given time, which means that groups are not intended to be used as "pub/sub rooms". A more suitable use of groups can be to implement subprotocols or differing behavior. There are no per-socket event handlers, all event handlers are attached to a group and all sockets belong to exactly one group.

A socket can transfer between different Groups at run-time and will thus change behavior. There are broadcasting helper functions for Groups, but again, these are not intended for pub/sub and are not even particularly efficient.

#### 下面的实验程序

##### 线程（Main---- Hub h)
main() 生成一个 uWS::Hub h;这个h有一个server组和一个client组。
h.client 在建立连接时，执行了以下两句：
```c
        std::thread t(client);
        t.detach();
```
##### 线程（client---- Hub h)
创建了一个新的线程，这个线程产生了一个新的 uWS::Hub（当然它有自己的 server组和一个client组，我们只用它的client组中的 自己的client）

这个新线程的client向 main()的server建立连接，并发了一个信息，其中，第一字母为 'H'
```
ws->send("Hello forked client ===");
```
##### 线程（server---- hub th)
main()的server onMessage 收到后，发现是 'H'，于是，再创建另一个线程，当然，它也有它自己的 一个server组和一个client组，我们这里，只用它的server组。
 
注意，此时的 client线程的client还是与 main线程的server相连的。

main()的server在onMessage里，创建完这个新的 server线程后， 再向 client线程发了一个"kk"， client在收到后， 又向main的server发了一个信息
```c
    h.onMessage([](uWS::WebSocket<uWS::CLIENT> *ws, char *message, size_t length, uWS::OpCode opCode) {
        char tmp[256];
        memcpy(tmp, message, length);
        tmp[length] = 0;
        printf("Client receive: %s\n", tmp);
        ws->send("Torkkk===");
```
#### server切换
当main的server收到 "Torkkk===" 后，发现第一个字母是'T'，于是，将这个server socket切换给了 （server---- hub th) 的server组。
```c
        if(tmp[0] == 'T') {
            ws->setUserData((void *) 12345);
            ws->transfer(tServerGroup);
        }
```
于是，以后 线程（client---- Hub h)的client socket 再发出的信息，就会被 （server---- hub th) 的server 收到并处理。


#### 程序如下
```c
/*
g++ -std=c++11 -O3 -I ../src -fPIC -o tr1.x transfer1.cpp -L/home/riddle/uws -luWS -lssl -lz -lpthread
*/
```
```c
riddle@d:~/uws/tests$ cat ~/codingtest/uWebsockets/tests/transfer1.ok1.cpp
#include <uWS/uWS.h>
#include <iostream>
#include <thread>   // std::thread
#include <unistd.h>

#pragma comment(lib, "uWS.lib")


int client()
{
    uWS::Hub h;

        std::cout << " thread client is creating!!!!!!!!\n";
    // 客户端连上后发送hello
    h.onConnection([](uWS::WebSocket<uWS::CLIENT> *ws, uWS::HttpRequest req) {
                std::cout << "thread client send: Hello forked client === \n";
        ws->send("Hello forked client ===");
    });

    // 客户端打印接收到的消息
    h.onMessage([](uWS::WebSocket<uWS::CLIENT> *ws, char *message, size_t length, uWS::OpCode opCode) {
        char tmp[256];
        memcpy(tmp, message, length);
        tmp[length] = 0;
        printf("Client receive: %s\n", tmp);
        ws->send("Torkkk===");
                usleep(1000000);
        ws->send("----new-----");
        usleep(1);

    });

    h.onDisconnection([&h](uWS::WebSocket<uWS::CLIENT> *ws, int code, char *message, size_t length) {
        h.getDefaultGroup<uWS::SERVER>().close();
        h.getDefaultGroup<uWS::CLIENT>().close();
    });

    h.connect("ws://localhost:3000");
    h.run();
    return 0;
}

int main()
{
    uWS::Hub h;

    uWS::Group<uWS::SERVER> *tServerGroup = nullptr;

    h.onConnection([&tServerGroup](uWS::WebSocket<uWS::SERVER> *ws, uWS::HttpRequest req) {
        // ws->setUserData((void *) 12345);
        //ws->transfer(tServerGroup);
    });

    // 服务端接收到包后原封不动返回
    h.onMessage([&tServerGroup](uWS::WebSocket<uWS::SERVER> *ws, char *message, size_t length, uWS::OpCode opCode) {
        char tmp[256];
        memcpy(tmp, message, length);
        tmp[length] = 0;
        printf("Main server recieved : %s\n", tmp);

        if(tmp[0] == 'T') {
            ws->setUserData((void *) 12345);
            ws->transfer(tServerGroup);
        }

        if(tmp[0] == 'H') {

            std::mutex m;

            std::thread t([&tServerGroup] {
            
                                std::cout<< "-----thread server -----------------------\n";
                uWS::Hub th;
                tServerGroup = &th.getDefaultGroup<uWS::SERVER>();
                                std::cout<< "-----thread server -----------------------\n";

                bool transferred = false;
                th.onTransfer([&transferred](uWS::WebSocket<uWS::SERVER> *ws)
                {
                    if(ws->getUserData() != (void *) 12345) {
                        std::cout << "onTransfer called with websocket with invalid user data set!" << std::endl;
                        exit(-1);
                    }
                    transferred = true;
                });

                th.onMessage([](uWS::WebSocket<uWS::SERVER> *ws, char *message, size_t length, uWS::OpCode opCode)
                {
                    ws->send("----from transfer server ----");
                });

                th.getDefaultGroup<uWS::SERVER>().listen(uWS::TRANSFERS);
                                std::cout<< "will running ----------------------------\n";
                th.run();
            });  /* thread t */

            std::cout << " in H HHH \n" ;
            //std::thread t(client);
            t.detach();
        }
        tmp[length] = 0;
        //printf("Server receive: %s\n", tmp);
                usleep(1000000);
        ws->send("kk", 2, opCode);
    });

    bool k = h.listen(3000) ;
    if(!k) {
        std::cout << " listen error !!" << std::endl;
    }

    // ----------- inside client ---------------------

    h.onConnection([](uWS::WebSocket<uWS::CLIENT> *ws, uWS::HttpRequest req) {
           std::cout << " inside client will create a new client thread !!\n";
        std::thread t(client);
        t.detach();
                usleep(1000);
    });

    h.onMessage([](uWS::WebSocket<uWS::CLIENT> *ws, char *message, size_t length, uWS::OpCode opCode) {
        char tmp[256];
        memcpy(tmp, message, length);
        tmp[length] = 0;
        printf("Client receive: %s\n", tmp);
        ws->send("forkkk===");
        usleep(1000);

    });
    h.connect("ws://localhost:3000");

    h.run();

}

/*
g++ -std=c++11 -O3 -I ../src -fPIC -o tr1.x transfer1.cpp -L/home/riddle/uws -luWS -lssl -lz -lpthread
*/
riddle@d:~/uws/tests$ 
riddle@d:~/uws/tests$ 
```

#### 运行的结果如下：

```c
riddle@d:~/uws/tests$ ./tr1.x
 inside client will create a new client thread !!
 thread client is creating!!!!!!!!
thread client send: Hello forked client === 
Main server recieved : Hello forked client ===
 in H HHH 
-----thread server -----------------------
-----thread server -----------------------
will running ----------------------------
Client receive: kk
Main server recieved : Torkkk===
Client receive: ----from transfer server ----
Client receive: ----from transfer server ----
Client receive: ----from transfer server ----
Client receive: ----from transfer server ----
Client receive: ----from transfer server ----
Client receive: ----from transfer server ----
Client receive: ----from transfer server ----
```
