设计的主要思路：

### 数据提供引擎

数据引擎主要提供以下功能

- 基本数据，包括 金融市场的一些相关数据，比如公司财务数据、舆情数据、等。
- 历史数据， 期货、股票 历史数据，用于回测，模拟行情等。
- 实时行情数据，发送实时行情数据，存储实时行情数据，K线生成，基本的指标数据生成。

对于基本数据， 提供API接口，按需获取。
对于历史数据，提供两种方式，1、按需获取， 2、pub/sub方式，用于模拟行情。
实时行情数据，只提供publish的方式，供所有策略使用，采用pub/sub的方式 。

数据引擎的数据来源，可以从多个方向来。 用户也可以提供自己的测试数据。

数据引擎设计成独立的模块。

有三种方式：

1、 策略模块   < 数据引擎 >                                                      // 内嵌包含
2、 策略模块   <-------->   数据引擎                                             // 独立进程，通过ZMQ 联系
3、 熏略模块   <-------->   第三方msg中间件有独立的broker <-------->  数据引擎     // 可以分布式部署
其中，方案 2和3 可以远程部署，也可以本地部署。方案2、3 要能保证大规模数据请求（100万级请求？）

对于历史数据模拟回测，有两种方式：
1、一个策略请求，需要开一个线程，单独服务于这个策略。
2、可以象实时行情一样，一直播放，循环播放。一个数据引擎线程，服务于多个策略。

对于实时行情数据， 一个数据引擎线程，服务于多个策略。

有跨平台要求。（WIN linux MAC)

### 策略引擎

有以下功能：

- 执行策略
- 信号生成
- 执行交易 // 要不要支持人工干预？
- 信息输出（用于可视化展示），

设计成一个循环，从数据引擎取数据，执行ontick onbar之类的策略处理。
要支持多级别。即同时可以送1分钟和5分钟的的K线数据。

另外，如何将生成的数据，传给前端展示。需要一个单独的线程？

多开发语言支持。

### 交易模块
支持多服务商接入

### 管理模块

和前端交互，接收用户请求

策略引擎的启动和关闭

支持多用户（家庭多用户，10-20个？）

### 前端用户界面

1、websockets
2、QT
3、WIN 之类？


<--------------->

###  strategy

class StrategyBase {
     init(){
         env_init();
         connect_to_data_engin();
     };

     ontimer(){};

     ontick(){};
     onbar(){};

     run(){

         while(1){
            just_like_uv_loop();
             get_data()
             case tick:
             ontick();
             case bar:
             on bar();
         }
     };
     stop();
     
}


### manager

class manager {
    init()
    run();
}

main(){
    web_event_loop(){
        conection from web ?
        get websockets conection !!
        create_pthread(strategy_inspector);
    }


}

// strategy_inspector_pthread -----------------
strategy_inspector(){
    //websockets connection!!!
        onmessage(){
            case 1:
                // strategy pthread ------------------
                start(strategy1,ID=101)
            case 2:
                stop(strategy1,ID=101)
            case 3:
                all futures tick data?
            case 4:
                break;
        }
        ping_pong()

}












