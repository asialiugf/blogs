```js
var a = 'foo', b = 42, c = { xy: "xxx" };
var o = { a, b, c };
let o2 = { a, b }
let o3 = { b, c }
let o4 = { a }
let o5 = { a: a }
let o6 = o4   // js对象浅拷贝

console.log('o4', o4)  // {a: "foo"}
console.log('o5', o5)  // {a: "foo"}

console.log(o4 === o5)  //false
console.log(o4 == o5)  //false
console.log('xxx',o4 = o5) // {{a: "foo"}}   js对象浅拷贝
console.log(o4 === o5)  //true
console.log(o4 == o5)  //true


console.log(o4)
o4.a = 'eeeeeeeeeeeeee'
console.log('o4:', o4) // {a: "eeeeeeeeeeeeee"}
console.log('o5:', o5) // {a: "eeeeeeeeeeeeee"}
console.log('o6:', o6) // {a: "foo"}

o4.b = 1000000
console.log('o4:', o4) // {a: "eeeeeeeeeeeeee", b: 1000000}
console.log('o5:', o5) // {a: "eeeeeeeeeeeeee", b: 1000000}

```
