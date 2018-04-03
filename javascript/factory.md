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


### Javascript 函数即对象
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
/*
相当于有一个函数：
function(m) {
  alert(m)
}
然后直接执行：
function("pppppppppp")
*/
 let kk = new barCharts()
 //kk.aa1()
 kk.aa2()
 kk.m = 10
 kk.aa2()

</script>

</html>
```

### test.js
```js
(function(global, factory) {
  typeof exports === 'object' && typeof module !== 'undefined' ? module.exports = factory() :
    typeof define === 'function' && define.amd ? define(factory) :
    (global.barCharts = factory()); // 为 this 返回一个 barCharts。
}(this, (function() {
  function aa(x) {
    this.m = x
    alert("mmm")
    
    function aa1() {
      alert("aaaaa1")
    }
    // 只有this定义的才是成员变量。
    this.aa2 = function() {
       alert(this.m)
       alert("this.aa1() aaaaa1")
    } 
    
    aa1();
    this.aa2();
  }
  alert ("88888888888888")
  let hh = new aa(100000)
  alert ("99999999999999")
  return aa;
})))
```
