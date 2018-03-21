### 情形1

【A】： barE  == tick  <  segE        { curBar 结束，newBar}
【B】： barE  <   tick  <   segE        { curBar 结束，newBar也可能结束，再new一个bar}
【C】： barE  <   tick  == segE       { curBar 结束， 此seg的最后一个bar也结束，在此seg之外，紧临，再new 一个bar}    除了多结束一个bar 以外，和下面一样 bar结束， seg也结束 
【D】： barE == tick  == segE        { curBar 结束，在此seg之外，紧临，再new 一个bar }          和后面一样 bar结束， seg也结束 
【E】： tick > segE                          { curBar 结束，在此seg之外，while，再new 一个bar }   这种情况要处理 00:00:00 ----
情形【E】要修改， 如果tick 是 00:00:00 其实也在 barE 之外，因为barE 是 24:00:00。但 tick < segE
