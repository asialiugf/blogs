### 安装 

#### 使用说明
* kityminder-core 依赖于 kity，开发中用到 seajs 进行异步加载。 例子中 dev.html 使用 seajs 进行包加载，example.html 使用同步加载的方式。 使用步骤如下：

* 安装 bower
* git clone https://github.com/fex-team/kityminder-core.git
* 切换到 kityminder-core 目录下，运行：
```
bower install
```
#### 开发说明
```
安装 bower
安装 npm
bower install
npm install
npm run dev
```

#### 修改hostname 
*　注意下面的hostname IP要加引号。
* [root@iZ23psatkqsZ kityminder-core]# vi node_modules/browser-sync/dist/default-config.js
```c++
    minify: true,
    /**
     * @property host
     * @type String
     * @default null
     */
    host: "121.199.28.62",
    //host: null,
    /**
     * Support environments where dynamic hostnames are not required
     * (ie: electron)
     * @property localOnly
     * @type Boolean
     * @default false
     * @since 2.14.0
```
```c++
[root@iZ23psatkqsZ kityminder-core]# npm run dev

> kityminder-core@1.4.49 dev /root/github/kityminder-core
> grunt dev

Running "browserSync:bsFiles" (browserSync) task
[Browsersync] Access URLs:
 --------------------------------------
       Local: http://localhost:3000
    External: http://121.199.28.62:3000
 --------------------------------------
          UI: http://localhost:3001
 UI External: http://121.199.28.62:3001
 --------------------------------------
[Browsersync] Serving files from: ./
[Browsersync] Watching files...
[Browsersync] Couldn't open browser (if you are using BrowserSync in a headless environment, you might want to set the open option to false)

```

#### 访问
```c++
       Local: http://localhost:3000
    External: http://121.199.28.62:3000
 --------------------------------------
          UI: http://localhost:3001
 UI External: http://121.199.28.62:3001
```
