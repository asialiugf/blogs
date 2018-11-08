
### vue-cli 3.0脚手架配置及扩展 （二）：vue.config.js多页配置
* https://blog.csdn.net/franks_t_d/article/details/80740268

### vuejs/vue-docs-zh-cn 这里会放所有VUE中文文档
* https://github.com/vuejs/vue-docs-zh-cn

### 本地安装vue-cli
参考：

- https://blog.csdn.net/xuqipeter/article/details/80452271

#### 前置条件

- 更新npm到最新版本 
```js
命令行运行: 
npm install -g npm 
npm就自动为我们更新到最新版本
npm install -g cnpm
npm install -g yarn
```
- 淘宝npm镜像使用方法 
```js
npm config set registry https://registry.npm.taobao.org 
cnpm config set registry https://registry.npm.taobao.org 
yarn config set registry https://registry.npm.taobao.org 
```
或者：
```js
[GD-0103-0121@rjsys ~]$ npm install -g cnpm --registry=https://registry.npm.taobao.org
```

- 配置后可通过下面方式来验证是否成功 ： 
```js
npm config get registry
```
- 使用 npm 全局安装 vue-cli ：(这里采用官网命令）
- https://cli.vuejs.org/
```js
npm install -g @vue/cli
# OR
yarn global add @vue/cli

vue create my-project
```
### vue create myproject 在windows下需要安装的依赖
```js
# npm install -g node-gyp 
# npm i -g windows-build-tools
```
如果安装不上，执行以下：
```
[Administrator @ DESKTOP-6IOA0JN /c/e/github.com/vue]$ npm config set registry http://registry.cnpmjs.org
[Administrator @ DESKTOP-6IOA0JN /c/e/github.com/vue]$ npm i -g windows-build-tools
```
***
# 【20180921】
如果你在 Windows 上通过 minTTY 使用 Git Bash，交互提示符并不工作。你必须通过 winpty vue.cmd create hello-world 启动这个命令。
```
winpty vue.cmd create hello-world
```
* ESLint是js中目前比较流行的插件化的静态代码检测工具。通过使用它可以保证高质量的代码，尽量减少和提早发现一些错误。使用eslint可以在工程中保证一致的代码风格，特别是当工程变得越来越大、越来越多的人参与进来时，需要加强一些最佳实践。

* What is Babel? Babel is a JavaScript compiler
* https://babeljs.io/docs/en/index.html

### 加element-ui
```
vue create my-app
cd my-app
vue add element
```

* main.js 会加一行 import './plugins/element.js'

```
import Vue from 'vue'
import App from './App.vue'
import router from './router'
import store from './store'
import './plugins/element.js'

Vue.config.productionTip = false

new Vue({
  router,
  store,
  render: h => h(App)
}).$mount('#app')
```


