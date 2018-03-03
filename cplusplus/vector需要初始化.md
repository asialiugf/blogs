### vector需要初始化

```c
int main()
{
  std::vector<uWS::WebSocket<uWS::CLIENT>*>  cs(0);
  uWS::WebSocket<uWS::CLIENT>*  cs_3000_1;
  uWS::WebSocket<uWS::CLIENT>*  cs_3000_2;
  uWS::WebSocket<uWS::CLIENT>*  cs_3000_3;
  uWS::WebSocket<uWS::CLIENT>*  cs_3000_4;
```
如果上面cs(0)，或者是 std::vector<uWS::WebSocket<uWS::CLIENT>*>  cs;
下面对CS[0]赋值，将出错：
```c
  h.onConnection([&](uWS::WebSocket<uWS::CLIENT> *ws, uWS::HttpRequest req) {
    std::cout << "connection ok!" << std::endl;
    switch((long) ws->getUserData()) {
    case 1:
      cs[0] = ws;
      cs_3000_1 = ws;
      break;
```
```c
 @ :~/uws/tests$ ./CM.x
connection ok!
Segmentation fault
 @ :~/uws/tests$ 
```
### 正确的做法：
```c
int main()
{
  std::vector<uWS::WebSocket<uWS::CLIENT>*>  cs(1);
  uWS::WebSocket<uWS::CLIENT>*  cs_3000_1;
  uWS::WebSocket<uWS::CLIENT>*  cs_3000_2;
  uWS::WebSocket<uWS::CLIENT>*  cs_3000_3;
  uWS::WebSocket<uWS::CLIENT>*  cs_3000_4;
```
以上cs的长度只要大于0即可。比如，以上为cs(1)，那么在后面的程序可以对 cs[0],cs[1],cs[2]...进行赋值。
