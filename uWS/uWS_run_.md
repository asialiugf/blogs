### uWS server端 run过程分析 

主循环程序在Epoll.cpp中实现。

- 针对每个fd会有一个Poll对象，严格来说，每个Poll对象会有一个fd。
- server进行listen时，会监听一个fd，当每个client连接上来时，会产生一个新的fd。
- 针对一个loop，会有一个epoll_wait,epfd
- 当有新的fd产生时，就会把它加入到epfd中，见下面 start()程序
- 当client离开时，就会把这个fd从epfd中DEL掉，见下面的stop()

```c
        int numFdReady = epoll_wait(epfd, readyEvents, 1024, delay);
```

```c
    void start(Loop *loop, Poll *self, int events)
    {
        std::cout << " Loop::start()   ====> EPOLL_CTL_ADD \n";
        epoll_event event;
        event.events = events;
        event.data.ptr = self;
        epoll_ctl(loop->epfd, EPOLL_CTL_ADD, state.fd, &event);
    }
```
```c
    void stop(Loop *loop)
    {
        epoll_event event;
        std::cout << " Loop::stop()   ====> EPOLL_CTL_DEL \n";
        epoll_ctl(loop->epfd, EPOLL_CTL_DEL, state.fd, &event);
    }
```
#### 具体的过程如下：
#### 首先，server 进行 listen动作
epoll_wait(epfd)中，只有它自己的listen fd被放入 epfd中 进行监听。
```c
riddle@d:~/uws/tests$ ./server
 Epoll.h --> createLoop 



[[ Hub.cpp ]]  Hub::listen() ::::   ====> listen<onServerAccept> 
[[Poll.h]]  Poll::Poll() ====> loop->numPolls++; 
[[  Node.h ]]  Node::listen() ::::   ====> setCb(accept_poll_cb<A>) 
[[ Epoll.h ]]  Poll::setCb() ::::   ====> callbacks[cbHead++] = cb;
 Loop::start()   ====> EPOLL_CTL_ADD 
 Node.cpp -->  Node::run() !!! 
Enter into  Epoll.cpp Loop::run () 
 numPolls:: 1
 ---- while( numPolls ) :: 1


```
#### 当有新的客户端连接进来时
针对这个客户端会产生一个fd, 然后把这个fd一起加入到epfd中。
这样的话，在run()中，这个epoll_wait 就会监听这两个 fd

```c

+++++++++++++++++++++++++++++++++++++for begin+++++++++++++++++++++++++++++++++
[[Epoll.cpp]]  Loop::run()  :::: ====>  for (int i = 0; i < numFdReady; i++) 
before callbacks[] 
[[Epoll.cpp]]  Loop::run()  :::: ====> callbacks[poll->state.cbIndex] (...); 
[[Poll.h]]  Poll::Poll() ====> loop->numPolls++; 
[[  Node.h ]]  Node::accecpt_cb() :::: ====>  A(socket); 
[[ Hub.cpp ]]  Hub::onServerAccept() :::: ====>  httpSocket->setState<HttpSocket<SERVER>>(); 
[[ Socket.h]]  Poll::setState() ::::   ====> setCb(ioHandler<STATE>); 
[[ Epoll.h ]]  Poll::setCb() ::::   ====> callbacks[cbHead++] = cb;
 Loop::start()   ====> EPOLL_CTL_ADD 
after callbacks []
thread id is: 140686540146560
+++++++++++++++++++++++++++++++++++++for end+++++++++++++++++++++++++++++++++++
 ---- while( numPolls ) :: 2


+++++++++++++++++++++++++++++++++++++for begin+++++++++++++++++++++++++++++++++
[[Epoll.cpp]]  Loop::run()  :::: ====>  for (int i = 0; i < numFdReady; i++) 
before callbacks[] 
[[Epoll.cpp]]  Loop::run()  :::: ====> callbacks[poll->state.cbIndex] (...); 
[[ Socket.h]]  Poll::ioHandler() ::::   ====> STATE::onData((Socket *) p, nodeData->recvBuffer, length); 
[[ Socket.h]]  Poll::setState() ::::   ====> setCb(ioHandler<STATE>); 
[[ Epoll.h ]]  Poll::setCb() ::::   ====> callbacks[cbHead++] = cb;
Server onConnection send: --server--
after callbacks []
thread id is: 140686540146560
+++++++++++++++++++++++++++++++++++++for end+++++++++++++++++++++++++++++++++++
 ---- while( numPolls ) :: 2


```
#### 当又有新的客户端连接进来时
针对这个客户端会产生一个fd, 然后把这个fd也一起加入到epfd中。
这样的话，在run()中，这个epoll_wait 就会监听这三个 fd

```c


+++++++++++++++++++++++++++++++++++++for begin+++++++++++++++++++++++++++++++++
[[Epoll.cpp]]  Loop::run()  :::: ====>  for (int i = 0; i < numFdReady; i++) 
before callbacks[] 
[[Epoll.cpp]]  Loop::run()  :::: ====> callbacks[poll->state.cbIndex] (...); 
[[Poll.h]]  Poll::Poll() ====> loop->numPolls++; 
[[  Node.h ]]  Node::accecpt_cb() :::: ====>  A(socket); 
[[ Hub.cpp ]]  Hub::onServerAccept() :::: ====>  httpSocket->setState<HttpSocket<SERVER>>(); 
[[ Socket.h]]  Poll::setState() ::::   ====> setCb(ioHandler<STATE>); 
 Loop::start()   ====> EPOLL_CTL_ADD 
after callbacks []
thread id is: 140686540146560
+++++++++++++++++++++++++++++++++++++for end+++++++++++++++++++++++++++++++++++
 ---- while( numPolls ) :: 3


+++++++++++++++++++++++++++++++++++++for begin+++++++++++++++++++++++++++++++++
[[Epoll.cpp]]  Loop::run()  :::: ====>  for (int i = 0; i < numFdReady; i++) 
before callbacks[] 
[[Epoll.cpp]]  Loop::run()  :::: ====> callbacks[poll->state.cbIndex] (...); 
[[ Socket.h]]  Poll::ioHandler() ::::   ====> STATE::onData((Socket *) p, nodeData->recvBuffer, length); 
[[ Socket.h]]  Poll::setState() ::::   ====> setCb(ioHandler<STATE>); 
Server onConnection send: --server--
after callbacks []
thread id is: 140686540146560
+++++++++++++++++++++++++++++++++++++for end+++++++++++++++++++++++++++++++++++
 ---- while( numPolls ) :: 3


```
####  第三个 新的客户端连接进来时
针对这个客户端会产生一个fd, 然后把这个fd也一起加入到epfd中。
这样的话，在run()中，这个epoll_wait 就会监听这四个 fd （三个客户 +　自己的listen ）

```c

+++++++++++++++++++++++++++++++++++++for begin+++++++++++++++++++++++++++++++++
[[Epoll.cpp]]  Loop::run()  :::: ====>  for (int i = 0; i < numFdReady; i++) 
before callbacks[] 
[[Epoll.cpp]]  Loop::run()  :::: ====> callbacks[poll->state.cbIndex] (...); 
[[Poll.h]]  Poll::Poll() ====> loop->numPolls++; 
[[  Node.h ]]  Node::accecpt_cb() :::: ====>  A(socket); 
[[ Hub.cpp ]]  Hub::onServerAccept() :::: ====>  httpSocket->setState<HttpSocket<SERVER>>(); 
[[ Socket.h]]  Poll::setState() ::::   ====> setCb(ioHandler<STATE>); 
 Loop::start()   ====> EPOLL_CTL_ADD 
after callbacks []
thread id is: 140686540146560
+++++++++++++++++++++++++++++++++++++for end+++++++++++++++++++++++++++++++++++
 ---- while( numPolls ) :: 4


+++++++++++++++++++++++++++++++++++++for begin+++++++++++++++++++++++++++++++++
[[Epoll.cpp]]  Loop::run()  :::: ====>  for (int i = 0; i < numFdReady; i++) 
before callbacks[] 
[[Epoll.cpp]]  Loop::run()  :::: ====> callbacks[poll->state.cbIndex] (...); 
[[ Socket.h]]  Poll::ioHandler() ::::   ====> STATE::onData((Socket *) p, nodeData->recvBuffer, length); 
[[ Socket.h]]  Poll::setState() ::::   ====> setCb(ioHandler<STATE>); 
Server onConnection send: --server--
after callbacks []
thread id is: 140686540146560
+++++++++++++++++++++++++++++++++++++for end+++++++++++++++++++++++++++++++++++
 ---- while( numPolls ) :: 4



```
####  当有客户端离开时
针对这个客户端会产生一个fd, 然后把这个fd也一起加入到epfd中。
这样的话，在run()中，这个epoll_wait 就会监听这四个 fd （三个客户 +　自己的listen ）

会调用 EPOLL_CTL_DEL  epoll_ctl(loop->epfd, EPOLL_CTL_DEL, state.fd, &event);
从epfd将这个客户端fd移走

```c

+++++++++++++++++++++++++++++++++++++for begin+++++++++++++++++++++++++++++++++
[[Epoll.cpp]]  Loop::run()  :::: ====>  for (int i = 0; i < numFdReady; i++) 
before callbacks[] 
[[Epoll.cpp]]  Loop::run()  :::: ====> callbacks[poll->state.cbIndex] (...); 
 Loop::stop()   ====> EPOLL_CTL_DEL 
[[Epoll.h]]  Poll:: close()  ====>loop->closing.push_back({this, cb}); 
after callbacks []
thread id is: 140686540146560
+++++++++++++++++++++++++++++++++++++for end+++++++++++++++++++++++++++++++++++
 ---- while( numPolls ) :: 4
```
####  再有客户端离开 

```c
+++++++++++++++++++++++++++++++++++++for begin+++++++++++++++++++++++++++++++++
[[Epoll.cpp]]  Loop::run()  :::: ====>  for (int i = 0; i < numFdReady; i++) 
before callbacks[] 
[[Epoll.cpp]]  Loop::run()  :::: ====> callbacks[poll->state.cbIndex] (...); 
 Loop::stop()   ====> EPOLL_CTL_DEL 
[[Epoll.h]]  Poll:: close()  ====>loop->closing.push_back({this, cb}); 
after callbacks []
thread id is: 140686540146560
+++++++++++++++++++++++++++++++++++++for end+++++++++++++++++++++++++++++++++++
 ---- while( numPolls ) :: 3

```
####  再有客户端离开 

```c

+++++++++++++++++++++++++++++++++++++for begin+++++++++++++++++++++++++++++++++
[[Epoll.cpp]]  Loop::run()  :::: ====>  for (int i = 0; i < numFdReady; i++) 
before callbacks[] 
[[Epoll.cpp]]  Loop::run()  :::: ====> callbacks[poll->state.cbIndex] (...); 
 Loop::stop()   ====> EPOLL_CTL_DEL 
[[Epoll.h]]  Poll:: close()  ====>loop->closing.push_back({this, cb}); 
after callbacks []
thread id is: 140686540146560
+++++++++++++++++++++++++++++++++++++for end+++++++++++++++++++++++++++++++++++
 ---- while( numPolls ) :: 2

```
