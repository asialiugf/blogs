如果你是用vue.js官网提供的脚手架工具并沿用默认配置的话，

你执行npm run dev的时候会出来页面，是因为你根目录下的package.json文件里script配置了"dev": "node build/dev-server.js"，

也就是其实执行的是dev-server.js这个文件，里面有定义var webpackConfig = require('./webpack.dev.conf');

因为我们这个脚手架工具里是用webpack来打包项目文件的，

依赖的webpack.dev.conf文件里又定义了var baseWebpackConfig = require('./webpack.base.conf');

在这个依赖webpack.base.conf文件里面entry入口文件就配置了app: './src/main.js'，

所以当你运行npm run dev的时候就从main.js这个入口文件开始执行了


#### /github/vue-element-admin$ vi package.json 
```javascript
  "scripts": {
    "dev": "cross-env BABEL_ENV=development webpack-dev-server --inline --progress --config build/webpack.dev.conf.js",
    "build:prod": "cross-env NODE_ENV=production env_config=prod node build/build.js",
    "build:sit": "cross-env NODE_ENV=production env_config=sit node build/build.js",
    "lint": "eslint --ext .js,.vue src",
    "test": "npm run lint"
  },
  ```
####  build/webpack.dev.conf.js
```js
'use strict'
const path = require('path')
const utils = require('./utils')
const webpack = require('webpack')
const config = require('../config')
const merge = require('webpack-merge')
const baseWebpackConfig = require('./webpack.base.conf')
const HtmlWebpackPlugin = require('html-webpack-plugin')
const FriendlyErrorsPlugin = require('friendly-errors-webpack-plugin')
const portfinder = require('portfinder')
```

#### webpack.base.conf.js
```js
module.exports = {
  context: path.resolve(__dirname, '../'),
  entry: {
    app: './src/main.js'
  },
  output: {
    path: config.build.assetsRoot,
    filename: '[name].js',
    publicPath: process.env.NODE_ENV === 'production' ?
      config.build.assetsPublicPath :
      config.dev.assetsPublicPath
  },
```
### vue安装
##### cli官网
- https://cli.vuejs.org/guide/creating-a-project.html#installation
```js
npm install vue
npm install -g @vue/cli
```
