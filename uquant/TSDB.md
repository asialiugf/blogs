### Influxdb
Influxdb是一个开源的分布式时序、时间和指标数据库，使用go语言编写，无需外部依赖。
它有三大特性：

-- 时序性（Time Series）：与时间相关的函数的灵活使用（诸如最大、最小、求和等）；
-- 度量（Metrics）：对实时大量数据进行计算；
-- 事件（Event）：支持任意的事件数据，换句话说，任意事件的数据我们都可以做操作。
同时，它有以下几大特点：

- schemaless(无结构)，可以是任意数量的列；
- min, max, sum, count, mean, median 一系列函数，方便统计；
- Native HTTP API, 内置http支持，使用http读写；
- Powerful Query Language 类似sql；
- Built-in Explorer 自带管理工具。

