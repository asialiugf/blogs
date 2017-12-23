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

##### 先定义了一个对象h.
```c++
uWS::Hub h;
```
##### 然后执行事件注册
这里，onConnection 的参数是一个 lambda表达式[]，也就是一个匿名函数。
```c++
    // 服务端接收到包后原封不动返回
    h.onConnection([](uWS::WebSocket<uWS::SERVER> *ws, uWS::HttpRequest req) {
        ws->send("-----No------");
    });

```
这个匿名函数，也可以用来捕获外面的变量，例如：
你可以在程序外面定义一个 ss, 当建立连接时，server有句柄保留下来（通过下面的 [&ss] , ss = ws;)
```c++
    uWS::WebSocket<uWS::SERVER> *ss;

    h.onConnection([&ss](uWS::WebSocket<uWS::SERVER> *ws, uWS::HttpRequest req) {
        ss = ws;
        ws->send("Hello");
    });
```
这样，在后面的程序中，可以使用，方法如下：
```c++
    h.onMessage([&ss](uWS::WebSocket<uWS::SERVER> *ws, char *message, size_t length, uWS::OpCode opCode) {
        ss->send(message, length, opCode); 
    });
```
即：在onMessage的回调函数中，捕获ss，然后使用它。

注意：对于server端的onConnection的回调函数的的参数 ws，每个client都不一样。也就是说，这个server在这里等着client来建立连接，都是使用的这个调用，但每个client建立连接时，产生的ws都不一样，和每个client是一对一的关系。
用户程序需要记录这个ws，以便将来向不同的client发送不同的数据。
比如：可以建立一个 std::vector< > 来记录不同的ws，也可用map，set。要看需求。

##### 再继续分析
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
再来看看 Group类
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
在Group.cpp里的定义如下：
```c++
template <bool isServer>
void Group<isServer>::onConnection(std::function<void (WebSocket<isServer> *, HttpRequest)> handler) {
    connectionHandler = handler;
}

template <bool isServer>
void Group<isServer>::onTransfer(std::function<void (WebSocket<isServer> *)> handler) {
    transferHandler = handler;
}

template <bool isServer>
void Group<isServer>::onMessage(std::function<void (WebSocket<isServer> *, char *, size_t, OpCode)> handler) {
    messageHandler = handler;
}
```
在这里，我们看到，当调用 h.onConnection时，只是简单地执行了 
```c++
    connectionHandler = handler;
```
即：把用户定义的 函数（见上面的分析所说的lamda表达式)，赋给了connectionHandler。

要注意，上面是基于 模板 template <bool isServer> ,其中，传入的是一个bool变量，对于 isServer，要么为真，要么假，就是，不是SERVER，就是CLIENT。所以，我们写回调函数时，第一个参数要指明是SERVER还是CLIENT.

##### 来看看connectionHandler是什么
其实，它是Group类的一个成员变量, 要注意，它是从uS::NodeData类继承过来. 在c++中，你可以将struct 等同于class。只是，如果你在里面的定义，如果不加任何的修饰（public,private,protected），那么它default是public,而在class里，默认是private。
```c++
struct WIN32_EXPORT Group : private uS::NodeData {
protected:
    friend struct Hub;
    friend struct WebSocket<isServer>;
    friend struct HttpSocket<false>;
    friend struct HttpSocket<true>;

    std::function<void(WebSocket<isServer> *, HttpRequest)> connectionHandler;
    std::function<void(WebSocket<isServer> *)> transferHandler;
    std::function<void(WebSocket<isServer> *, char *message, size_t length, OpCode opCode)> messageHandler;
    std::function<void(WebSocket<isServer> *, int code, char *message, size_t length)> disconnectionHandler;
    std::function<void(WebSocket<isServer> *, char *, size_t)> pingHandler;
    std::function<void(WebSocket<isServer> *, char *, size_t)> pongHandler;
    std::function<void(HttpSocket<isServer> *)> httpConnectionHandler;
    std::function<void(HttpResponse *, HttpRequest, char *, size_t, size_t)> httpRequestHandler;
    std::function<void(HttpResponse *, char *, size_t, size_t)> httpDataHandler;
    std::function<void(HttpResponse *)> httpCancelledRequestHandler;
    std::function<void(HttpSocket<isServer> *)> httpDisconnectionHandler;
    std::function<void(HttpSocket<isServer> *, HttpRequest)> httpUpgradeHandler;
```
#### 小结一下
经过分析，象 h.onConnection h.onMessage h.onPing等，只是简单地，初始化了 Hub h这个对象的成员变量，这些成员变量，是一些函数。



### 我们来看 h.run做了些什么
那这些成员变量(匿名函数)是如何被事件驱动的
Hub 的run继承了 Node::run。
```c++
Hub.h:    using uS::Node::run;
```
在Node.cpp中定义，相当简单，就是一个loop->run()。
```c++
void Node::run() {
    nodeData->tid = pthread_self();
    loop->run();
}
```



