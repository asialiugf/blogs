### CTP TICK结构
```c
struct CThostFtdcDepthMarketDataField								///深度行情
{
  char          TradingDay[9];          ///交易日
  char          InstrumentID[31];       ///合约代码
  char          ExchangeID[9];          ///交易所代码
  char          ExchangeInstID[31];     ///合约在交易所的代码
  double        LastPrice;              ///最新价
  double        PreSettlementPrice;     ///上次结算价
  double        PreClosePrice;          ///昨收盘
  double        PreOpeninterest;        ///昨持仓量
  double        OpenPrice;              ///今开盘
  double        HighestPrice;           ///最高价
  double        LowestPrice;            ///最低价
  int           Volume;                 ///数量
  double        Turnover;               ///成交金额
  double        OpenInterest;           ///持仓量
  double        ClosePrice;             ///今收盘
  double        SettlementPrice;        ///本次结算价
  double        UpperLimitPrice;        ///涨停板价
  double        LowerLimitPrice;        ///跌停板价
  char          UpdateTime[9];          ///最后修改时间
  int           UpdateMillisec;         ///最后修改毫秒
  double        BidPrice1;          	///申买价一
  int           BidVolume1;             ///申买量一
  double        AskPrice1;          	///申卖价一
  int           AskVolume1;             ///申卖量一
  double        BidPrice2;       	///申买价二
  int           BidVolume2;             ///申买量二
  double        AskPrice2;         	///申卖价二
  int           AskVolume2;             ///申卖量二
  double        BidPrice3;       	///申买价三
  int           BidVolume3;             ///申买量三
  double        AskPrice3;          	///申卖价三
  int           AskVolume3;             ///申卖量三
  double        BidPrice4;          	///申买价四
  int           BidVolume4;             ///申买量四
  double        AskPrice4;          	///申卖价四
  int           AskVolume4;             ///申卖量四
  double        BidPrice5;          	///申买价五
  int           BidVolume5;             ///申买量五
  double        AskPrice5;          	///申卖价五
  int           AskVolume5;             ///申卖量五
  double        AveragePrice;           ///当日均价
  char          ActionDay[9];           ///业务日期
};

struct CThostFtdcDepthMarketDataField								///深度行情
{
	char[9] 	TThostFtdcDateType			TradingDay;             	///交易日
	char[31]	TThostFtdcInstrumentIDType		InstrumentID;           	///合约代码
	char[9]		TThostFtdcExchangeIDType		ExchangeID;              	///交易所代码
	char[31]	TThostFtdcExchangeInstIDType		ExchangeInstID;   		///合约在交易所的代码
	double 		TThostFtdcPriceType			LastPrice;             		///最新价
	Double 		TThostFtdcPriceType			PreSettlementPrice; 		///上次结算价
	Double 		TThostFtdcPriceType			PreClosePrice; 			///昨收盘
	double 		TThostFtdcLargeVolumeType		PreOpenInterest; 		///昨持仓量
	Double 		TThostFtdcPriceType			OpenPrice; 			///今开盘
	Double 		TThostFtdcPriceType			HighestPrice; 			///最高价
	Double 		TThostFtdcPriceType			LowestPrice; 			///最低价
	Int 		TThostFtdcVolumeType			Volume; 			///数量
	double 		TThostFtdcMoneyType			Turnover; 			///成交金额
	double 		TThostFtdcLargeVolumeType		OpenInterest; 			///持仓量
	Double 		TThostFtdcPriceType			ClosePrice; 			///今收盘
	Double 		TThostFtdcPriceType			SettlementPrice; 		///本次结算价
	Double 		TThostFtdcPriceType			UpperLimitPrice; 		///涨停板价
	Double 		TThostFtdcPriceType			LowerLimitPrice; 		///跌停板价
	double 		TThostFtdcRatioType			PreDelta; 			///昨虚实度
	double 		TThostFtdcRatioType			CurrDelta; 			///今虚实度
	char[9]		TThostFtdcTimeType			UpdateTime; 			///最后修改时间
	int 		TThostFtdcMillisecType			UpdateMillisec; 		///最后修改毫秒
	Double 		TThostFtdcPriceType			BidPrice1; 			///申买价一
	Int 		TThostFtdcVolumeType			BidVolume1; 			///申买量一
	Double 		TThostFtdcPriceType			AskPrice1; 			///申卖价一
	Int 		TThostFtdcVolumeType			AskVolume1; 			///申卖量一
	Double 		TThostFtdcPriceType			BidPrice2; 			///申买价二
	Int 		TThostFtdcVolumeType			BidVolume2; 			///申买量二
	Double 		TThostFtdcPriceType			AskPrice2; 			///申卖价二
	Int 		TThostFtdcVolumeType			AskVolume2; 			///申卖量二
	Double 		TThostFtdcPriceType			BidPrice3; 			///申买价三
	Int 		TThostFtdcVolumeType			BidVolume3; 			///申买量三
	Double 		TThostFtdcPriceType			AskPrice3; 			///申卖价三
	Int 		TThostFtdcVolumeType			AskVolume3; 			///申卖量三
	Double 		TThostFtdcPriceType			BidPrice4; 			///申买价四
	Int 		TThostFtdcVolumeType			BidVolume4; 			///申买量四
	Double 		TThostFtdcPriceType			AskPrice4; 			///申卖价四
	Int 		TThostFtdcVolumeType			AskVolume4; 			///申卖量四
	Double 		TThostFtdcPriceType			BidPrice5; 			///申买价五
	int 		TThostFtdcVolumeType			BidVolume5; 			///申买量五
	Double 		TThostFtdcPriceType			AskPrice5; 			///申卖价五
	Int 		TThostFtdcVolumeType			AskVolume5; 			///申卖量五
	Double 		TThostFtdcPriceType			AveragePrice; 			///当日均价
	char[9]		TThostFtdcDateType			ActionDay; 			///业务日期
};



 
struct CThostFtdcDepthMarketDataField					///深度行情
{
	char[9] 	TThostFtdcDateType				TradingDay;                		///交易日
	char[31]	TThostFtdcInstrumentIDType			InstrumentID;           		///合约代码
	char[9]		TThostFtdcExchangeIDType			ExchangeID;               		///交易所代码
	char[31]	TThostFtdcExchangeInstIDType			ExchangeInstID;   			///合约在交易所的代码
	double 		TThostFtdcPriceType				LastPrice;             			///最新价
	Double 		TThostFtdcPriceType				PreSettlementPrice; 			///上次结算价
	Double 		TThostFtdcPriceType				PreClosePrice; 				///昨收盘
	double 		TThostFtdcLargeVolumeType			PreOpenInterest; 			///昨持仓量
	Double 		TThostFtdcPriceType				OpenPrice; 				///今开盘
	Double 		TThostFtdcPriceType				HighestPrice; 				///最高价
	Double 		TThostFtdcPriceType				LowestPrice; 				///最低价
	Int 		TThostFtdcVolumeType				Volume; 				///数量
	double 		TThostFtdcMoneyType				Turnover; 				///成交金额
	double 		TThostFtdcLargeVolumeType			OpenInterest; 				///持仓量
	Double 		TThostFtdcPriceType				ClosePrice; 				///今收盘
	Double 		TThostFtdcPriceType				SettlementPrice; 			///本次结算价
	Double 		TThostFtdcPriceType				UpperLimitPrice; 			///涨停板价
	Double 		TThostFtdcPriceType				LowerLimitPrice; 			///跌停板价
	double 		TThostFtdcRatioType				PreDelta; 				///昨虚实度
	double 		TThostFtdcRatioType				CurrDelta; 				///今虚实度
	char[9]		TThostFtdcTimeType				UpdateTime; 				///最后修改时间
	int 		TThostFtdcMillisecType				UpdateMillisec; 			///最后修改毫秒
	Double 		TThostFtdcPriceType				BidPrice1; 				///申买价一
	Int 		TThostFtdcVolumeType				BidVolume1; 				///申买量一
	Double 		TThostFtdcPriceType				AskPrice1; 				///申卖价一
	Int 		TThostFtdcVolumeType				AskVolume1; 				///申卖量一
	Double 		TThostFtdcPriceType				AveragePrice; 				///当日均价
	char[9]		TThostFtdcDateType				ActionDay; 				///业务日期
};



























{
    "instrument_id":    "a1805",
    "datetime": "2018-01-03 14:19:27.841000",
    "ask_price1":   3658,
    "ask_volume1":  32,
    "bid_price1":   3657,
    "bid_volume1":  117,
    "last_price":   3658,
    "highest":  3675,
    "lowest":   3631,
    "amount":   4272294700,
    "volume":   116992,
    "open_interest":    235180,
    "pre_open_interest":    233118,
    "pre_close":    3639,
    "open": 3639,
    "close":    "-",
    "lower_limit":  3454,
    "upper_limit":  3816,
    "average":  3651,
    "pre_settlement":   3635,
    "settlement":   "-",
    "status":   ""
} 
{
    "instrument_id":    "TF1806",
    "ask_price1":   96.645,
    "ask_volume1":  3,
    "bid_price1":   96.61,
    "bid_volume1":  1,
    "last_price":   96.63,
    "change":   -0.015,
    "change_percent":   -0.000155,
    "highest":  96.71,
    "lowest":   96.565,
    "volume":   58,
    "open_interest":    467,
    "pre_open_interest":    445,
    "pre_close":    96.585,
    "open": 96.565,
    "close":    "NaN",
    "lower_limit":  95.49,
    "upper_limit":  97.8,
    "average":  96.608,
    "pre_settlement":   96.645,
    "amount":   56032800,
    "settlement":   "NaN",
    "datetime": "2018-01-03 14:19:22.500000"
}













 
TradingDay;           
InstrumentID;         
ExchangeID;           
ExchangeInstID;   		
LastPrice;            
PreSettlementPrice; 	
PreClosePrice; 			
PreOpenInterest; 		
OpenPrice; 				
HighestPrice; 			
LowestPrice; 			
Volume; 				
Turnover; 				
OpenInterest; 			
ClosePrice; 			
SettlementPrice; 		
UpperLimitPrice; 		
LowerLimitPrice; 		
PreDelta; 				
CurrDelta; 				
UpdateTime; 			
UpdateMillisec; 		
BidPrice1; 				
BidVolume1; 			
AskPrice1; 				
AskVolume1; 			
BidPrice2; 				
BidVolume2; 			
AskPrice2; 				
AskVolume2; 	
BidPrice3; 		
BidVolume3; 	
AskPrice3; 		
AskVolume3; 	
BidPrice4; 		
BidVolume4; 	
AskPrice4; 		
AskVolume4; 	
BidPrice5; 		
BidVolume5; 	
AskPrice5; 		
AskVolume5; 	
AveragePrice; 
ActionDay; 		



 
create table tutorials_tbl(
   tutorial_id INT NOT NULL AUTO_INCREMENT,
   tutorial_title VARCHAR(100) NOT NULL,
   tutorial_author VARCHAR(40) NOT NULL,
   submission_date DATE,
   PRIMARY KEY ( tutorial_id )
);



 
{
    "instrument_id":    "TF1806",
    "ask_price1":   96.645,
    "ask_volume1":  3,
    "bid_price1":   96.61,
    "bid_volume1":  1,
    "last_price":   96.63,
    "change":   -0.015,
    "change_percent":   -0.000155,
    "highest":  96.71,
    "lowest":   96.565,
    "volume":   58,
    "open_interest":    467,
    "pre_open_interest":    445,
    "pre_close":    96.585,
    "open": 96.565,
    "close":    "NaN",
    "lower_limit":  95.49,
    "upper_limit":  97.8,
    "average":  96.608,
    "pre_settlement":   96.645,
    "amount":   56032800,
    "settlement":   "NaN",
    "datetime": "2018-01-03 14:19:22.500000"
}
```
