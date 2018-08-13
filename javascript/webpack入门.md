### 一 mkdir test01
创建一个项目目录。
### 二 npm init
```js
cd test01
npm init
```
执行完后，在 test01目录下生成一个 package.json 文件

这个文件是npm 的配置文件，其中 scripts 是 使用npm 命令可以执行的脚本。

```js
[GD-0103-0121@rjsys /d/github.com/webpacktest/test01]$ ll
total 1
-rw-r--r-- 1 GD-0103-0121 197121 202 八月 13 20:05 package.json
[GD-0103-0121@rjsys /d/github.com/webpacktest/test01]$
```
```js
[GD-0103-0121@rjsys /d/github.com/webpacktest/test01]$ cat package.json
{
  "name": "test01",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC"
}
[GD-0103-0121@rjsys /d/github.com/webpacktest/test01]$
```
### 三 创建项目
项目目录结构
```js
[GD-0103-0121@rjsys /d/github.com/webpacktest/mytest]$ tree
.
|-- dist
|   |-- 9395f8fb81e0b6275c3827a4c73b5c78.gif
|   |-- index.html
|   `-- main.js
|-- package-lock.json
|-- package.json
|-- src
|   |-- hsq.gif
|   |-- index.css
|   `-- index.js
`-- webpack.config.js

2 directories, 9 files
[GD-0103-0121@rjsys /d/github.com/webpacktest/mytest]$
```
* dist/index.html 
```js
[GD-0103-0121@rjsys /d/github.com/webpacktest/test01/dist]$ cat index.html
<!doctype html>
<html>
  <head>
    <title>Getting Started</title>
  </head>
  <body>
    <script src="main.js"></script>
  </body>
</html>
[GD-0103-0121@rjsys /d/github.com/webpacktest/test01/dist]$
```
* src/index.css
```js
[GD-0103-0121@rjsys /d/github.com/webpacktest/test01/src]$ cat index.css
body {
  background-color: #d0e4fe;
}

h1 {
  color: orange;
  text-align: center;
}

p {
  font-family: "Times New Roman";
  font-size: 20px;
}
[GD-0103-0121@rjsys /d/github.com/webpacktest/test01/src]$
```
* src/index.js
```js
[GD-0103-0121@rjsys /d/github.com/webpacktest/test01/src]$ cat index.js
import _ from 'lodash';
import './index.css';
import picture from './hsq.gif';

function component() {
  var element = document.createElement('div');

  // Lodash, now imported by this script
  element.innerHTML = _.join(['Hello', 'webpack'], ' ');
  element.classList.add('colorRed');

  var image = new Image();
  image.src = picture;
  element.appendChild(image);

  return element;
}

document.body.appendChild(component());
[GD-0103-0121@rjsys /d/github.com/webpacktest/test01/src]$
```
### 四 创建webpack配置文件
在 test01/目录下
```js
[GD-0103-0121@rjsys /d/github.com/webpacktest/test01]$ cat webpack.config.js
var path = require('path')

module.exports = {
  entry: './src/index.js',
  output: {
    filename: 'main.js',
    path: path.resolve(__dirname, 'dist')
  },
  module: {
    rules:[
      {
        test: /\.css$/,
            use: [
              'style-loader',
              'css-loader'
            ]
      },
      {
        test:/\.(gif|png|jpg)$/,
            use: ['file-loader']
      }
    ]
  }
}
[GD-0103-0121@rjsys /d/github.com/webpacktest/test01]$
```

### 五 修改 package.json
```js
[GD-0103-0121@rjsys /d/github.com/webpacktest/test01]$ cat package.json
{
  "name": "test01",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "dev": "npx webpack --mode development",
    "build": "npx webpack --config webpack.config.js"
  },
  "author": "",
  "license": "ISC",
  "dependencies": {
    "lodash": "^4.17.10"
  },
  "devDependencies": {
    "css-loader": "^1.0.0",
    "file-loader": "^1.1.11",
    "style-loader": "^0.22.1"
  }
}
[GD-0103-0121@rjsys /d/github.com/webpacktest/test01]$

```

### 六 运行npm 依赖
```js
$  npm install --save-dev style-loader css-loader file-loader
```

### 七 执行打包
```js
npm run dev
npm run build
```

### 八 浏览器打开
```js
file:///D:/github.com/webpacktest/test01/dist/index.html
```




