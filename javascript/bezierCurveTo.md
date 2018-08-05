```js
[root@iZ23psatkqsZ html]# cat t001.html
<!DOCTYPE html>
<html>

<body>

  <canvas id="myCanvas" width="600" height="600" style="border:1px solid #d3d3d3;">
Your browser does not support the HTML5 canvas tag.</canvas>

  <script>
    var c = document.getElementById("myCanvas");
    var ctx = c.getContext("2d");
    //ctx.beginPath();
    ctx.beginPath();
    ctx.moveTo(20, 200);
    ctx.bezierCurveTo(20, 200, 100, 20, 150, 20);
    ctx.bezierCurveTo(150, 20, 100, 20, 30, 200);
    ctx.stroke();
    ctx.fillStyle="rgb(200,100,30)";
    ctx.fill();


    ctx.beginPath();
    ctx.moveTo(320, 200);
    ctx.bezierCurveTo(330, 180, 350, 20, 400, 20);
    ctx.bezierCurveTo(400, 20, 350, 20, 330, 200);
    ctx.stroke();
    ctx.fillStyle="rgb(200,100,30)";
    ctx.fill();


    ctx.beginPath();
    ctx.moveTo(200, 200);
    ctx.bezierCurveTo(200, 200, 300, 20, 350, 20);
    ctx.bezierCurveTo(350, 20, 300, 20, 210, 200);
    //ctx.bezierCurveTo(150, 20, 300, 20, 30, 400);
    ctx.stroke();
    ctx.fillStyle="rgb(100,100,30)";
    ctx.fill();



    /*
    ctx.beginPath();
    ctx.moveTo(sh.px0[0], sh.py0[0]);
    ctx.bezierCurveTo(sh.px0[1], sh.py0[1], sh.px0[2], sh.py0[2], sh.px0[3], sh.py0[3]);
    ctx.lineTo(sh.px1[3], sh.py1[3]);
    ctx.bezierCurveTo(sh.px1[2], sh.py1[2], sh.px1[1], sh.py1[1], sh.px1[0], sh.py1[0]);
    ctx.lineTo(sh.px0[0], sh.py0[0]);
    ctx.fill();
    */
  </script>

</body>

</html>
[root@iZ23psatkqsZ html]# 
```
