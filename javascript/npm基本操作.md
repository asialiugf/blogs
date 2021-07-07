```js
[GD-0103-0121@rjsys ~]$ npm -v
6.1.0
[GD-0103-0121@rjsys ~]$
```
### npm upgrade
```js
npm install npm -g

[GD-0103-0121@rjsys ~]$ npm -v
6.1.0
[GD-0103-0121@rjsys ~]$
[GD-0103-0121@rjsys ~]$ npm install npm -g
C:\Users\GD-0103-0121\AppData\Roaming\npm\npm -> C:\Users\GD-0103-0121\AppData\Roaming\npm\node_modules\npm\bin\npm-cli.js
C:\Users\GD-0103-0121\AppData\Roaming\npm\npx -> C:\Users\GD-0103-0121\AppData\Roaming\npm\node_modules\npm\bin\npx-cli.js
+ npm@6.3.0
added 265 packages from 147 contributors, removed 551 packages and updated 16 packages in 86.32s
[GD-0103-0121@rjsys ~]$ npm -v
6.3.0
[GD-0103-0121@rjsys ~]$

```
```js
[GD-0103-0121@rjsys ~]$ which npm
/c/Program Files/nodejs/npm
[GD-0103-0121@rjsys ~]$
```

### 使用淘宝 NPM 镜像
大家都知道国内直接使用 npm 的官方镜像是非常慢的，这里推荐使用淘宝 NPM 镜像。

淘宝 NPM 镜像是一个完整 npmjs.org 镜像，你可以用此代替官方版本(只读)，同步频率目前为 10分钟 一次以保证尽量与官方服务同步。

你可以使用淘宝定制的 cnpm (gzip 压缩支持) 命令行工具代替默认的 npm:
```js
$ npm install -g cnpm --registry=https://registry.npm.taobao.org
```
这样就可以使用 cnpm 命令来安装模块了：
```js
$ cnpm install [name]
```
更多信息可以查阅：http://npm.taobao.org/。

### 清理chache
```js
 npm cache clear --force
 npm config set global=false
npm config set sass_binary_site https://npm.taobao.org/mirrors/node-sass/
npm config set sharp_dist_base_url https://npm.taobao.org/mirrors/sharp-libvips/
npm config set electron_mirror https://npm.taobao.org/mirrors/electron/
npm config set puppeteer_download_host https://npm.taobao.org/mirrors/
npm config set phantomjs_cdnurl https://npm.taobao.org/mirrors/phantomjs/
npm config set sentrycli_cdnurl https://npm.taobao.org/mirrors/sentry-cli/
npm config set sqlite3_binary_site https://npm.taobao.org/mirrors/sqlite3/
npm config set python_mirror https://npm.taobao.org/mirrors/python/

npm install --force
```


