#### getDefaultGroup
通过捕获 Hub h,找到server然后可以关闭它。

一般来说，这里的SERVER端的 onDisconnection 是client 失联了，所以，这里应该处理 该 client相关的数据，而不应该把 自己给关掉，因为自己是SERVER。

```c++
    uWS::Hub h;
    int count = 0;
    std::map<int, uWS::WebSocket<uWS::SERVER>*> rr ;

    // --------------------------------------------------------------------------------------------
    // 服务端接收到包后原封不动返回
    h.onConnection([&count,&rr](uWS::WebSocket<uWS::SERVER> *ws, uWS::HttpRequest req) {
        //rr.insert(std::pair<int,uWS::WebSocket<uWS::SERVER>*>(count,ws));
        rr[count] = ws ;
        if(count == 0) {
            ws->send("0");
        } else if(count ==1) {
            ws->send("1");
        }
        std::cout << "websocket server:" << count << " online.\n" ;
        count++;
    });
    h.onDisconnection([&h](uWS::WebSocket<uWS::SERVER> *ws, int code, char *message, size_t length) {
        h.getDefaultGroup<uWS::SERVER>().close();
    });
```
