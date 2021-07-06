### 创建新对象
```js
let obj = {};
var o1 = new Object();
var o2 = new Object(undefined);
var o3 = new Object(null)
```
### 对象初始化
```js

let a = 'aaaaaaaa'
let b = 44
let c = {}

let obj01 = {a:'aaaaaaaa',b:44,c:{}}  
let obj02 = {a,b,c}
let obj03 = {
    a:a,
    b:b,
    c:c
}
```
###  添加新属性
```js
obj01.x = 100
obj01.y = {a:'dd',b:'cc'}

```
### 指向同一个对象
```js
let obj1 = new Object(obj); // 相当于 let obj1 = obj ; 指向同一个对象
```

