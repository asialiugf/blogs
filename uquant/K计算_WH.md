### K计算方法
- 将所有的bar统一成 09:00:00--09:00:59  09:01:00--09:01:59 09:02:00--09:02:59 这种类型。
- 在seg的结束点， 修改成： 10:13:00--10:13:59   <10:14:00--10:15:00>   这里的 10:15:00 是一个seg的结束点，并且这个seg后面的seg不是连续的。
- 如果是连续的seg，比如：  seg1: 13:00:00--14:00:00  seg2：14:00:00--15:00:00 是连续的，那么在14:00:00的bar要写成：<13:59:00--13:59:59>
- 所有的判断，都是： if(tik>barE) ... 这样表示新的bar开始。
- 对于跨段的bar，其bar的结束点，也遵循最后的seg是否连续。如果不连续，barE包含当前时间点，如果连续，则不包含。
- 在MARK==0时 curB--curE ，和bar保持一致。
- 如果MARK>0,  curB--curE  则和seg保持一致，bar最后一个seg， curB--curE和bar保持一致。
 
#### 初始化
-  所有不连续的seg，结束 seg->cE 都是 整点 比如  09:00:00----10:15:00  <--不连续-->  10:30:00----11:30:00 （iE与cE保持一致)
-  所有连续的seg，结束 seg->cE 都是减1 比如  21:00:00----23:59:59  <--连续--> 00:00:00----02:30:00 （iE与cE保持一致)
-  跨段的 barB--barE 仍然是 整点

```c
805 i:9 iF:120 id:6:10, mk:1  sn:1  segBE:11:29:00-11:30:00 barBE:11:29:00--13:31:00  sgiBE:41340--41400  bariBE:41340--48660   barBxEx:6--7
805 i:9 iF:120 id:7:10, mk:2  sn:0  segBE:13:30:00-13:30:59 barBE:11:29:00--13:31:00  sgiBE:48600--48659  bariBE:41340--48660   barBxEx:6--7
```

![](https://github.com/asialiugf/blogs/blob/master/uquant/k_calculte001.PNG)

### 【情形1 ( mark == 0 )】
    seg[i]->sn : 用于记录这个seg和下一个seg之间是否连续。为1表示不连续 比如 09:00:00--10:15:00   10:30:00--11:30:00 这两个seg之间不连续
    21:00:00--24:00:00  00:00:00--02:30:00 这两个seg之间连续。 
    
    这种情况， cur == bar, 每个tick来时，如果   curB <= tik <= curE ，即update .  所以下面不用考虑 tick == barE的情况.

#### OK
```c++
【A】：barE <tick<=segE   内   ==>  a
【B】：barE <tick==segE   内   ==>  a
【C】：barE<=segE <tick   外   ==>  e
【D】：tick <barE<=segE   外   ==>  e  24:00:00

注： barE == tick 这种情况不用考虑， 会在 curB <= tick <= curE 处理掉。 （这里 curB == barB   curE == barE)
a:  while( next barB <=  tick <= next barE ) {  if( tick == next barE ) {} ==> 将会处理到 tick==segE的情况; }
```
#### 参考
```c++

【A】：barE <tick <segE  内   ==>  a+        s ? or 2       {可能结束两个bar}
【B】：barE <tick==segE  外   ==>  c+        s ? + or  2    {一次结束两个bar}

【C】：barE<=segE <tick  外   ==>  e         s ?
【D】：tick <barE<=segE  外   ==>  e         s ?            {结束当前bar}   1：0点问题（A B，见下）  2：无效tick     4：第一个tick 


a: Newbar      NextBar         (same seg)
a+: NewBar  +   while(bar)      (same seg)
c: if(SN)  update,  if(!SN) NextSeg, NextBar,
c+: if(SN)  NewBar,  if(!SN) NextSeg, NextBar,

e: while(seg) 


// 计算SendBar时的顺序：
???
???

```

- 4  衔接问题： 周五晚盘 和 下周一的K柱衔接。
- 5  开盘问题： 20：59：59 .......
- 6  任意时候开机问题： ......
- 7  一个bar的结束问题： 比如在 14:59:52秒之后，就没有数据过来，那 以15:00:00结束的这个K bar如何结束 ？




![](https://github.com/asialiugf/blogs/blob/master/uquant/k_calculte003.PNG)
### 【情形2 ( mark >= 0 )】

#### OK
```c++
segE <= barE : 
【A】：segE <tick<=barE    内  ==   n
【E】：segE<=barE< tick    外  ==>  e             s    tick> barE
【F】：tick <segE<=barE    外  ==>  e             s    tick< barE

segE > barE : 
【1】：tick > segE > barE           内  ==>  c                          图中seg2 
【2】：       segE > barE>= tick    内  ==>  c                          图中seg3 4 5 
【3】：       segE > tick > barE    外  ==>  e    s    tick> barE
```
请考虑 24:00:00 之后 不连续的情况，最后的tick将是 00:00:00 000   00:00:00 500,要并入 24:00:00的K线中

#### 参考
```c++
tick > barE segE    有可能属于 【B】 ， 例如  seg1 (21:00:00—22:00:00) seg2(22:30:00—24:00:00) seg3(00:00:00—01:00:00) 属于一个BAR
当 tick落于 seg2时，就出现这种情况。

segE <= barE : 
【A】：segE==tick <barE    内  ==   n
【B】：segE< tick <barE    内  ==>  n
【C】：segE< tick==barE    外  ==>  c             s    tick==barE
【D】：segE==tick==barE    外  ==>  c             s    tick==barE
【E】：segE<=barE< tick    外  ==>  e             s    tick> barE
【F】：tick <segE<=barE    外  ==>  e             s    tick< barE

segE > barE : 
【1】：tick = segE > barE           内  ==>  c                          图中seg1
【2】：tick > segE > barE           内  ==>  c                          图中seg2 
【3】：       segE > barE > tick    内  ==>  c                          图中seg3 4 5 
【4】：       segE > barE = tick    外  ==>  e    s    tick==barE
【5】：       segE > tick > barE    外  ==>  e    s    tick> barE

 

【1】：segE>barE  ( segE <= tick <= 24:00:00 || 00:00:00 < tick <= barE )  属于上面的 【A】【B】
【2】：segE>barE  ( tick>barE )


segE <= barE : 

【A】：segE==tick <barE    内  ==   c
【1】：tick= segE> barE    内  ==>  c                          图中seg1

【B】：segE< tick <barE    内  ==>  d
【2】：tick> segE> barE    内  ==>  d                          图中seg2 
【3】：segE> barE> tick    内  ==>  d                          图中seg3 4 5 

【C】：segE< tick==barE    外  ==>  a             s    tick==barE
【D】：segE==tick==barE    外  ==>  a             s    tick==barE
【4】：segE> barE==tick    外  ==>  a             s    tick==barE

【E】：segE<=barE< tick    外  ==>  b             s    tick> barE
【F】：tick <segE<=barE    外  ==>  b + 0?        s    tick< barE             0点问题   
【5】：segE >tick> barE    外  ==>  b             s    tick> barE


// 计算SendBar时的顺序：
【C】：segE< tick==barE    外  ==>  a             s    tick==barE
【D】：segE==tick==barE    外  ==>  a             s    tick==barE
【4】：segE> barE==tick    外  ==>  a             s    tick==barE

【5】：segE >tick> barE    外  ==>  b             s    tick> barE
【E】：segE<=barE< tick    外  ==>  b             s    tick> barE

【F】：tick <segE<=barE    外  ==>  b + 0?        s    tick< barE             0点问题   




情形【E】要修改， 如果tick 是 00:00:00 其实也在 barE 之外，因为barE 是 24:00:00。但 tick < barE

```
-  情形2 会出现 barE<barB的情况 跨0点的情形。
-  反向考虑，如果满足【A】【B】【C】【D】 属于正常情况。
-  如果出现 segE > barE， 并且跨段， 就必须将其和 情形1分开，并且要考虑0点问题。

### 【情形3 ( mark <  0 )】
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
