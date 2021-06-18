### 加注释的方法
```js
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
// import App from './App';
import TT from './App';
import reportWebVitals from './reportWebVitals';

ReactDOM.render(
  <React.StrictMode>
    <TT />
    {/* <App /> */}   
    
  </React.StrictMode>,
  document.getElementById('root')
);

// If you want to start measuring performance in your app, pass a function
// to log results (for example: reportWebVitals(console.log))
// or send to an analytics endpoint. Learn more: https://bit.ly/CRA-vitals
reportWebVitals();
```
###  jsx单行注释  和 多行注释
``` js
{/* jsx */} 单行注释

{/*
    多行注释   
*/}
```
