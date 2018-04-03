/* charmi liu version 1.0 */
/*
               --------------------
               |                  |
 (  function(global,factory){} ( this,(fuction(){}) )    );
                      |                   |
                      ---------------------
 上面这一句，相当于直接执行了第一个function。
 这个function的参数有两个，global ==> 在这里就是 浏览器window
 第二个参数   factory 就是后面的function。


### 
*/
```js
(function(global, factory) {
  typeof exports === 'object' && typeof module !== 'undefined' ? module.exports = factory() :
    typeof define === 'function' && define.amd ? define(factory) :
    (global.barCharts = factory());  // 为 this 返回一个 barCharts。
}(this, (function() {} )));
```

```js
<head>
  <!--  <script src="vue.js"></script> -->
  <script src="test.js">
  </script>
</head>

<body>
  <ul id="test">
    <li> item 1 </li>
    <li> item 2 </li>
    <li> item 3 </li>
  </ul>
</body>

<script>
(function(m){alert(m)}("ppppppppp"))
 let kk = new barCharts()
 //kk.aa1()
 kk.aa2()
 kk.m = 10
 kk.aa2()

</script>

</html>
```
