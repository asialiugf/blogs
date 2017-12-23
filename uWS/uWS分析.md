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

各种Handler都在下面：

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
在Node.h中，loop 是 Node类的成员变量 
```c++
protected:
    Loop *loop;
    NodeData *nodeData;
    std::recursive_mutex asyncMutex;

public:
    Loop *getLoop() {
        return loop;
    }
```
在Node的构造函数中，通过createLoop来初始化一个loop
```c++
Node::Node(int recvLength, int prePadding, int postPadding, bool useDefaultLoop) {
    nodeData = new NodeData;
    nodeData->recvBufferMemoryBlock = new char[recvLength];
    nodeData->recvBuffer = nodeData->recvBufferMemoryBlock + prePadding;
    nodeData->recvLength = recvLength - prePadding - postPadding;

    nodeData->tid = pthread_self();
    loop = Loop::createLoop(useDefaultLoop);
```
#### uWS如何选择网络库

在src/Backend.h中有以下定义：
``` c++
#ifndef BACKEND_H
#define BACKEND_H

// Default to Epoll if nothing specified and on Linux
// Default to Libuv if nothing specified and not on Linux
#ifdef USE_ASIO
#include "Asio.h"
#elif !defined(__linux__) || defined(USE_LIBUV)
#include "Libuv.h"
#else
#define USE_EPOLL
#include "Epoll.h"
#endif

#endif // BACKEND_H
```
在编译中，因为是在  uWebsockets目录下直接 make，ubuntu下所使用的是 Epoll。

#### Epoll.h中的createLoop()
简单地生成一个Loop对象
```c++
    static Loop *createLoop(bool defaultLoop = true) {
        std::cout << " Epoll.h --> createLoop \n" ;
        return new Loop(defaultLoop);
    }
```
Loop类如下：
其中run也在里面
```c++
struct Loop {
    int epfd;
    int numPolls = 0;
    bool cancelledLastTimer;
    int delay = -1;
    epoll_event readyEvents[1024];
    std::chrono::system_clock::time_point timepoint;
    std::vector<Timepoint> timers;
    std::vector<std::pair<Poll *, void (*)(Poll *)>> closing;

    void (*preCb)(void *) = nullptr;
    void (*postCb)(void *) = nullptr;
    void *preCbData, *postCbData;

    Loop(bool defaultLoop) {
        epfd = epoll_create1(EPOLL_CLOEXEC);
        timepoint = std::chrono::system_clock::now();
    }

    static Loop *createLoop(bool defaultLoop = true) {
        std::cout << " Epoll.h --> createLoop \n" ;
        return new Loop(defaultLoop);
    }

    void destroy() {
        ::close(epfd);
        delete this;
    }

    void run();

    int getEpollFd() {
        return epfd;
    }
};

```
在 Loop::run()中
有以下一段：
```c++
        for (int i = 0; i < numFdReady; i++) {
            Poll *poll = (Poll *) readyEvents[i].data.ptr;
            int status = -bool(readyEvents[i].events & EPOLLERR);
            callbacks[poll->state.cbIndex](poll, status, readyEvents[i].events);
        }
```
即执行真正的callbacks[]

在Epoll.cpp的开头，定了callbacks[]
```c++
namespace uS {

// todo: remove this mutex, have callbacks set at program start
std::recursive_mutex cbMutex;
void (*callbacks[16])(Poll *, int, int);
int cbHead = 0;
```
在Epoll.h中
```c++
extern std::recursive_mutex cbMutex;
extern void (*callbacks[16])(Poll *, int, int);
extern int cbHead;
```
在Epoll.h中，有一个类 Poll，真正的回调函数的注册，是在这里完成的。这个Poll类中有一个 setCb()。
```c++
struct Poll {
protected:
    struct {
        int fd : 28; 
protected:
    struct {
        int fd : 28; 
        unsigned int cbIndex : 4;
    } state = {-1, 0};

    Poll(Loop *loop, uv_os_sock_t fd) {
        fcntl(fd, F_SETFL, fcntl(fd, F_GETFL, 0) | O_NONBLOCK);
        state.fd = fd;
        loop->numPolls++;
    }

    // todo: pre-set all of callbacks up front and remove mutex
    void setCb(void (*cb)(Poll *p, int status, int events)) {
        cbMutex.lock();
        state.cbIndex = cbHead;
        for (int i = 0; i < cbHead; i++) {
            if (callbacks[i] == cb) {
                state.cbIndex = i;
                break;
            }
        }
        if (state.cbIndex == cbHead) {
            callbacks[cbHead++] = cb;
        }
        cbMutex.unlock();
    }
    
```
对于以上的 setCb() 在三个地方出现。
- 1、 class Node 的 listen 里
- 2、 class Node 的 connect 里
- 3、 Socket.h 的 Socket:Poll 里

其中第部分，Socket类里面有一个成员函数 setState()，在这里做了 setCb动作，对于 onMessage onPing onPang 等都是在这里注册。
```c++
    template<class STATE>
    void setState()
    {
        if(ssl) {
            setCb(sslIoHandler<STATE>);
        } else {
            std::cout << "i\n----  no ssl befor setCB -----i\n";
            setCb(ioHandler<STATE>);
            std::cout << "i\n----  no ssl after setCB -----i\n";
        }
    }
```

在Hub.cpp里，Hub的成员方法有两个地方调用了 setState()
```c++
void Hub::onServerAccept(uS::Socket *s) {
    HttpSocket<SERVER> *httpSocket = new HttpSocket<SERVER>(s);
    delete s;

    httpSocket->setState<HttpSocket<SERVER>>();
    httpSocket->start(httpSocket->nodeData->loop, httpSocket, httpSocket->setPoll(UV_READABLE));
    httpSocket->setNoDelay(true);
    Group<SERVER>::from(httpSocket)->addHttpSocket(httpSocket);
    Group<SERVER>::from(httpSocket)->httpConnectionHandler(httpSocket);
}

void Hub::onClientConnection(uS::Socket *s, bool error) {
    HttpSocket<CLIENT> *httpSocket = (HttpSocket<CLIENT> *) s;

    if (error) {
        httpSocket->onEnd(httpSocket);
    } else {
        httpSocket->setState<HttpSocket<CLIENT>>();
        httpSocket->change(httpSocket->nodeData->loop, httpSocket, httpSocket->setPoll(UV_READABLE));
        httpSocket->setNoDelay(true);
        httpSocket->upgrade(nullptr, nullptr, 0, nullptr, 0, nullptr);
    }
}
```
在Hub listen() 里，也就是当执行 程序里的 h.listen()时，
onServerAccept被当作模板参数，传入。
```
bool Hub::listen(const char *host, int port, uS::TLS::Context sslContext, int options, Group<SERVER> *eh) {
    if (!eh) {
        eh = (Group<SERVER> *) this;
    }

    if (uS::Node::listen<onServerAccept>(host, port, sslContext, options, (uS::NodeData *) eh, nullptr)) {
        eh->errorHandler(port);
        return false;
    }
    return true;
}
```
同样，在 Hub connect() 里，也就是当执行 程序里的 h.connect()时，
onClientConnection 被当作模板参数，传入。
```c++
void Hub::connect(std::string uri, void *user, std::map<std::string, std::string> extraHeaders, int timeoutMs, Group<CLIENT> *eh) {
    if (!eh) {
        eh = (Group<CLIENT> *) this;
    }

    int port;
    bool secure;
    std::string hostname, path;

    if (!parseURI(uri, secure, hostname, port, path)) {
        eh->errorHandler(user);
    } else {
        HttpSocket<CLIENT> *httpSocket = (HttpSocket<CLIENT> *) uS::Node::connect<allocateHttpSocket, onClientConnection>(hostname.c_str(), port, secure, eh);

```
由上面的程序可以看到，onClientConnection 是由 Node::connect()来传入的。
在Node.h里有下面一段：
```c++
    template <uS::Socket *I(Socket *s), void C(Socket *p, bool error)>
    Socket *connect(const char *hostname, int port, bool secure, NodeData *nodeData) {
        Context *netContext = nodeData->netContext;

        addrinfo hints, *result;
        memset(&hints, 0, sizeof(addrinfo));
        hints.ai_family = AF_UNSPEC;
        hints.ai_socktype = SOCK_STREAM;
        if (getaddrinfo(hostname, std::to_string(port).c_str(), &hints, &result) != 0) {
            return nullptr;
        }

        uv_os_sock_t fd = netContext->createSocket(result->ai_family, result->ai_socktype, result->ai_protocol);
        if (fd == INVALID_SOCKET) {
            freeaddrinfo(result);
            return nullptr;
        }
        
        ::connect(fd, result->ai_addr, result->ai_addrlen);
        freeaddrinfo(result);

        SSL *ssl = nullptr;
        if (secure) {
            ssl = SSL_new(nodeData->clientContext);
            SSL_set_connect_state(ssl);
            SSL_set_tlsext_host_name(ssl, hostname);
        }

        Socket initialSocket(nodeData, getLoop(), fd, ssl);
        uS::Socket *socket = I(&initialSocket);

        std::cout << "\n--------22 -------------\n";
        socket->setCb(connect_cb<C>);
        socket->start(loop, socket, socket->setPoll(UV_WRITABLE));
        return socket;
    }
```
在以上 Node::connect()的最后，由下面的 setCb() 注册。
```c++
socket->setCb(connect_cb<C>);
```
