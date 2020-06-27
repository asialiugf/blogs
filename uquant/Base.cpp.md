```
[rabbit@VM-0-12-ubuntu ~/uquant/src]$ cat Base.cpp
#include "uBEE.h"
#include "SigSlot.h"
#include "Base.h"
#include "Bars.h"
#include "ApiFun.h"
#include <cjson/cJSON.h>
#include <iostream>
#include <thread>
#include <unistd.h>
#include <algorithm>
#include <vector>
#include <uuid/uuid.h>

namespace uBEE
{
using lSignal::Connection;
using lSignal::Signal;
using lSignal::Slot;

Base::Base():cs(100,nullptr)       // constructor  new thread for getting data APIs.
{
  Mode         = 4;                // default
  memset(DayB,'\0',9);
  memset(DayE,'\0',9);

  uuid_t uu;
  uuid_generate(uu);
  snprintf(Uuid,33, "%02X%02X%02X%02X%02X%02X%02X%02X%02X%02X%02X%02X%02X%02X%02X%02X",
           uu[0],uu[1],uu[2], uu[3], uu[4], uu[5], uu[6], uu[7],
           uu[8],uu[9],uu[10],uu[11],uu[12],uu[13],uu[14],uu[15]);
  Uuid[33] = '\0';

  data         = &DT;
  tick         = &TK;
  InstrumentID = data->InstrumentID;
  TradingDay   = data->TradingDay;
  ActionDay    = data->ActionDay;
  for(int i=0; i<50; ++i) {
    bars[i] = nullptr ;
  }

  /* 20200607
  api = new  FutureApiClient(grpc::CreateChannel(
                               "localhost:50051", grpc::InsecureChannelCredentials()));

  */

}


/*
  策略主进程 要调用 Base::FuInit()，针对每个需要处理的合约，初始化，并放在一个map中，M_Fu.insert()。
  M_Fu 是 Base的成员变量。
  input: || const std::map<std::string,std::vector<int>> *M  ||
  std::map< std::string, std::vector<int> > fuMap ; // 定义在每个策略主程序中 mnf.cpp
  fuMap["xu1801"] = {0,60,300,3600};
  fuMap["tu1801"] = {0,60,300,3600};
  fuMap["ru1805"] = {5,60,300,3600};
  fuMap["ru1809"] = {5,60};
  fuMap["xu1807"] = {60,19,300,3600};
  fuMap["zz1805"] = {5,15,30,60,300,3600,14401};
*/
void Base::FuInit(const std::map<std::string,std::vector<int>> *M)
{
  int i =0;
  for(auto it = (*M).begin(); it != (*M).end(); ++it) {    // for strategy ... future
    std::cout << it->first << std::endl;
    uBEE::sFuBo *fu = new uBEE::sFuBo(it->first) ;
    for(auto iter = it->second.cbegin(); iter != it->second.cend(); iter++) {
      std::cout << *iter << std::endl;
      i = 0;
      //std::map<std::string,int>::const_iterator itt;
      for(auto itt = M_FF.begin(); itt != M_FF.end(); ++itt) {    // all periods defined in M_FF <bars.h>
        if(*iter == itt->second) {
          fu->iP[i] = *iter ;
          fu->pBars[i] = new sOHLC() ;
          std::cout << "i:" << i << "  ip:" << *iter << std::endl;
          break;
        }
        i++;
      }
    }
    std::cout << "befor insert !!!\n" ;
    //M_Fu.insert(std::pair<std::string,uBEE::sFuBo>(it->first, *fu));
    MFuBo.insert(std::pair<std::string,uBEE::sFuBo *>(it->first, fu));
    //MFuBo.insert(std::pair<std::string,void*>(it->first, (void*)fu));
  }  // ---- (*M).begin()
}

//----------------------------------------
int Base::GetFrequencyIdx(int f)
{
  int i = 0;
  std::map<std::string,int>::const_iterator itt;
  for(itt = M_FF.begin(); itt != M_FF.end(); ++itt) {    // all periods defined in M_FF <bars.h>
    if(f == itt->second) {
      break;
    }
    i++;
  }
  if(itt == M_FF.end()) {
    return -1;
  }
  return i;
}
//----------------------------------------


void Base::Init()
{

}

void Base::onDaily(std::function<void()> handler)
{
  Base::onDailyHandler = handler;
}

void Base::onWeekly(std::function<void()> handler)
{
  Base::onWeeklyHandler = handler;
}

void Base::onMonthly(std::function<void()> handler)
{
  Base::onMonthlyHandler = handler;
}

void Base::onInit(std::function<void()> handler)
{
  Base::onInitHandler = handler;
}

/*
这里的 onTick()的参数 handler，实际上是一个函数。是在策略程序里，由用户自己定义的。 然后 赋给 Base::onTickHandler。 
*/
void Base::onTick(std::function<void(sTick *)> handler)
{
  Base::onTickHandler = handler;
}

void Base::onBars(std::function<void(sKbar *[],int)> handler)
{
  Base::onBarsHandler = handler;
}

void Base::onMessageInit()
{
  mainHub.onMessage([this](uWS::WebSocket<uWS::CLIENT> *ws, char *data, size_t length, uWS::OpCode opCode) {

    map<std::string,uBEE::sFuBo *>::iterator it;

    std::cout << "4rrrrrrrrrrrrrrrrrrrrrrrrrrr--------------------------" << std::endl;
    std::cout << "length: " << length <<std::endl;
    std::cout << "type: " << *((int*)data) <<std::endl;
    uBEE::ErrLog(1000,"kkkk",1,(const char*)data,length);

    // Bars.h sTick/sData ==> iType (*((int*)data)) ;
    switch(*((int*)data)) {
    case T_TICK:
      memcpy((char *)this->tick,data,length);
      it = this->MFuBo.find(this->tick->InstrumentID);
      if(it != this->MFuBo.end()) {
        this->fu = it->second ;
        this->onTickHandler(this->tick);  //在这里执行 策略程序的 onTick里面的代码。
      }
      break;
    case T_BARS:
      memcpy((char *)this->data,data,length);
      it = this->MFuBo.find(this->data->InstrumentID);
      if(it != this->MFuBo.end()) {
        //it->second
        //--------------- deal bars --------------------------
        int j = 0;
        for(int i=0; i<this->data->iN; ++i) {
          if(it->second->iP[ this->data->KK[i].iX ] == this->data->KK[i].iF) {
            this->bars[j] = (sKbar *)&this->data->KK[i] ;
            // todo 更新各种指标......
            //std::cout << this->bars[j]->cB << std::endl;
            it->second->pBars[ this->data->KK[i].iX ]->Insert(this->bars[j]);
            it->second->Update[ this->data->KK[i].iX ]();
            ++j ;
          }
        }
        if(j>0) {
          this->fu = it->second ;
          this->onBarsHandler(this->bars,j);  // 在这里执行 策略程序的 onBars里面的代码。 参数请看 策略程序里的定义：  “ BB->onBars([&](sKbar * bar[], int len) { ”
        }
      } // -----  end if(it != this->M_Fu.end())
      break;
    case T_UPDATE:
      memcpy((char *)this->data,data,length);
      it = this->MFuBo.find(this->data->InstrumentID);
      if(it != this->MFuBo.end()) {
        //it->second
        //--------------- deal bars --------------------------
        int j = 0;
        for(int i=0; i<this->data->iN; ++i) {
          if(it->second->iP[ this->data->KK[i].iX ] == this->data->KK[i].iF) {
            this->bars[j] = (sKbar *)&this->data->KK[i] ;
            // todo 更新各种指标......
            it->second->pBars[ this->data->KK[i].iX ]->Update(this->bars[j]);
            it->second->Update[ this->data->KK[i].iX ]();
            ++j ;
          }
        }
        if(j>0) {
          this->fu = it->second ;
          this->onBarsHandler(this->bars,j);
        }
      } // -----  end if(it != this->M_Fu.end())
      // todo !!
      // send to websocket web browser !
      break;
    default:
      break;
    }

    if(Mode == 2) {
      this->BckNextData();
    }
  });
}

void Base::Run()
{
  std::cout << " enter into Start !!\n";

  this->onInitHandler();  // 策略程序里面的 onInit() 在这里执行。

  mainHub.onConnection([this](uWS::WebSocket<uWS::CLIENT> *ws, uWS::HttpRequest req) {
    std::cout << "ppppppppppppp:" << (long) ws->getUserData() << std::endl;
    switch((long) ws->getUserData()) {
    case 1:
      this->cs[1] = ws;
      this->cw = ws;
      break;
    case 2: {
      this->cs[2] = ws;
      this->c_bck = ws;

      //map<std::string,uBEE::sFuBo *>::iterator it;

      /*
      std::string msg;
      for(auto iter = this->MFuBo.begin(); iter != this->MFuBo.end(); ++iter) {
        //std::string p = (char*)iter->first.c_str();  // char *Future, const char *pFile
        msg += iter->first ;
      }
      ws->send((char*)msg.c_str(), msg.length(), uWS::OpCode::BINARY);
      */

      std::cout << "================================================================\n";
      BckMsg *bckmsg = new BckMsg() ;
      int n = BckMsgInit(bckmsg);
      std::cout <<"n:"<<n<< std::endl;
      if(n>0) {
        int len = sizeof(BckMsg) - (256-n) * sizeof(BckFu) ;
        ws->send((char*)bckmsg, len, uWS::OpCode::BINARY);
      } else {
        std::cout << "no future !!\n" ;
      }
      delete bckmsg;

      break;
    }
    case 3:
      this->cs[3] = ws;
      this->c_ctp = ws;
      break;
    case 4:
      this->cs[4] = ws;
      this->c_sim = ws;
      break;
    default:
      std::cout << "FAILURE: " << ws->getUserData() << " should not connect!" << std::endl;
      exit(-1);
    }
    /*
    std::cout << "  befor send !!!================================================================\n";
    ws->send("hhhhhhhaaaaaaaaaa!!");
    std::cout << "  after send !!!================================================================\n";
    */
  });

  std::cout << " Base::Run   -------------- after onConnnection !!!!!!!!!!!!!!!!!\n";
  // 客户端打印接收到的消息
  Base::onMessageInit();
  std::cout << " Base::Run   -------------- after onConnnection 333333333333333333333\n";

  mainHub.onDisconnection([this](uWS::WebSocket<uWS::CLIENT> *ws, int code, char *message, size_t length) {
    auto result = find(this->cs.begin(), this->cs.end(), ws);     //查找 ws
    if(result == this->cs.end()) {    //没找到
      std::cout << "Not found" << std::endl;
    } else {
      std::cout << "YesYes!!" << std::endl;  //找到了
      (*result) = nullptr ;
      //ws->close();
    }
    std::cout << "code:"<< code << std::endl;
    //this->mainHub.getDefaultGroup<uWS::SERVER>().close();
    //this->mainHub.getDefaultGroup<uWS::CLIENT>().close();
    switch((long) ws->getUserData()) {
    case 1:
      break;
    case 5:
      mainHub.connect("ws://localhost:3004",(void *) 4);  //  data server HubSim  //模拟
      break;
    }
  });

  std::cout << " Base::Run   -------------- after onConnnection 44444444444444444444\n";

  switch(Mode) {
  case 1:
    break;
  case 2:
    mainHub.connect("ws://localhost:3002",(void *) 2);  //  data server HubBck  //回测
    break;
  case 3:
    mainHub.connect("ws://localhost:3003",(void *) 3);  //  data server HubCtp  //实盘
    break;
  case 4:
    mainHub.connect("ws://localhost:3004",(void *) 4);  //  data server HubSim  //模拟
    break;
  default:
    mainHub.connect("ws://localhost:3004",(void *) 4);  //  data server HubSim  //模拟
    break;
  }

  std::cout << " Base::Run   -------------- after onConnnection 55555555555555555555555555\n";

  //mainHub.connect("ws://localhost:3000",(void *) 1);  //  web server
  //mainHub.connect("ws://localhost:5000",(void *) 3);  //  trading server
  mainHub.run();
  std::cout << "after run \n";
// 程序何时结束 ？？
}

void Base::BckNextData()
{
  int iType = 1;
  c_bck->send((char*)&iType, sizeof(int), uWS::OpCode::BINARY);
}

int Base::BckMsgInit(BckMsg *bck_msg)
{
  bck_msg->iType = 0;
  memcpy(bck_msg->DayB,DayB,9);
  memcpy(bck_msg->DayE,DayE,9);
  memcpy(bck_msg->Uuid,Uuid,33);

  int i = 0;
  for(auto iter = MFuBo.begin(); iter != MFuBo.end(); ++iter) {
    memset(bck_msg->fubo[i].InstrumentID,'\0',31);
    memcpy(bck_msg->fubo[i].InstrumentID,(char*)iter->first.c_str(),iter->first.length());
    for(int j=0; j<50; ++j) {
      bck_msg->fubo[i].iP[j] = iter->second->iP[j] ;
    }
    i++;
  }
  bck_msg->iN = i;
  return i;
}

} //namespace
[rabbit@VM-0-12-ubuntu ~/uquant/src]$ 

``
