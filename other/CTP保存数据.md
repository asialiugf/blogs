###  CTP保存数据

```c
void CMdSpi::OnRtnDepthMarketData(CThostFtdcDepthMarketDataField *pDepthMarketData)
{
	if(pDepthMarketData == nullptr)
		return;
	std::ostringstream fileName;
	fileName << DataPath << pDepthMarketData->InstrumentID;
	std::ofstream file;
	file.open(fileName.str(), std::ios::app);
	file << std::setprecision(14) << pDepthMarketData->TradingDay << "," << pDepthMarketData->LastPrice << ","
		 << pDepthMarketData->Volume << "," << pDepthMarketData->Turnover << "," << pDepthMarketData->OpenInterest << ","
		 << pDepthMarketData->UpdateTime << ":" << pDepthMarketData->UpdateMillisec / 100<< "," 
		 << pDepthMarketData->BidPrice1 << "," << pDepthMarketData->BidVolume1 << "," << pDepthMarketData->AskPrice1 << ","
		 << pDepthMarketData->AskVolume1 << "\n";
	file.close();
 	tickDelta += 1;
}
```
