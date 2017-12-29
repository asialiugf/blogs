## uWS 和 web客户端交互

由server端发出ping,由web js client端回应pong，由下面的程序可以看到：

- js只要建立连接，没有onping onpong事件，所以不必写任何动作，但它建立连接后，能够回应 server发出的ping，然后自动回应pong
- server端 要定时发送ping包，然后在onPong中进行处理，如果发现异常，可以做后继处理。

在server端的程序如下：
```c
  h.onPong([](uWS::WebSocket<uWS::SERVER> *ws, char *message, size_t length) {

    std::cout<< "ok ping\n";

  });
  h.getDefaultGroup<uWS::SERVER>().startAutoPing(2000);
```

index.html的js代码如下：
```js
riddle@d:~/uquant/html$ cat ping.html
<!DOCTYPE html>
<html>

<head>
  <meta charset="utf-8">
</head>

<body>

  <script type="text/javascript">
    // -------------------------------websocket

    // new Websocket
    var s = new WebSocket('ws://147.93.57.54:80/api/push/ws');

    // register my topic
    s.onopen = function()
    {
      this.send('{"action":"subs","stamp":"x","data":["TA703","TA705"]}');
      //this.send('{"action":"subs","stamp":"x","data":["TA701"]}');
    }

    s.onmessage = function(v)
    {
          console.log("haha");
      console.log(v.data);
    }

    // ping/pong heartbeat
    //job = setInterval('s.send(\'{"action":"test ping"}\')', 1000);
  </script>

</body>

</html>
```
### 运行的结果，在服务器端可以看到：
```c
riddle@asiamiao:~/uquant/src$ ./webdata.x 
Server onConnection send: --server--
Server onMessage receive: {"action":"subs","stamp":"x","data":["TA703","TA705"]}
ok ping
ok ping
ok ping
ok ping
ok ping
ok ping
ok ping
ok ping
ok ping
ok ping
ok ping
ok ping
ok ping
ok ping
ok ping
ok ping
.
.
.
```
