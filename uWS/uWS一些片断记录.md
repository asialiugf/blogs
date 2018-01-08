### vector 查找
```c
#include <algorithm>
#include <vector>

    auto result = find(this->cs.begin(), this->cs.end(), ws);     //查找 ws
    if(result == this->cs.end()) {    //没找到
      std::cout << "Not found" << std::endl;
    } else {
      std::cout << "Yes" << std::endl;  //找到了
      (*result)->send("HHHH from assiInit client \n"); //注意， *result要括起来。
    }
```


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
#### getDefaultGroup 这一句在其它地方调用

比如，getDefaultGroup<uWS::SERVER>().close()在onMessage或者在onPong等地方调用后，onDisconnection<SERVER>会收到相应的信息。

这时，如果在 onDisconnection<SERVER> 又调用了 getDefaultGroup<uWS::SERVER>().close()，那么相当于这一句被调用两次，
那么就会出现：Segmentation fault。
    
请注意下面的代码，在onPong调用getDefaultGroup<uWS::SERVER>().close()时，会造成 onDisconnection<SERVER>  收到信息，而  onDisconnection<SERVER>中又调用了getDefaultGroup<uWS::SERVER>().close()一次，于是出现 Segmentation fault。
    
### 为什么？
调用getDefaultGroup<uWS::SERVER>().close()时，为什么 会造成 onDisconnection<SERVER>  收到信息 ？
要查一下原代码。 留个作业。
     
```c++
h.getDefaultGroup<uWS::SERVER>().close();
```

```c++
riddle@d:~/x/src$ cat uq_data_server.cpp 
#include <uWS/uWS.h>
#include <unistd.h>
#include <iostream>
#include <map>
//#include <uuid/uuid.h>

#pragma comment(lib, "uWS.lib")


int main()
{
    uWS::Hub h;
    int pongs =0;
    int pings =0;
    int count = 0;
    std::map<int, uWS::WebSocket<uWS::SERVER>*> rr ;
    //uWS::WebSocket<uWS::SERVER>* rr[100];
    //uWS::WebSocket<uWS::SERVER>* ws0;

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
        //todo  收到客户端关闭的信息时，需要将这个client的相关数据进行清理。
        std::cout << "recieved client close info !!\n";
        //h.getDefaultGroup<uWS::SERVER>().close();
                //exit(-1);
    });


    h.onMessage([&rr,&h](uWS::WebSocket<uWS::SERVER> *ws, char *message, size_t length, uWS::OpCode opCode) {
        char tmp[16];
        memcpy(tmp, message, length);
        tmp[length] = '\0';
        printf("Client receive: %s\n", tmp);
        if(tmp[0] == '0') {
            std::cout << "send to client 0 \n" ;
            ws->send("--000000");
            rr[0]->send("--111111");
            rr[0]->send("--111111");
            rr[0]->send("--111111");
            rr[0]->send("--111111");
        } else if(tmp[0] == '1') {
            std::cout << "send to client 1 \n" ;
            rr[1]->send("--111111");
            rr[1]->send("--111111");
            rr[1]->send("--111111");
            rr[1]->send("--111111");
        }
        std::cout << "send end!!!!!!!!!! \n" ;
        //h.getDefaultGroup<uWS::SERVER>().close();
    });


    h.onPong([&pings,&pongs, &h](uWS::WebSocket<uWS::SERVER> *ws, char *message, size_t length) {
        std::cout << "PONG" << std::endl;
        pongs++;
        if(pongs == 3) {
            if(pings != pongs) {
                std::cout << "FAILURE: mismatching ping/pongs" << std::endl;
                //exit(-1);
            }
            h.getDefaultGroup<uWS::SERVER>().close();
                    //exit(-1);
        }
    });

    h.getDefaultGroup<uWS::SERVER>().startAutoPing(1000);


    bool k = h.listen(3001) ;
    if(!k) {
        std::cout << " listen error !!" << std::endl;
    }
    // --------------------------------------------------------------------------------------------

    h.run();
}
riddle@d:~/x/src$ 
```
