### 关系数据库设计 

```c++
node_id     node_name     parrent_ids     child_ids                  desc       url...       tags...
001         "数据库"       000,002        100,101,102,103             
```

### no_sql 设计
```c++
mongodb

node_id     node_name     parrent_ids     child_ids                  desc       url...       tags...
001         "数据库"       000,002        100,101,102,103             

```
### 临接表
```c
Vertex        name        
1             "database"
2             "mysql"
3             "oracle"
4             "mongdb"

```

```c
EdgeId	      Start Vertex           	EndVertex
001             A                       C
003             A                       D
004             B                       D

```
### 查询设计
* ElasticSearch

### 相关事项
* 每个节点的访问量
* 每个节点的人物记
```c++

```

### 后端传到前端的数据
* 每个节点的 字串的长度也 传过来
* 后端计算相关位置？还是前端计算。
* 如何保存数据量

### 数据库设计
* https://blog.csdn.net/jim8757/article/details/52385612

Closure Table将树中每个节点与其子孙节点的关系都存储了下来，如下图所示：
![](https://github.com/asialiugf/blogs/blob/master/image/1333729578_4459.jpg)

### 要实现的目标
1. DAG以某一节点为root，将其下面的所有子节点以及link全部查询出来。
2. 每个用户可以独立存储自己的多个不同的DAG
3. 用户各自的DAG可以相互收藏
4. 可以全文检索
5. 可以AI智能推荐


