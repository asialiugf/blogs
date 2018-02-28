# SetProcTitle 问题没有解决
现象：

1. 主进程必须调用 SetProcTitle 后，子进程 调用 SetProcTitle才有效。
2. 主进程调用 
```c
uBEE:: SetProcTitle("master ","DataServ: ");
```
这里的master是6个字符，那么子进程调用 SetProcTitle只能显示5个字符。 如果 master后面加一个空格， 子进程调用 SetProcTitle就能显示6个字符。

```c
riddle@asiamiao:~/uquant/src$ cat Dataserver.cpp 
#include "uBEE.h"
#include <thread>
#include <unistd.h>
#include <iostream>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <sys/prctl.h>

extern char **environ;
int ForkApi();
int ForkBck();
int ForkCtp();
int ForkSim();
uBEE::HubBck hb;
int main(int argc, char **argv)
{

  int rtn;
  int pid;
  uBEE::Daemon(1,0) ;

  hb.Init();
  hb.Start();

  pid = getpid();

  uBEE::SaveArgv(argc,argv);
  uBEE::InitSetProcTitle();
  uBEE:: SetProcTitle("master ","DataServ: ");


  rtn = ForkApi();
  sleep(1);
  for(int i=0; i<5; i++) {
    rtn = ForkBck();
    sleep(1);
  }
  sleep(1);
  rtn = ForkCtp();
  sleep(1);
  rtn = ForkSim();
  sleep(1);

  while(true) {
    sleep(100);
  }

}

int ForkApi()
{
  int rc;
  int pid;
  int i = 0;
  char ca_futures[1024];
  pid = fork();
  uBEE::HubApi hub;
  switch(pid) {
  case -1:
    return -1;

  case 0:
    pid = getpid();
    uBEE::InitSetProcTitle();
    uBEE:: SetProcTitle("HubApi","DataServ: ");
    hub.Init();
    hub.Start();
    break;
  default:
    break;
  }
  return 0;
}

int ForkBck()
{
  int rc;
  int pid;
  int i = 0;
  pid = fork();
  switch(pid) {
  case -1:
    return -1;

  case 0:
    pid = getpid();
    uBEE::InitSetProcTitle();
    uBEE:: SetProcTitle("HubBck:","DataServ: ");
    hb.Run();
    break;
  default:
    break;
  }
  return 0;
}

int ForkCtp()
{
  int rc;
  int pid;
  pid = fork();

  switch(pid) {
  case -1:
    return -1;
  case 0: {
    const char *brMsg = "This will be broadcasted!";
    size_t brMsgLength = strlen(brMsg);
    uBEE::HubCtp hub;
    pid = getpid();
    uBEE::InitSetProcTitle();
    uBEE:: SetProcTitle("HubCtp:","DataServ: ");
    hub.Init();
    sleep(2);

    std::thread t([&hub,&brMsg,&brMsgLength] {
      while(1)
      {
        //std::cout << " will broadcast !! " << std::endl;
        hub.sg->broadcast(brMsg, brMsgLength, uWS::OpCode::TEXT);
        sleep(1);
      }
    });  /* thread t */
    t.detach();
    hub.Start();

  }
  break;
  default:
    break;
  }
  return 0;
}

int ForkSim()
{
  int rc;
  int pid;
  int i = 0;
  pid = fork();
  switch(pid) {
  case -1:
    return -1;

  case 0: {
    uBEE::HubSim hub;
    pid = getpid();
    uBEE::InitSetProcTitle();
    uBEE:: SetProcTitle("HubSim:","DataServ: ");
    hub.Init();
    hub.Start();
  }
  break;
  default:
    break;
  }
  return 0;
}
@:~//
```
显示的结果如下：
```c
riddle   23959     1  0 12:41 ?        00:00:00 DataServ: master
riddle   23960 23959  0 12:41 ?        00:00:00 DataServ: HubApi
riddle   23961 23959  0 12:41 ?        00:00:00 DataServ: HubBck
riddle   23962 23959  0 12:41 ?        00:00:00 DataServ: HubBck
riddle   23965 23959  0 12:41 ?        00:00:00 DataServ: HubBck
riddle   23966 23959  0 12:41 ?        00:00:00 DataServ: HubBck
riddle   23967 23959  0 12:41 ?        00:00:00 DataServ: HubBck
riddle   23970 23959  0 12:41 ?        00:00:00 DataServ: HubCtp
riddle   23975 23959  0 12:41 ?        00:00:00 DataServ: HubSim
```
### 问题1
如果将主程序中的
```c
 uBEE:: SetProcTitle("master ","DataServ: ");
```
这一句去掉，则子程序显示成以下：
```c
riddle   24095     1  0 12:53 ?        00:00:00 ./dserver.x
riddle   24096 24095  0 12:53 ?        00:00:00 DataServ: 
riddle   24097 24095  0 12:53 ?        00:00:00 DataServ: 
riddle   24100 24095  0 12:53 ?        00:00:00 DataServ: 
riddle   24101 24095  0 12:53 ?        00:00:00 DataServ: 
riddle   24102 24095  0 12:53 ?        00:00:00 DataServ: 
riddle   24103 24095  0 12:53 ?        00:00:00 DataServ: 
riddle   24108 24095  0 12:53 ?        00:00:00 DataServ: 
riddle   24111 24095  0 12:54 ?        00:00:00 DataServ: 
```
### 问题2
如果将主程序中的
```c
 uBEE:: SetProcTitle("master ","DataServ: ");
```
master后面的空格去掉，改成：
```c
 uBEE:: SetProcTitle("master","DataServ: ");
```
则显示成以下：
```c
riddle   24177     1  0 12:56 ?        00:00:00 DataServ: master
riddle   24178 24177  0 12:56 ?        00:00:00 DataServ: HubAp
riddle   24179 24177  0 12:56 ?        00:00:00 DataServ: HubBc
riddle   24180 24177  0 12:56 ?        00:00:00 DataServ: HubBc
riddle   24181 24177  0 12:56 ?        00:00:00 DataServ: HubBc
riddle   24182 24177  0 12:56 ?        00:00:00 DataServ: HubBc
riddle   24183 24177  0 12:56 ?        00:00:00 DataServ: HubBc
riddle   24186 24177  0 12:56 ?        00:00:00 DataServ: HubCt
riddle   24189 24177  0 12:56 ?        00:00:00 DataServ: HubSi
```
子进程，无法显示全，少一个字符。

### ！！！问题没有解决，目前先将master后加一个空格，暂时绕过问题。

