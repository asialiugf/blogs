![](https://github.com/asialiugf/blogs/blob/master/uquant/k_calculte001.PNG)
### 情形1 ( mark == 0 )
```c
【A】：barE==tick <segE {newBar}
【B】：barE <tick <segE {newBar也可能结束，再new一个bar}
【C】：barE <tick==segE {此seg最后一个bar结束，此seg之外紧临再new一个bar} 除了多结束一个bar 以外，和下面一样 bar结束， seg也结束 
【D】：barE==tick==segE {，在此seg之外，紧临，再new 一个bar }          和后面一样 bar结束， seg也结束 
【E】：tick > segE barE {在此seg之外，while，再new 一个bar }   这种情况要处理 00:00:00 ----
情形【E】要修改， 如果tick 是 00:00:00 其实也在 barE 之外，因为barE 是 24:00:00。但 tick < segE
```
- 1  情形1 只会有 barB<barE 还是 curB<curE 还是 segB<segE，不可能出现因为过了0点以后，segB>segE barB>barE curB>curE的情况。
- 2  1【E】 表示tick落在了seg之外。因为0点问题，可能会出现 tick < segB segE的情况。
- 3  1【E】 还有一种情况就是 00:00:00 000 这种 ms<500的情形，要算到前一个bar里。
- 4  衔接问题： 周五晚盘 和 下周一的K柱衔接。
- 5  开盘问题： 20：59：59 .......
- 6  任意时候开机问题： ......
- 7  一个bar的结束问题： 比如在 14:59:52秒之后，就没有数据过来，那 以15:00:00结束的这个K bar如何结束 ？




![](https://github.com/asialiugf/blogs/blob/master/uquant/k_calculte002.PNG)
### 情形2 ( mark >= 0 )
```c
tick > barE segE    有可能属于 【B】 ， 例如  seg1 (21:00:00—22:00:00) seg2(22:30:00—24:00:00) seg3(00:00:00—01:00:00) 属于一个BAR
当 tick落于 seg2时，就出现这种情况。

【A】：segE==tick <barE { curseg 结束，} 0点：落在barE外
【B】：segE< tick <barE { curseg 结束，} 0点：落在barE外
【C】：segE< tick==barE { curbar 结束，}
【D】：segE==tick==barE { curbar 结束，}
【E】：tick > segE barE { curbar结束，在此seg之外，while，再new 一个bar 

情形【E】要修改， 如果tick 是 00:00:00 其实也在 barE 之外，因为barE 是 24:00:00。但 tick < barE

```
-  情形2 会出现 barE<barB的情况 跨0点的情形。

### 情形3 ( mark <  0 )
- curiX = iSegNum ;
- seg[idx]->mark = -1 ;
- cB,cE,barB,barE curB curE 均为 "\0\0\0...." ;

初始化后，第一个tick到来时，比较 curB curE，均比它大，即落入到 tick>curB tick>curE tick>segB tick>segE !!

```c
  // ------- 最后多申请一个 seg. -- 用于最开始第一个Kbar处理。 ------
  curiX = iSegNum ;
  //idx = iSegNum ;
  seg[idx] = (stSegment *)malloc(sizeof(stSegment)) ;
  see_memzero(seg[idx]->cB,9);
  see_memzero(seg[idx]->cE,9);
  see_memzero(seg[idx]->barB,9);
  see_memzero(seg[idx]->barE,9);
  seg[idx]->iB =0;
  seg[idx]->iE =0;
  seg[idx]->bariB =0;
  seg[idx]->bariE =0;
  seg[idx]->mark = -1;
  see_memzero(curB,9);
  see_memzero(curE,9);
  // ---------------------------- 初始化  bar0 bar1 ----------------
```
