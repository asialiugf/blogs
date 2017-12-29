## uWS 和 web客户端交互

在server端的程序如下：
```c
  h.onPong([](uWS::WebSocket<uWS::SERVER> *ws, char *message, size_t length) {

    std::cout<< "ok ping\n";

  });
  h.getDefaultGroup<uWS::SERVER>().startAutoPing(2000);
```

index.html的js代码如下：
```js
riddle@asiamiao:~/uquant/html$ cat ping.html
<!DOCTYPE html>
<html>

<head>
  <meta charset="utf-8">
</head>

<body>

  <script type="text/javascript">
    // -------------------------------websocket

    // new Websocket
    var s = new WebSocket('ws://47.93.37.54:80/api/push/ws');

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
