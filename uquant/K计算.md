![](https://github.com/asialiugf/blogs/blob/master/uquant/k_calculte001.PNG)
### 情形1 ( mark == 0 )
```
【A】： barE == tick <  segE  { curBar 结束，newBar}
【B】： barE  < tick <  segE  { curBar 结束，newBar也可能结束，再new一个bar}
【C】： barE  < tick == segE  { curBar 结束， 此seg的最后一个bar也结束，在此seg之外，紧临，再new 一个bar}    除了多结束一个bar 以外，和下面一样 bar结束， seg也结束 
【D】： barE == tick == segE  { curBar 结束，在此seg之外，紧临，再new 一个bar }          和后面一样 bar结束， seg也结束 
【E】： tick > segE                          { curBar 结束，在此seg之外，while，再new 一个bar }   这种情况要处理 00:00:00 ----
情形【E】要修改， 如果tick 是 00:00:00 其实也在 barE 之外，因为barE 是 24:00:00。但 tick < segE
```
![](https://github.com/asialiugf/blogs/blob/master/uquant/k_calculte002.PNG)
### 情形2 ( mark >= 0 )
```
tick > barE segE    有可能属于 【B】 ， 例如  seg1 (21:00:00—22:00:00) seg2(22:30:00—24:00:00) seg3(00:00:00—01:00:00) 属于一个BAR
当 tick落于 seg2时，就出现这种情况。

【A】： segE  == tick  <  barE                                 { curseg 结束，}
【B】： segE  <   tick  <   barE                                 { curseg 结束，}
【C】： segE  <   tick  == barE                                 { curbar 结束，}
【D】： segE == tick  == barE                                 { curbar 结束，在此seg之外，紧临，再new 一个bar }
【E】： tick > barE                                                    { curbar结束，在此seg之外，while，再new 一个bar }
情形【E】要修改， 如果tick 是 00:00:00 其实也在 barE 之外，因为barE 是 24:00:00。但 tick < barE

```
### 情形3 ( mark <  0 )
