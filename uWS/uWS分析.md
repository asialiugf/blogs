##  uWS分析

使用uWS，一般会采用下面的结构，让我们从一个简单的 websockets server开始：
```c++
riddle@d:~/uws/tests$ cat server.cpp
#include <uWS/uWS.h>
#include <unistd.h>
#include <iostream>

#pragma comment(lib, "uWS.lib")

int main()
{
    uWS::Hub h;

    // 客户端连上后发送hello
    h.onConnection([](uWS::WebSocket<uWS::CLIENT> *ws, uWS::HttpRequest req) {
        ws->send("Hello");
    });
    h.onConnection([](uWS::WebSocket<uWS::SERVER> *ws, uWS::HttpRequest req) {
        ws->send("-----No------");
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
riddle@d:~/uws/tests$ 
```
### 其实以上这段程序，可以修改成以下的写法，看起来更清晰。

这段程序主要分为两个部分：
1、server部分
2、client部分
也就是说，这个程序，是将server和client写在一起了。实际的情况，有可能是分别来写。

从下面的程序，我们可以看到，server和client是共用一个 uWS::Hub h;这并没有什么问题。要注意的是，server的listen动作，必须先于client的connect 执行。

最后的h.run()，让程序进入到事循环。

``` c++
riddle@d:~/uws/tests$ cat server.cpp
#include <uWS/uWS.h>
#include <unistd.h>
#include <iostream>

#pragma comment(lib, "uWS.lib")

int main()
{
    uWS::Hub h;

        // --------------------------------------------------------------------------------------------
    // 服务端接收到包后原封不动返回 
    h.onConnection([](uWS::WebSocket<uWS::SERVER> *ws, uWS::HttpRequest req) {
        ws->send("-----No------");
    });

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
        // --------------------------------------------------------------------------------------------

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
        ws->send(message, length, opCode);
        usleep(1000000);

        //ws->close();
    });
    h.connect("ws://localhost:3000");
        // --------------------------------------------------------------------------------------------


    h.run();
}
riddle@d:~/uws/tests$ 
```
### 下面来具体分析

先定义了一个对象h.
```c++
uWS::Hub h;
```
然后执行事件注册
```c++
    // 服务端接收到包后原封不动返回
    h.onConnection([](uWS::WebSocket<uWS::SERVER> *ws, uWS::HttpRequest req) {
        ws->send("-----No------");
    });

```
在 src/目录下的Hub.h中定义了Hub类：
```c++
struct WIN32_EXPORT Hub : private uS::Node, public Group<SERVER>, public Group<CLIENT> {
    // ...
    // ...
    using uS::Node::run;
    using uS::Node::getLoop;
    using Group<SERVER>::onConnection;
    using Group<CLIENT>::onConnection;
    using Group<SERVER>::onTransfer;
    using Group<CLIENT>::onTransfer;
    using Group<SERVER>::onMessage;
    using Group<CLIENT>::onMessage;
    using Group<SERVER>::onDisconnection;
    using Group<CLIENT>::onDisconnection;
    using Group<SERVER>::onPing;
    using Group<CLIENT>::onPing;
    using Group<SERVER>::onPong;
    using Group<CLIENT>::onPong;
    using Group<SERVER>::onError;
    using Group<CLIENT>::onError;
    using Group<SERVER>::onHttpRequest;
    using Group<SERVER>::onHttpData;
    using Group<SERVER>::onHttpConnection;
    using Group<SERVER>::onHttpDisconnection;
    using Group<SERVER>::onHttpUpgrade;
    using Group<SERVER>::onCancelledHttpRequest;

    friend struct WebSocket<SERVER>;
    friend struct WebSocket<CLIENT>;
};

```
可以看到， 各种事件，都在这里。其中 server 和 client 的 onConnection均是从 Group里继承过来。
再来看看 Group
```c++
public:
    void onConnection(std::function<void(WebSocket<isServer> *, HttpRequest)> handler);
    void onTransfer(std::function<void(WebSocket<isServer> *)> handler);
    void onMessage(std::function<void(WebSocket<isServer> *, char *, size_t, OpCode)> handler);
    void onDisconnection(std::function<void(WebSocket<isServer> *, int code, char *message, size_t length)> handler);
    void onPing(std::function<void(WebSocket<isServer> *, char *, size_t)> handler);
    void onPong(std::function<void(WebSocket<isServer> *, char *, size_t)> handler);
    void onError(std::function<void(errorType)> handler);
    void onHttpConnection(std::function<void(HttpSocket<isServer> *)> handler);
    void onHttpRequest(std::function<void(HttpResponse *, HttpRequest, char *data, size_t length, size_t remainingBytes
)> handler);
    void onHttpData(std::function<void(HttpResponse *, char *data, size_t length, size_t remainingBytes)> handler);
    void onHttpDisconnection(std::function<void(HttpSocket<isServer> *)> handler);
    void onCancelledHttpRequest(std::function<void(HttpResponse *)> handler);
    void onHttpUpgrade(std::function<void(HttpSocket<isServer> *, HttpRequest)> handler);
```
可以看到，象onConnection, onMessage OnPing 等，其参数是一个函数handler，一个回调函数。

