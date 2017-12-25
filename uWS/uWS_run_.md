### uWS server端 run过程分析 

主循环程序在Epoll.cpp中实现。

- 针对每个fd会有一个Poll对象，严格来说，每个Poll对象会有一个fd。
- server进行listen时，会监听一个fd，当每个client连接上来时，会产生一个新的fd。
- 针对一个loop，会有一个

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
