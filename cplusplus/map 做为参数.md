### C++ Map传递参数

转载 2012年12月20日 17:29:32 9598
map可以被看做是普通变量一样可以直接赋值，同时map也可以看做普通变量一样在函数间以值传递或者以指针传递方式传递。

下面是一个小小的例子：
```c
#include <iostream>
#include <map>
#include <string.h>
using namespace std;

void translate(map<int,int> temp_map);//map直接作为参数传递
void translate(map<int,int> *pMap);//传递map指针
int main()
{
 map<int,int> map1;
 map1.insert(map<int,int>::value_type(1,22));
 
 translate(map1);
 
 map<int,int> *pmap1;
 pmap1 = &map1;
 
 translate(pmap1);
 
 map<int,int> map2;
 map2 = map1;//直接赋值
 map<int,int>::iterator iter;
 iter = map2.begin();
 cout<<iter->second<<endl;
 return 0;   
}

void translate(map<int,int> temp_map)
{
 map<int,int> map2;
 map2 = temp_map;
 map<int,int>::iterator iter;
 iter = map2.begin();
 cout<<iter->second<<endl;
}
void translate(map<int,int> *pMap)
{
 map<int,int> map2;
 map2 = *pMap;
 map<int,int>::iterator iter;
 iter = map2.begin();
 cout<<iter->second<<endl; 
}
```
