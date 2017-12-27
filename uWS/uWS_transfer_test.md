### uWS_transfer_test.md 



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
