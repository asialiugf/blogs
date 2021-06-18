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
### ESLint 没有用的VAR 注释  在前面加 // eslint-disable-next-line no-unused-vars
```js
// eslint-disable-next-line no-unused-vars
function App() {
  return (
    <div className="App">
      <header className="App-header">
        <img src={logo} className="App-logo" alt="logo" />
        <p>
          Edit <code>src/App.js</code> and save to reload.
        </p>
        <a
          className="App-link"
          href="https://reactjs.org"
          target="_blank"
          rel="noopener noreferrer"
        >
          Learn React
        </a>
      </header>
    </div>
  );
}

```

### jsx 要用 {} 来引用e
#### 下面定义了常量 "e" ，在return  中, 用 {e} 来引用
```js
const e = <div>
  <div>abc</div>
  <div>abc</div>
  <div>abc</div>
</div>

function App() {
  return (
    <div className="App">
      <header className="App-header">
        <img src={logo} className="App-logo" alt="logo" />
        <p>
          Edit <code>src/App.tsx</code> and save to reload.
        </p>
        <a
          className="App-link"
          href="https://reactjs.org"
          target="_blank"
          rel="noopener noreferrer"
        >
          Learn React
        </a>
      </header>
      {e}
    </div>
  );
}

export default App;
```
