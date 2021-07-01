### export default
在js文件中，只能 export一个default。其它的default需要加 {}
#### App.js
```js
import { useRef } from 'react';
import logo from './logo.svg';
import './App.css';

export default function App() {
  let s1 = useRef()

  return (
    <div className="App">
      <button type="botton" onClick={e => {
        s1.current.style.background = "blue"
      }}>加1</button>
      <div ref={s1}>
        好人平安！
      </div>

    </div>
  );
}

// class App extends React.Component {
//   constructor(props){
//     super(props)
//     s1 = useRef()
//   }
//   //let s1 = useRef()
//   render(){
//     return(
//     <div className="App">
//       <button type="botton" onClick={e => {
//         s1.current.style.background = "blue"
//       }}>加1</button>
//       <div ref={s1}>
//         好人平安！
//       </div>

//     </div>
//     )
//   }

// }




export function Bar() {
  return(
    <div>
      ahah 
    </div>
  )

}
function Foo() {
  return(
    <div>
      bbb
    </div>
  )
}

export {Foo}
```

#### index.js
```js
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';
import {Foo} from './App';
import {Bar} from './App';
import reportWebVitals from './reportWebVitals';

ReactDOM.render(
  <React.StrictMode>
    <App />
    <Foo />
    <Bar />
  </React.StrictMode>,
  document.getElementById('root')
);

// If you want to start measuring performance in your app, pass a function
// to log results (for example: reportWebVitals(console.log))
// or send to an analytics endpoint. Learn more: https://bit.ly/CRA-vitals
reportWebVitals();

```
