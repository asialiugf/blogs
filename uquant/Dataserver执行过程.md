
有4中类型的 dataserver:
- HubCtp  期货 实盘 
- HubApi  普通数据服务，策略程序通过各种get_xxx() 来这里取数据
- HubBck  backtest 回测
- HubSim  模拟盘

主程序开机时，会针对以上服务 fork出 不同的进程。

其中，对于 HubCtp 的流程如下：

1.  主线程 生成一个对象
```c
uBEE::HubCtp hub;
```

2.  另外生成一个线程，在这个线程中， 将这个 hub的 server group句柄 传递给 MdCtp()
```c
    std::thread t([&hub,&brMsg,&brMsgLength] {
      uBEE::MdCtp(hub.sg);
      /*
      while(1)
      {
        //std::cout << " will broadcast !! " << std::endl;
        hub.sg->broadcast(brMsg, brMsgLength, uWS::OpCode::TEXT);
        sleep(1);
      }
      */
    });  /* thread t */
```
这样，MdCtp收到的tick数据，就可以通过 hub.sg发送给 所有的客户端。

3.  主线程进入 loop. 接收客户端的连接请求。
```c
    t.detach();
    hub.Start();
```
