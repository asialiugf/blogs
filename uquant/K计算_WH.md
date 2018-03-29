### K计算方法

![](https://github.com/asialiugf/blogs/blob/master/uquant/k_calculte001.PNG)
### 【情形1 ( mark == 0 )】
    seg[i]->sn : 用于记录这个seg和下一个seg之间是否连续。为1表示不连续 比如 09:00:00--10:15:00   10:30:00--11:30:00 这两个seg之间不连续
    21:00:00--24:00:00  00:00:00--02:30:00 这两个seg之间连续。 

```c++
【A】：barE==tick <segE  内   ==>  a         s ?            {结束当前bar}
【B】：barE <tick <segE  内   ==>  a+        s ? or 2       {可能结束两个bar}

【C】：barE==tick==segE  外   ==>  c         s ?            {结束当前bar}
【D】：barE <tick==segE  外   ==>  c+        s ? + or  2    {一次结束两个bar}

【E】：barE<=segE <tick  外   ==>  e         s ?
【F】：tick <barE<=segE  外   ==>  e         s ?            {结束当前bar}   1：0点问题（A B，见下）  2：无效tick     4：第一个tick 


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
