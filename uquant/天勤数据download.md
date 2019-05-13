https://doc.shinnytech.com/pysdk/latest/reference/tqsdk.tools.download.html


tqsdk.tools.downloader - 数据下载工具¶
class tqsdk.tools.downloader.DataDownloader(api, symbol_list, dur_sec, start_dt, end_dt, csv_file_name)
历史数据下载器, 输出到csv文件

多合约按时间横向对齐

创建历史数据下载器实例

Args:
api (TqApi): TqApi实例，该下载器将使用指定的api下载数据

symbol_list (str/list of str): 需要下载数据的合约代码，当指定多个合约代码时将其他合约按第一个合约的交易时间对齐

dur_sec (int): 数据周期，以秒为单位。例如: 1分钟线为60,1小时线为3600,日线为86400,Tick数据为0

start_dt (date/datetime): 起始时间, 如果类型为 date 则指的是交易日, 如果为 datetime 则指的是具体时间点

end_dt (date/datetime): 结束时间, 如果类型为 date 则指的是交易日, 如果为 datetime 则指的是具体时间点

csv_file_name (str): 输出csv的文件名

Example:

from datetime import datetime
from contextlib import closing
from tqsdk import TqApi, TqSim
from tqsdk.tools import DataDownloader

api = TqApi(TqSim())
download_tasks = {}
# 下载从 2018-01-01 到 2018-09-01 的 SR901 日线数据
download_tasks["SR_daily"] = DataDownloader(api, symbol_list="CZCE.SR901", dur_sec=24*60*60,
                    start_dt=date(2018, 1, 1), end_dt=date(2018, 9, 1), csv_file_name="SR901_daily.csv")
# 下载从 2017-01-01 到 2018-09-01 的 rb主连 5分钟线数据
download_tasks["rb_5min"] = DataDownloader(api, symbol_list="KQ.m@SHFE.rb", dur_sec=5*60,
                    start_dt=date(2017, 1, 1), end_dt=date(2018, 9, 1), csv_file_name="rb_5min.csv")
# 下载从 2018-01-01凌晨6点 到 2018-06-01下午4点 的 cu1805,cu1807,IC1803 分钟线数据，所有数据按 cu1805 的时间对齐
# 例如 cu1805 夜盘交易时段, IC1803 的各项数据为 N/A
# 例如 cu1805 13:00-13:30 不交易, 因此 IC1803 在 13:00-13:30 之间的K线数据会被跳过
download_tasks["cu_min"] = DataDownloader(api, symbol_list=["SHFE.cu1805", "SHFE.cu1807", "CFFEX.IC1803"], dur_sec=60,
                    start_dt=datetime(2018, 1, 1, 6, 0 ,0), end_dt=datetime(2018, 6, 1, 16, 0, 0), csv_file_name="cu_min.csv")
# 下载从 2018-05-01凌晨0点 到 2018-06-01凌晨0点 的 T1809 盘口Tick数据
download_tasks["T_tick"] = DataDownloader(api, symbol_list=["CFFEX.T1809"], dur_sec=0,
                    start_dt=datetime(2018, 5, 1), end_dt=datetime(2018, 6, 1), csv_file_name="T1809_tick.csv")
# 使用with closing机制确保下载完成后释放对应的资源
with closing(api):
    while not all([v.is_finished() for v in download_tasks.values()]):
        api.wait_update()
        print("progress: ", { k:("%.2f%%" % v.get_progress()) for k,v in download_tasks.items() })
is_finished()
判断是否下载完成

Returns:
bool: 如果数据下载完成则返回 True, 否则返回 False.
get_progress()
获得下载进度百分比

Returns:
float: 下载进度,100表示下载完成


```python
from datetime import datetime
from contextlib import closing
from tqsdk import TqApi, TqSim
from tqsdk.tools import DataDownloader

api = TqApi(TqSim())
download_tasks = {}

download_tasks["T_tick"] = DataDownloader(api, symbol_list=["CFFEX.T1509"], dur_sec=0,
                    start_dt=datetime(2013, 5, 1), end_dt=datetime(2019, 6, 1), csv_file_name="T1509_tick.csv")
# 使用with closing机制确保下载完成后释放对应的资源
with closing(api):
    while not all([v.is_finished() for v in download_tasks.values()]):
        api.wait_update()
        print("progress: ", { k:("%.2f%%" % v.get_progress()) for k,v in download_tasks.items() })
```
