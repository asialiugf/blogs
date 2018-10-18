### mall
![](https://github.com/asialiugf/blogs/blob/master/image/mall_002.png) 

* 类目可以分多级，每级都会有图中ABCDE区
* A区点击三角，会覆盖D区 （同级展开，A区显示的名称)
* A区有名称（比如，当当网叫“全部商品分类”）
* B区和C区也可以有名称，但不显示。

#### 嵌套
* 是指在上级类目点击，打开后，出现下级类目，每级类目都可以有ABCDE

#### 同级组合
```
家居 /	家具 /	家装 /	厨具
男装 /	女装 /	童装 /	内衣
美妆 /	个护清洁 /	宠物
```

####  上下级同显

```
【电脑配件】
CPU / SSD / 硬盘
显示器 / 显卡组装 / 电脑机箱

```
#### 展开同级， 展开下级


## 【导航栏 表设计】

### level
导航栏 分级，一般情况分为三级， 第一级，主页面， 第二级（家电、电子产品。。。） 第三级。。。
```
ID  name 
1   level one
2   level two
3   level three
4   level four

```
### 【area】 每级分为5个区，见上图
```
ID  name
A   area A
B   area B
C   area C
D   arec D
E   area E

```
### 【level_area】 每个  级别_区   对应的名称，以及是否显示名称，    是否展开
```
ID       name           level_ID      area_ID       display       spread 
0001    “全部商品分类”         1        A              Y             N
0002    "B区导航栏”            1        B              N             Y
0004    “A区导航栏”            2        A              Y             N
0005    “B区导航栏”            2        B              Y             Y
0006    “D区图书分类”          2        C              Y             Y
0007    “图书分类”             2        D              Y             Y
0008    “E区”                 2        E              Y             Y
。
。
。
。
```
### 【category】 真正的分类
```
ID        name     level_area_ID    group       url
00001     家居        0006(2D区）     1
00002     家装        0006           1
00003     厨具        0006           1
00004     男装        0006           2
00005     女装        0006           2
00006     童装        0006           2
00007     内衣        0006           2
00008     美妆        0006           3
00009     个护清洁    0006            3
00010     宠物        0006           3
.
.
.

```








