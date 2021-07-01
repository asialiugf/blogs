```js
import { useState, useRef } from 'react';
import logo from './logo.svg';
import './App.css';

export function App() {
  let [s1, HaHa] = useState(100)
  let [s3,Se] = useState()
  let s2 = useRef()
  let m = 1000

  const Hello = () => {
    HaHa(s1 + 1 + m)
    Se(s3 = <div>hellolo</div>)
    s2.current.style.background = "red"
  }


  return (
    <div className="App">
      <p>
        {s1}
      </p>
      {/* <button type="botton" onClick={()=>{HaHa(cc)}}>加1</button> */}
      {/* <button type="botton" onClick={() => { HaHa(dd) }}>加1</button> */}
      {/* <button type="botton" onClick={() => Hello()}>加1</button> */}
      <button type="botton" onClick={Hello}>加1</button>
      {/* <button type="botton" onClick={() => HaHa(s1 + 1)}>加1</button> */}
      {/* <button type="botton" onClick={e=>{HaHa(bb)}}>加1</button> */}
      <div ref={s2}>
        好人平安！
        {s3}
      </div>

    </div>
  );
}



function bb(x) {
  return x + 1
}

let cc = bb

let dd = x => x + 1
let ff = y => { console.log("ttt") }

ff()

```

```js
import { useState, useRef } from 'react';
import logo from './logo.svg';
import './App.css';

export function App() {
  let [s1, HaHa] = useState(100)
  let [s3, Se] = useState()
  let s2 = useRef()
  let m = 1000

  const Hello = () => {
    // HaHa(s1+1+m)
    HaHa(bb)
    Se(s3 = <div>hellolo</div>)
    s2.current.style.background = "red"
  }



  return (
    <div className="App">
      <p>
        {s1}
      </p>
      {/* <button type="botton" onClick={()=>{HaHa(cc)}}>加1</button> */}
      {/* <button type="botton" onClick={() => { HaHa(dd) }}>加1</button> */}
      <button type="botton" onClick={() => { HaHa(s1 + 1) }}>加1</button>
      {/* <button type="botton" onClick={ HaHa(s1 + 1) }>加1</button>  这一句 是错误的*/}
      {/* <button type="botton" onClick={() => Hello()}>加1</button> */}
      {/* <button type="botton" onClick={Hello}>加2</button> */}
      {/* <button type="botton" onClick={() => HaHa(s1 + 1)}>加1</button> */}
      {/* <button type="botton" onClick={e=>{HaHa(bb)}}>加1</button> */}
      <div ref={s2}>
        好人平安！
        {s3}
      </div>

    </div>
  );
}


function bb(y) {
  return 7 + 1 + y
}

let cc = bb

let dd = x => x + 1
let ff = y => { console.log("ttt") }

ff()
```
