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

let obj01 = {a:'aaaaaaaa',b:44,c:{}}  // 【一】 直接赋值
let obj02 = {a,b,c}                   // 【二】 新写法
let obj03 = {                         // 【三】 老写法
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
### 利用以前的对象生成新对象
#### 【一】  利用以前的对象生成一个新的空对象，原形为以前的对象 let obj3 = Object.create(obj)
注意：如下例子中
- 可以用 obj3.d 引用原型对象(obj)中的属性 d (obj.d) 
- 最好用 console.log(obj3.__proto__.d) 这种方法来引用 原型中的 d这个属性
- 可以用 obj3.e 新增加属性
- 如果用 obj3.d = '88888' ，相当于给新的obj3增加了一个d属性。 不会改变 obj.d 'kkkkkk'
```js
let obj3 = Object.create(obj)  // 以obj为原型对象，创建一个新的空对象

console.log('obj:', obj)
console.log('obj1:', obj1)
console.log('obj2:', obj2)
console.log('after create,obj3:', obj3)
console.log(obj3.d)
console.log(obj3.e)


// after create,obj3: {}d: "88888"e: "hello"__proto__: Object
// 5555
// undefined

obj3.d = '88888'
obj3.e = 'hello'

// obj3: 
// {d: "88888", e: "hello"}
// d: "88888"
// e: "hello"
// __proto__:
// a: "bar"
// b: 100
// c: "hhhhhhhhh"
// d: "kkkkkk"
// __proto__: Object

```
####【二】 复制出一个新对象 let obj4 = Object.assign({},obj2)
注意：
- 不会复制对象中的对象
- 不会复制对象中的原型

```js
let obj4 = Object.assign({},obj2) // 复制一个新的对象
//或者：
let obj5 = {}
Object.assign(obj5,obj1)  // 复制一个对的对象
```
```js
let obj6 = Object.assign({},obj3)  // 复制 obj3，但不复制obj3的原型
console.log(obj6)


let obj_a = {name:"qiankaobei",other:{age:"123",height:"321"}};
let obj_b = Object.assign({},obj_a); // // 复制obj_a的第一层，里面的对象不复制
console.log(obj_a);//{ name: 'qiankaobei', other: { age: '999', height: '321' } }
console.log(obj_b);//{ name: 'qiankaobei', other: { age: '999', height: '321' } }
console.log('a age',obj_a.other.age) // 123
console.log('b age',obj_b.other.age) // 123
 
obj_b.name = 'yiceng';
obj_a.other.age = '999';
console.log(obj_a);//{ name: 'qiankaobei', other: { age: '999', height: '321' } }
console.log(obj_b);//{ name: 'yiceng', other: { age: '999', height: '321' } }
console.log('a age',obj_a.other.age) // 999
console.log('b age',obj_b.other.age) // 999


let ccc = Object.assign({},obj_a.other)
obj_b.other = ccc 

console.log(obj_a.other)
console.log(obj_b.other)
obj_a.other.age = '999';
obj_b.other.age = '111111999';
console.log(obj_a.other)
console.log(obj_b.other)
```






