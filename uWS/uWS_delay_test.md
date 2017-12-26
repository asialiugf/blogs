### uWS延迟测试
```c++
riddle@d:~/uws/tests$ cat delay_test.cpp
#include <uWS/uWS.h>
#include <unistd.h>
#include <iostream>
#include <sys/time.h>

#pragma comment(lib, "uWS.lib")

int main()
{
    uWS::Hub h;

    struct  timeval start;
    struct  timeval end;
    //unsigned  long diff;


    gettimeofday(&start,NULL);

    // --------------------------------------------------------------------------------------------
    // 服务端接收到包后原封不动返回
    h.onConnection([](uWS::WebSocket<uWS::SERVER> *ws, uWS::HttpRequest req) {
        //ws->send("--server--");
        std::cout <<"Server onConnection send: --server--" << std::endl;
    });

    h.onMessage([&start](uWS::WebSocket<uWS::SERVER> *ws, char *message, size_t length, uWS::OpCode opCode) {
        char tmp[256];
        memcpy(tmp, message, length);
        tmp[length] = 0;
        printf("Server onMessage receive: %s\n", tmp);

        gettimeofday(&start,NULL);
        ws->send(message, length, opCode);
    });

    bool k = h.listen(3000) ;
    if(!k) {
        std::cout << " listen error !!" << std::endl;
    }
    // --------------------------------------------------------------------------------------------

    // 客户端连上后发送hello
    h.onConnection([](uWS::WebSocket<uWS::CLIENT> *ws, uWS::HttpRequest req) {
        ws->send("--client0000-----------------------------------------------------------------");
        std::cout <<"Client onConnection send: --client00--" << std::endl;
    });

    // 客户端打印接收到的消息
    h.onMessage([&start,&end](uWS::WebSocket<uWS::CLIENT> *ws, char *message, size_t length, uWS::OpCode opCode) {

        gettimeofday(&end,NULL);
        unsigned  long diff = 1000000 * (end.tv_sec-start.tv_sec)+ end.tv_usec-start.tv_usec;
        printf("thedifference is %ld\n",diff);

        char tmp[256];
        memcpy(tmp, message, length);
        tmp[length] = 0;
        printf("Client onMessage receive: %s\n", tmp);
        //usleep(1000);
        ws->send(message, length, opCode);

    });

    h.connect("ws://localhost:3000");
    // --------------------------------------------------------------------------------------------

    h.run();
}
riddle@d:~/uws/tests$ 
```

#### 测试结果如下：
从server发出到client收到，仅用了6微秒，还是很不错的。
```c
Client onMessage receive: --client0000-----------------------------------------------------------------
Server onMessage receive: --client0000-----------------------------------------------------------------
thedifference is 6
Client onMessage receive: --client0000-----------------------------------------------------------------
Server onMessage receive: --client0000-----------------------------------------------------------------
thedifference is 6
Client onMessage receive: --client0000-----------------------------------------------------------------
Server onMessage receive: --client0000-----------------------------------------------------------------
thedifference is 6
Client onMessage receive: --client0000-----------------------------------------------------------------
Server onMessage receive: --client0000-----------------------------------------------------------------
thedifference is 6
Client onMessage receive: --client0000-----------------------------------------------------------------
Server onMessage receive: --client0000-----------------------------------------------------------------
thedifference is 7
Client onMessage receive: --client0000-----------------------------------------------------------------
Server onMessage receive: --client0000-----------------------------------------------------------------
thedifference is 6
```
