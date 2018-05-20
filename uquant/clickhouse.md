#编译安装
```c
https://github.com/yandex/ClickHouse
https://clickhouse.yandex/docs/en/development/build/
https://clickhouse.yandex/docs/en/getting_started/example_datasets/ontime/
```
```c++
grep -q sse4_2 /proc/cpuinfo && echo "SSE 4.2 supported" || echo "SSE 4.2 not supported"
export THREADS=$(grep -c ^processor /proc/cpuinfo)

apt-get install software-properties-common
apt-add-repository ppa:ubuntu-toolchain-r/test
apt-get update
apt-get install gcc-7 g++-7
apt-get install devscripts dupload fakeroot debhelper

export CC=gcc-7
export CXX=g++-7

apt-get install libicu-dev libreadline-dev libmysqlclient-dev libssl-dev unixodbc-dev ninja-build

git clone -b stable --recursive https://github.com/yandex/ClickHouse.git

cd ClickHouse/
mkdir build
cd build
cmake ..
make -j $THREADS
make clickhouse
make install
cd ..
```

### 安装后的目录
```c++
/usr/local/etc/clickhouse-server# clickhouse-server ./config.xml 
Install the project...
-- Install configuration: "RELWITHDEBINFO"
-- Installing: /usr/local/include/re2/filtered_re2.h
-- Installing: /usr/local/include/re2/re2.h
-- Installing: /usr/local/include/re2/set.h
-- Installing: /usr/local/include/re2/stringpiece.h
-- Installing: /usr/local/lib/libre2.a
-- Installing: /usr/local/lib/libdouble-conversion.a
-- Installing: /usr/local/include/double-conversion/bignum.h
-- Installing: /usr/local/include/double-conversion/cached-powers.h
-- Installing: /usr/local/include/double-conversion/diy-fp.h
-- Installing: /usr/local/include/double-conversion/double-conversion.h
-- Installing: /usr/local/include/double-conversion/fast-dtoa.h
-- Installing: /usr/local/include/double-conversion/fixed-dtoa.h
-- Installing: /usr/local/include/double-conversion/ieee.h
-- Installing: /usr/local/include/double-conversion/strtod.h
-- Installing: /usr/local/include/double-conversion/utils.h
-- Installing: /usr/local/lib/cmake/double-conversion/double-conversionConfig.cmake
-- Installing: /usr/local/lib/cmake/double-conversion/double-conversionConfigVersion.cmake
-- Installing: /usr/local/lib/cmake/double-conversion/double-conversionTargets.cmake
-- Installing: /usr/local/lib/cmake/double-conversion/double-conversionTargets-relwithdebinfo.cmake
-- Installing: /usr/local/lib/libz.so.1.2.11.zlib-ng
-- Installing: /usr/local/lib/libz.so.1
-- Installing: /usr/local/lib/libz.so
-- Installing: /usr/local/lib/libz.a
-- Installing: /usr/local/include/zconf.h
-- Installing: /usr/local/include/zlib.h
-- Installing: /usr/local/share/man/man3/zlib.3
-- Installing: /usr/local/share/pkgconfig/zlib.pc
-- Installing: /usr/local/lib/cmake/RdKafka/RdKafkaConfig.cmake
-- Installing: /usr/local/lib/cmake/RdKafka/RdKafkaTargets.cmake
-- Installing: /usr/local/lib/cmake/RdKafka/RdKafkaTargets-relwithdebinfo.cmake
-- Installing: /usr/local/share/licenses/librdkafka/LICENSES.txt
-- Installing: /usr/local/lib/librdkafka.a
-- Installing: /usr/local/include/librdkafka/rdkafka.h
-- Installing: /usr/local/lib/librdkafka++.a
-- Installing: /usr/local/include/librdkafka/rdkafkacpp.h
-- Installing: /usr/local/lib/cmake/Poco/PocoConfig.cmake
-- Installing: /usr/local/lib/cmake/Poco/PocoConfigVersion.cmake
-- Installing: /usr/local/bin/corrector_utf8
-- Installing: /usr/local/bin/config-processor
-- Installing: /usr/local/bin/clickhouse-zookeeper-cli
-- Installing: /usr/local/bin/clickhouse-report
-- Installing: /usr/local/etc/clickhouse-client/config.xml
-- Installing: /usr/local/bin/clickhouse-server
-- Installing: /usr/local/bin/clickhouse-client
-- Installing: /usr/local/bin/clickhouse-local
-- Installing: /usr/local/bin/clickhouse-benchmark
-- Installing: /usr/local/bin/clickhouse-performance-test
-- Installing: /usr/local/bin/clickhouse-extract-from-config
-- Installing: /usr/local/bin/clickhouse-compressor
-- Installing: /usr/local/bin/clickhouse-format
-- Installing: /usr/local/bin/clickhouse-copier
-- Installing: /usr/local/bin/clickhouse
-- Set runtime path of "/usr/local/bin/clickhouse" to ""
-- Installing: /usr/local/bin/clickhouse-clang
-- Installing: /usr/local/bin/clickhouse-lld
-- Installing: /usr/local/etc/clickhouse-server/config.xml
-- Installing: /usr/local/etc/clickhouse-server/users.xml
-- Installing: /usr/local/bin/clickhouse-test
-- Installing: /usr/local/bin/clickhouse-test-server
-- Installing: /usr/local/share/clickhouse-test/queries
-- Installing: /usr/local/share/clickhouse-test/queries/shell_config.sh
-- Installing: /usr/local/share/clickhouse-test/queries/0_stateless
-- Installing: /usr/local/share/clickhouse-test/queries/0_stateless/00213_multiple_global_in.sql
-- Installing: /usr/local/share/clickhouse-test/queries/0_stateless/00292_parser_tuple_element.reference
-- Installing: /usr/local/share/clickhouse-test/queries/0_stateless/00183_skip_unavailable_shards.sql
-- Installing: /usr/local/share/clickhouse-test/queries/0_stateless/00322_disable_checksumming.sh
-- Installing: /usr/local/share/clickhouse-test/queries/0_stateless/00294_shard_enums.reference
...
...
-- Installing: /usr/local/share/clickhouse-test/external_dictionaries/reference/Date.reference
-- Installing: /usr/local/share/clickhouse-test/external_dictionaries/reference/StringOrDefault.reference
-- Installing: /usr/local/etc/clickhouse-server/server-test.xml
-- Installing: /usr/local/etc/clickhouse-client/client-test.xml

root@asialine:/usr/local/etc/clickhouse-server# ll
总用量 60
drwxr-xr-x 2 root root  4096 5月  20 20:47 ./
drwxr-xr-x 4 root root  4096 5月  20 20:18 ../
-rw-r--r-- 1 root root 15631 5月  20 20:18 config-preprocessed.xml
-rw-r--r-- 1 root root 15540 5月  20 19:44 config.xml
-rw-r--r-- 1 root root  3850 5月  20 19:44 server-test.xml
-rw-r--r-- 1 root root  4829 5月  20 20:18 users-preprocessed.xml
-rw-r--r-- 1 root root  4664 5月  20 19:44 users.xml

```
### 开启&测试
```c++
/usr/local/etc/clickhouse-server# clickhouse-server ./config.xml
# clickhouse-client
ClickHouse client version 1.1.54381.
Connecting to localhost:9000.
Connected to ClickHouse server version 1.1.54381.

bbb :) 
bbb :) 

bbb :) show databases;

SHOW DATABASES

┌─name────┐
│ default │
│ system  │
└─────────┘

2 rows in set. Elapsed: 0.004 sec. 

bbb :)
bbb :) select now();                        

SELECT now()

┌───────────────now()─┐
│ 2018-05-20 21:05:15 │
└─────────────────────┘

1 rows in set. Elapsed: 0.002 sec. 

bbb :) 
bbb :) select 1;

SELECT 1

┌─1─┐
│ 1 │
└───┘

1 rows in set. Elapsed: 0.002 sec. 

bbb :) select 10008

SELECT 10008

┌─10008─┐
│ 10008 │
└───────┘

1 rows in set. Elapsed: 0.002 sec. 

bbb :) 
bbb :) show tables;

SHOW TABLES

┌─name──────────┐
│ code_province │
│ ontime        │
└───────────────┘

2 rows in set. Elapsed: 0.003 sec. 

bbb :) 

bbb :) drop table ontime;

DROP TABLE ontime

Ok.

0 rows in set. Elapsed: 0.002 sec. 

bbb :) 
bbb :) 
bbb :) 
bbb :) CREATE TABLE `ontime` ( \
:-]   `Year` UInt16, \
:-]   `Quarter` UInt8, \
:-]   `Month` UInt8, \
:-]   `DayofMonth` UInt8, \
:-]   `DayOfWeek` UInt8, \
:-]   `FlightDate` Date, \
:-]   `UniqueCarrier` FixedString(7), \
:-]   `AirlineID` Int32, \
:-]   `Carrier` FixedString(2), \
:-]   `TailNum` String, \
:-]   `FlightNum` String, \
:-]   `OriginAirportID` Int32, \
:-]   `OriginAirportSeqID` Int32, \
:-]   `OriginCityMarketID` Int32, \
:-]   `Origin` FixedString(5), \
:-]   `OriginCityName` String, \
:-]   `OriginState` FixedString(2), \
:-]   `OriginStateFips` String, \
:-]   `OriginStateName` String, \
:-]   `OriginWac` Int32, \
:-]   `DestAirportID` Int32, \
:-]   `DestAirportSeqID` Int32, \
:-]   `DestCityMarketID` Int32, \
:-]   `Dest` FixedString(5), \
:-]   `DestCityName` String, \
:-]   `DestState` FixedString(2), \
:-]   `DestStateFips` String, \
:-]   `DestStateName` String, \
:-]   `DestWac` Int32, \
:-]   `CRSDepTime` Int32, \
:-]   `DepTime` Int32, \
:-]   `DepDelay` Int32, \
:-]   `DepDelayMinutes` Int32, \
:-]   `DepDel15` Int32, \
:-]   `DepartureDelayGroups` String, \
:-]   `DepTimeBlk` String, \
:-]   `TaxiOut` Int32, \
:-]   `WheelsOff` Int32, \
:-]   `WheelsOn` Int32, \
:-]   `TaxiIn` Int32, \
:-]   `CRSArrTime` Int32, \
:-]   `ArrTime` Int32, \
:-]   `ArrDelay` Int32, \
:-]   `ArrDelayMinutes` Int32, \
:-]   `ArrDel15` Int32, \
:-]   `ArrivalDelayGroups` Int32, \
:-]   `ArrTimeBlk` String, \
:-]   `Cancelled` UInt8, \
:-]   `CancellationCode` FixedString(1), \
:-]   `Diverted` UInt8, \
:-]   `CRSElapsedTime` Int32, \
:-]   `ActualElapsedTime` Int32, \
:-]   `AirTime` Int32, \
:-]   `Flights` Int32, \
:-]   `Distance` Int32, \
:-]   `DistanceGroup` UInt8, \
:-]   `CarrierDelay` Int32, \
:-]   `WeatherDelay` Int32, \
:-]   `NASDelay` Int32, \
:-]   `SecurityDelay` Int32, \
:-]   `LateAircraftDelay` Int32, \
:-]   `FirstDepTime` String, \
:-]   `TotalAddGTime` String, \
:-]   `LongestAddGTime` String, \
:-]   `DivAirportLandings` String, \
:-]   `DivReachedDest` String, \
:-]   `DivActualElapsedTime` String, \
:-]   `DivArrDelay` String, \
:-]   `DivDistance` String, \
:-]   `Div1Airport` String, \
:-]   `Div1AirportID` Int32, \
:-]   `Div1AirportSeqID` Int32, \
:-]   `Div1WheelsOn` String, \
:-]   `Div1TotalGTime` String, \
:-]   `Div1LongestGTime` String, \
:-]   `Div1WheelsOff` String, \
:-]   `Div1TailNum` String, \
:-]   `Div2Airport` String, \
:-]   `Div2AirportID` Int32, \
:-]   `Div2AirportSeqID` Int32, \
:-]   `Div2WheelsOn` String, \
:-]   `Div2TotalGTime` String, \
:-]   `Div2LongestGTime` String, \
:-]   `Div2WheelsOff` String, \
:-]   `Div2TailNum` String, \
:-]   `Div3Airport` String, \
:-]   `Div3AirportID` Int32, \
:-]   `Div3AirportSeqID` Int32, \
:-]   `Div3WheelsOn` String, \
:-]   `Div3TotalGTime` String, \
:-]   `Div3LongestGTime` String, \
:-]   `Div3WheelsOff` String, \
:-]   `Div3TailNum` String, \
:-]   `Div4Airport` String, \
:-]   `Div4AirportID` Int32, \
:-]   `Div4AirportSeqID` Int32, \
:-]   `Div4WheelsOn` String, \
:-]   `Div4TotalGTime` String, \
:-]   `Div4LongestGTime` String, \
:-]   `Div4WheelsOff` String, \
:-]   `Div4TailNum` String, \
:-]   `Div5Airport` String, \
:-]   `Div5AirportID` Int32, \
:-]   `Div5AirportSeqID` Int32, \
:-]   `Div5WheelsOn` String, \
:-]   `Div5TotalGTime` String, \
:-]   `Div5LongestGTime` String, \
:-]   `Div5WheelsOff` String, \
:-]   `Div5TailNum` String \
:-] ) ENGINE = MergeTree(FlightDate, (Year, FlightDate), 8192);

CREATE TABLE ontime
(
    Year UInt16, 
    Quarter UInt8, 
    Month UInt8, 
    DayofMonth UInt8, 
    DayOfWeek UInt8, 
    FlightDate Date, 
    UniqueCarrier FixedString(7), 
    AirlineID Int32, 
    Carrier FixedString(2), 
    TailNum String, 
    FlightNum String, 
    OriginAirportID Int32, 
    OriginAirportSeqID Int32, 
    OriginCityMarketID Int32, 
    Origin FixedString(5), 
    OriginCityName String, 
    OriginState FixedString(2), 
    OriginStateFips String, 
    OriginStateName String, 
    OriginWac Int32, 
    DestAirportID Int32, 
    DestAirportSeqID Int32, 
    DestCityMarketID Int32, 
    Dest FixedString(5), 
    DestCityName String, 
    DestState FixedString(2), 
    DestStateFips String, 
    DestStateName String, 
    DestWac Int32, 
    CRSDepTime Int32, 
    DepTime Int32, 
    DepDelay Int32, 
    DepDelayMinutes Int32, 
    DepDel15 Int32, 
    DepartureDelayGroups String, 
    DepTimeBlk String, 
    TaxiOut Int32, 
    WheelsOff Int32, 
    WheelsOn Int32, 
    TaxiIn Int32, 
    CRSArrTime Int32, 
    ArrTime Int32, 
    ArrDelay Int32, 
    ArrDelayMinutes Int32, 
    ArrDel15 Int32, 
    ArrivalDelayGroups Int32, 
    ArrTimeBlk String, 
    Cancelled UInt8, 
    CancellationCode FixedString(1), 
    Diverted UInt8, 
    CRSElapsedTime Int32, 
    ActualElapsedTime Int32, 
    AirTime Int32, 
    Flights Int32, 
    Distance Int32, 
    DistanceGroup UInt8, 
    CarrierDelay Int32, 
    WeatherDelay Int32, 
    NASDelay Int32, 
    SecurityDelay Int32, 
    LateAircraftDelay Int32, 
    FirstDepTime String, 
    TotalAddGTime String, 
    LongestAddGTime String, 
    DivAirportLandings String, 
    DivReachedDest String, 
    DivActualElapsedTime String, 
    DivArrDelay String, 
    DivDistance String, 
    Div1Airport String, 
    Div1AirportID Int32, 
    Div1AirportSeqID Int32, 
    Div1WheelsOn String, 
    Div1TotalGTime String, 
    Div1LongestGTime String, 
    Div1WheelsOff String, 
    Div1TailNum String, 
    Div2Airport String, 
    Div2AirportID Int32, 
    Div2AirportSeqID Int32, 
    Div2WheelsOn String, 
    Div2TotalGTime String, 
    Div2LongestGTime String, 
    Div2WheelsOff String, 
    Div2TailNum String, 
    Div3Airport String, 
    Div3AirportID Int32, 
    Div3AirportSeqID Int32, 
    Div3WheelsOn String, 
    Div3TotalGTime String, 
    Div3LongestGTime String, 
    Div3WheelsOff String, 
    Div3TailNum String, 
    Div4Airport String, 
    Div4AirportID Int32, 
    Div4AirportSeqID Int32, 
    Div4WheelsOn String, 
    Div4TotalGTime String, 
    Div4LongestGTime String, 
    Div4WheelsOff String, 
    Div4TailNum String, 
    Div5Airport String, 
    Div5AirportID Int32, 
    Div5AirportSeqID Int32, 
    Div5WheelsOn String, 
    Div5TotalGTime String, 
    Div5LongestGTime String, 
    Div5WheelsOff String, 
    Div5TailNum String
)
ENGINE = MergeTree(FlightDate, (Year, FlightDate), 8192)

Ok.

0 rows in set. Elapsed: 0.010 sec. 

bbb :) 

```
https://blog.csdn.net/m0_37739193/article/details/79612186


