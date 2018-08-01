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
