
### 苍青浪 webpack4-用之初体验，一起敲它十一遍
* https://www.cnblogs.com/cangqinglang/p/8964460.html
### install
* https://webpack.js.org/guides/installation/


### webpack 4 使用示例（一）
* https://blog.csdn.net/Hill_Kinsham/article/details/81328440
* https://blog.csdn.net/snow_small/article/details/79709585

### version
```js
[GD-0103-0121@rjsys /d/github.com/webpacktest/mytest]$ npm -version
6.1.0
[GD-0103-0121@rjsys /d/github.com/webpacktest/mytest]$
[GD-0103-0121@rjsys /d/github.com/webpacktest/mytest]$
[GD-0103-0121@rjsys /d/github.com/webpacktest/mytest]$ node --version
v8.11.3
[GD-0103-0121@rjsys /d/github.com/webpacktest/mytest]$

```
### 一 mkdir mytest
```js
[GD-0103-0121@rjsys /d/github.com/webpacktest]$ mkdir mytest
[GD-0103-0121@rjsys /d/github.com/webpacktest]$ cd mytest/
[GD-0103-0121@rjsys /d/github.com/webpacktest/mytest]$ ll
total 0
[GD-0103-0121@rjsys /d/github.com/webpacktest/mytest]$
```
### 二 npm init

```js
[GD-0103-0121@rjsys /d/github.com/webpacktest/mytest]$
[GD-0103-0121@rjsys /d/github.com/webpacktest/mytest]$ npm init
This utility will walk you through creating a package.json file.
It only covers the most common items, and tries to guess sensible defaults.

See `npm help json` for definitive documentation on these fields
and exactly what they do.

Use `npm install <pkg>` afterwards to install a package and
save it as a dependency in the package.json file.

Press ^C at any time to quit.
package name: (mytest)
version: (1.0.0)
description:
entry point: (index.js)
test command:
git repository:
keywords:
author:
license: (ISC)
About to write to D:\github.com\webpacktest\mytest\package.json:

{
  "name": "mytest",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC"
}


Is this OK? (yes) yes
[GD-0103-0121@rjsys /d/github.com/webpacktest/mytest]$
[GD-0103-0121@rjsys /d/github.com/webpacktest/mytest]$
[GD-0103-0121@rjsys /d/github.com/webpacktest/mytest]$ ll
total 1
-rw-r--r-- 1 GD-0103-0121 197121 202 八月 12 14:43 package.json
[GD-0103-0121@rjsys /d/github.com/webpacktest/mytest]$
[GD-0103-0121@rjsys /d/github.com/webpacktest/mytest]$
[GD-0103-0121@rjsys /d/github.com/webpacktest/mytest]$ cat package.json
{
  "name": "mytest",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC"
}
[GD-0103-0121@rjsys /d/github.com/webpacktest/mytest]$

```
### 全局安装webpack
```js
[GD-0103-0121@rjsys /d/github.com/webpacktest/mytest]$
[GD-0103-0121@rjsys /d/github.com/webpacktest/mytest]$ npm install webpack -g
C:\Users\GD-0103-0121\AppData\Roaming\npm\webpack -> C:\Users\GD-0103-0121\AppData\Roaming\npm\node_modules\webpack\bin\webpack.js
npm WARN optional SKIPPING OPTIONAL DEPENDENCY: fsevents@1.2.4 (node_modules\webpack\node_modules\fsevents):
npm WARN notsup SKIPPING OPTIONAL DEPENDENCY: Unsupported platform for fsevents@1.2.4: wanted {"os":"darwin","arch":"any"} (current: {"os":"win32","arch":"x64"})

+ webpack@4.16.5
updated 3 packages in 38.759s
[GD-0103-0121@rjsys /d/github.com/webpacktest/mytest]$
[GD-0103-0121@rjsys /d/github.com/webpacktest/mytest]$
[GD-0103-0121@rjsys /d/github.com/webpacktest/mytest]$ webpack -v
4.16.5
[GD-0103-0121@rjsys /d/github.com/webpacktest/mytest]$
[GD-0103-0121@rjsys /d/github.com/webpacktest/mytest]$ npm install webpack-cli -g
C:\Users\GD-0103-0121\AppData\Roaming\npm\webpack-cli -> C:\Users\GD-0103-0121\AppData\Roaming\npm\node_modules\webpack-cli\bin\cli.js
npm WARN webpack-cli@3.1.0 requires a peer of webpack@^4.x.x but none is installed. You must install peer dependencies yourself.

+ webpack-cli@3.1.0
updated 4 packages in 11.027s
[GD-0103-0121@rjsys /d/github.com/webpacktest/mytest]$
[GD-0103-0121@rjsys /d/github.com/webpacktest/mytest]$ webpack -v
4.16.5
[GD-0103-0121@rjsys /d/github.com/webpacktest/mytest]$
```
### src/index.js
```js
[GD-0103-0121@rjsys /d/github.com/webpacktest/mytest/src]$ ll
total 1
-rw-r--r-- 1 GD-0103-0121 197121 256 八月 12 14:51 index.js
[GD-0103-0121@rjsys /d/github.com/webpacktest/mytest/src]$
[GD-0103-0121@rjsys /d/github.com/webpacktest/mytest/src]$ cat *
import _ from 'lodash';

function component() {
  let element = document.createElement('div');

  // Lodash, now imported by this script
  element.innerHTML = _.join(['Hello', 'webpack'], ' ');

  return element;
}

document.body.appendChild(component());
[GD-0103-0121@rjsys /d/github.com/webpacktest/mytest/src]$
```
### ./dist/index.html
```js
[GD-0103-0121@rjsys /d/github.com/webpacktest/mytest]$ cd dist
[GD-0103-0121@rjsys /d/github.com/webpacktest/mytest/dist]$ cat index.html
<!doctype html>
<html>

<head>
  <title>Getting Started</title>
</head>

<body>
  <script src="main.js"></script>
</body>

</html>
[GD-0103-0121@rjsys /d/github.com/webpacktest/mytest/dist]$

```
### install lodash
```js
[GD-0103-0121@rjsys /d/github.com/webpacktest/mytest]$ npm install --save lodash
npm WARN mytest@1.0.0 No description
npm WARN mytest@1.0.0 No repository field.

+ lodash@4.17.10
added 1 package from 2 contributors in 3.506s
[GD-0103-0121@rjsys /d/github.com/webpacktest/mytest]$
[GD-0103-0121@rjsys /d/github.com/webpacktest/mytest]$ ll
total 2
drwxr-xr-x 1 GD-0103-0121 197121   0 八月 12 14:53 dist/
drwxr-xr-x 1 GD-0103-0121 197121   0 八月 12 14:55 node_modules/
-rw-r--r-- 1 GD-0103-0121 197121 252 八月 12 14:54 package.json
-rw-r--r-- 1 GD-0103-0121 197121 306 八月 12 14:54 package-lock.json
drwxr-xr-x 1 GD-0103-0121 197121   0 八月 12 14:51 src/
[GD-0103-0121@rjsys /d/github.com/webpacktest/mytest]$ cd node_modules/
[GD-0103-0121@rjsys /d/github.com/webpacktest/mytest/node_modules]$ ll
total 256
drwxr-xr-x 1 GD-0103-0121 197121 0 八月 12 14:55 lodash/
[GD-0103-0121@rjsys /d/github.com/webpacktest/mytest/node_modules]$
```
### npx webpack
下面这几个命令差不多，
```js
$ webpack
$ webpack --mode development
$ npx webpack
$ npx webpack --mode development
```

```js
[GD-0103-0121@rjsys /d/github.com/webpacktest/mytest]$ npx webpack
npx: 1 安装成功，用时 12.003 秒
Path must be a string. Received undefined
C:\Users\GD-0103-0121\AppData\Roaming\npm\node_modules\webpack\bin\webpack.js
Hash: 2685c26673ffdcd386a1
Version: webpack 4.16.5
Time: 5484ms
Built at: 2018-08-12 14:56:45
  Asset      Size  Chunks             Chunk Names
main.js  70.5 KiB       0  [emitted]  main
Entrypoint main = main.js
[1] ./src/index.js 256 bytes {0} [built]
[2] (webpack)/buildin/global.js 489 bytes {0} [built]
[3] (webpack)/buildin/module.js 497 bytes {0} [built]
    + 1 hidden module

WARNING in configuration
The 'mode' option has not been set, webpack will fallback to 'production' for this value. Set 'mode' option to 'development' or 'production' to enable defaults for each environment.
You can also set it to 'none' to disable any default behavior. Learn more: https://webpack.js.org/concepts/mode/
[GD-0103-0121@rjsys /d/github.com/webpacktest/mytest]$
```
```js
[GD-0103-0121@rjsys /d/github.com/webpacktest/mytest]$ cd dist
[GD-0103-0121@rjsys /d/github.com/webpacktest/mytest/dist]$ ll
total 73
-rw-r--r-- 1 GD-0103-0121 197121   131 八月 12 14:53 index.html
-rw-r--r-- 1 GD-0103-0121 197121 72399 八月 12 14:56 main.js
[GD-0103-0121@rjsys /d/github.com/webpacktest/mytest/dist]$
```
### npm run dev
```js
[GD-0103-0121@rjsys /d/github.com/webpacktest/mytest]$ cat package.json
{
  "name": "mytest",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "dev": "webpack --mode development"
  },
  "author": "",
  "license": "ISC",
  "dependencies": {
    "lodash": "^4.17.10"
  }
}
[GD-0103-0121@rjsys /d/github.com/webpacktest/mytest]$
```
```js
[GD-0103-0121@rjsys /d/github.com/webpacktest/mytest]$ npm run dev

> mytest@1.0.0 dev D:\github.com\webpacktest\mytest
> webpack --mode development

Hash: 3db362630bbd79fb5b1f
Version: webpack 4.16.5
Time: 526ms
Built at: 2018-08-12 15:22:17
  Asset     Size  Chunks             Chunk Names
main.js  551 KiB    main  [emitted]  main
Entrypoint main = main.js
[../../node_modules/webpack/buildin/global.js] (webpack)/buildin/global.js 489 bytes {main} [built]
[../../node_modules/webpack/buildin/module.js] (webpack)/buildin/module.js 497 bytes {main} [built]
[./src/index.js] 256 bytes {main} [built]
    + 1 hidden module
[GD-0103-0121@rjsys /d/github.com/webpacktest/mytest]$
```
### webpack.config.js
```js
[GD-0103-0121@rjsys /d/github.com/webpacktest/mytest]$ cat webpack.config.js
var path = require('path')

module.exports = {
  entry: './src/index.js',
  output: {
    filename: 'main.js',
    path: path.resolve(__dirname, 'dist')
  }
}
[GD-0103-0121@rjsys /d/github.com/webpacktest/mytest]$
```
### package.json
添加一句："build": "npx webpack --config webpack.config.js"

```js
[GD-0103-0121@rjsys /d/github.com/webpacktest/mytest]$ cat package.json
{
  "name": "mytest",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "dev": "webpack --mode development",
    "build": "npx webpack --config webpack.config.js"
  },
  "author": "",
  "license": "ISC",
  "dependencies": {
    "lodash": "^4.17.10"
  }
}
[GD-0103-0121@rjsys /d/github.com/webpacktest/mytest]$
```
### npm run build
```js
[GD-0103-0121@rjsys /d/github.com/webpacktest/mytest]$ npm run build

> mytest@1.0.0 build D:\github.com\webpacktest\mytest
> npx webpack --config webpack.config.js

npx: 1 安装成功，用时 2.839 秒
Path must be a string. Received undefined
C:\Users\GD-0103-0121\AppData\Roaming\npm\node_modules\webpack\bin\webpack.js
Hash: be4ddd1abc08b5900878
Version: webpack 4.16.5
Time: 546ms
Built at: 2018-08-12 15:30:13
  Asset      Size  Chunks             Chunk Names
main.js  70.5 KiB       0  [emitted]  main
Entrypoint main = main.js
[1] ./src/index.js 256 bytes {0} [built]
[2] (webpack)/buildin/global.js 489 bytes {0} [built]
[3] (webpack)/buildin/module.js 497 bytes {0} [built]
    + 1 hidden module

WARNING in configuration
The 'mode' option has not been set, webpack will fallback to 'production' for this value. Set 'mode' option to 'development' or 'production' to enable defaults for each environment.
You can also set it to 'none' to disable any default behavior. Learn more: https://webpack.js.org/concepts/mode/
[GD-0103-0121@rjsys /d/github.com/webpacktest/mytest]$
```
### 【css】
```js
[GD-0103-0121@rjsys /d/github.com/webpacktest/mytest]$ npm install --save-dev style-loader css-loader file-loader
npm WARN css-loader@1.0.0 requires a peer of webpack@^4.0.0 but none is installed. You must install peer dependencies yourself.
npm WARN file-loader@1.1.11 requires a peer of webpack@^2.0.0 || ^3.0.0 || ^4.0.0 but none is installed. You must install peer dependencies yourself.
npm WARN mytest@1.0.0 No description
npm WARN mytest@1.0.0 No repository field.

+ css-loader@1.0.0
+ file-loader@1.1.11
+ style-loader@0.22.1
added 50 packages from 61 contributors in 9.052s
[GD-0103-0121@rjsys /d/github.com/webpacktest/mytest]$
[GD-0103-0121@rjsys /d/github.com/webpacktest/mytest]$
[GD-0103-0121@rjsys /d/github.com/webpacktest/mytest]$ ll
total 38
drwxr-xr-x 1 GD-0103-0121 197121     0 八月 12 15:30 dist/
drwxr-xr-x 1 GD-0103-0121 197121     0 八月 12 15:33 node_modules/
-rw-r--r-- 1 GD-0103-0121 197121   464 八月 12 15:33 package.json
-rw-r--r-- 1 GD-0103-0121 197121 14589 八月 12 15:33 package-lock.json
drwxr-xr-x 1 GD-0103-0121 197121     0 八月 12 14:51 src/
-rw-r--r-- 1 GD-0103-0121 197121   159 八月 12 15:27 webpack.config.js
[GD-0103-0121@rjsys /d/github.com/webpacktest/mytest]$
[GD-0103-0121@rjsys /d/github.com/webpacktest/mytest]$ vi package.json
[GD-0103-0121@rjsys /d/github.com/webpacktest/mytest]$ cat package.json
{
  "name": "mytest",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "dev": "webpack --mode development",
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
[GD-0103-0121@rjsys /d/github.com/webpacktest/mytest]$
```

### 修改 index.js
```js
[GD-0103-0121@rjsys /d/github.com/webpacktest/mytest/src]$ ll
total 1
-rw-r--r-- 1 GD-0103-0121 197121 429 八月 12 15:41 index.js
[GD-0103-0121@rjsys /d/github.com/webpacktest/mytest/src]$ cat index.js
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
[GD-0103-0121@rjsys /d/github.com/webpacktest/mytest/src]$
```

```js
[GD-0103-0121@rjsys /d/github.com/webpacktest/mytest/src]$ ls -l
total 6
-rw-r--r-- 1 GD-0103-0121 197121 4077 八月 12 16:05 hsq.gif
-rw-r--r-- 1 GD-0103-0121 197121  145 八月 12 16:04 index.css
-rw-r--r-- 1 GD-0103-0121 197121  429 八月 12 15:41 index.js
[GD-0103-0121@rjsys /d/github.com/webpacktest/mytest/src]$ ll
```
### index.css
```js
[GD-0103-0121@rjsys /d/github.com/webpacktest/mytest/src]$ cat index.css
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
[GD-0103-0121@rjsys /d/github.com/webpacktest/mytest/src]$
```
### 配置 webpack.config.js
```js
[GD-0103-0121@rjsys /d/github.com/webpacktest/mytest]$ cat webpack.config.js
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
[GD-0103-0121@rjsys /d/github.com/webpacktest/mytest]$
```
### npm run build
### 浏览器打开 file:///D:/github.com/webpacktest/mytest/dist/index.html
即可以看到效果

###   插件的使用举例
* https://www.cnblogs.com/xuelongqy/p/7081419.html
