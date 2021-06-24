![image](https://user-images.githubusercontent.com/24709527/123290277-b5874b80-d543-11eb-991e-ea890fa4251b.png)
### 目录结构
#### index.html (public目录下）
```js
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <link rel="icon" href="%PUBLIC_URL%/favicon.ico" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <meta name="theme-color" content="#000000" />
    <meta
      name="description"
      content="Web site created using create-react-app"
    />
    <link rel="apple-touch-icon" href="%PUBLIC_URL%/logo192.png" />

    <link rel="manifest" href="%PUBLIC_URL%/manifest.json" />

    <title>React App</title>
  </head>
  <body>
    <noscript>You need to enable JavaScript to run this app.</noscript>
    <div id="root"></div>
    <div id="root1"></div>
 
  </body>
</html>

```

#### index.jsx
 index.jsx是程序的入口，主要是对组件进行渲染。 如下，是对     <App /> <Hello /> 两个组件进行渲染。 分别与 index.html中的 root root1 挂接
```js
ReactDOM.render(
  <React.StrictMode>
    <App />
    <Hello />
  </React.StrictMode>,
  document.getElementById('root')
);

ReactDOM.render(
  <React.StrictMode>

    <Hello />
  </React.StrictMode>,
  document.getElementById('root1')
);
```
#### Hello.jsx App.jsx是组件
主要是return一个<div>组件（一个return 只能返回一个 <div>)
```js
import React from 'react';
// import logo from './logo.svg';


const e = <div>
  <div>abc</div>
  <div>abc</div>
  <div>abc</div>
</div>

function Hello() {
  return (
    <div className="App">

      <p>
        我为什么在这里？
      </p>

      {e}
    </div>
  );
}

export default Hello;

```



