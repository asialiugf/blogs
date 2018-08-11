
### vue多页面开发和打包正确处理方法
* https://www.jb51.net/article/138575.htm
### vue2.0之多页面的开发的示例
* https://www.jb51.net/article/133976.htm
### CommonsChunkPlugin
* https://webpack.docschina.org/plugins/commons-chunk-plugin

大型vue单页面项目优化总结
这是之前在公司oa项目优化时罗列的优化点，基本都已经完成，当时花了点心思整理的，保存在这里，方便以后其他项目用到查漏补缺。

1、打包文件中的app.js文件放入cdn，加快页面首次加载速度 
2、提取公共方法，减少js代码量 
3、提取公共组件，将统计分析的售前和售后，客户分配，客户管理，客服管理等页面的搜索条件模块化，减少了html代码量，减少了每个页面中都有的重复方法。
4、vue-router路由全部改成懒加载路由，该页面被点开时才加载该页面.vue组件，提高首页加载速度。 
5、根据页面复杂度，删除部分页面生命周期created中的window.setTimeout方法 
6、将所以v-show替换成v-if，v-if如果不为真就不会加载这段代码，而v-show还是会加载这些代码，这样加快了页面加载速度。
7、检查所有页面，删除页面中没有用到的css和data数据以及js方法，减少文件体积。
8、使用webpack插件，将常用不需要重复打包的依赖打包出来，在index.html直接引用，减小最后上线打包出来vendor文件体积，加快首屏加载速度和打包速度。（实验成功，但是没有在打包版本实施）
9、对复杂页面的弹窗使用lazyRender懒渲染组件，优化该页面的打开速度。
10、webpack build打包时去除debugger和console语句，具体修改在webpack.prod.conf.js UglifyJsPlugin插件的compress里。
11、对每个页面vuex进行优化，提到全局方法，减少大量重复代码 
12、对页面自适应样式进行优化， 用全局css代替原来的js；减少了每个页面css代码 
13、对表格进行优化，提取出列名等写出数组，减少表格html体积 0.2
14、引入顶部进度条插件，提高页面加载体验 
15、研究首页优化方法，加快单页面首页加载体验 

16、引用路径优化 webpack.base.conf.js resolve.alias
17、webpack 解析模块时应该搜索的目录优化 webpack.base.conf.js resolve.modules
18、使用webpack进行代码分离，每个页面打包成一个单独js，减少文件体积，加快加载速度 
19、把常用的依赖使用外部cdn引入，不再打包，分担服务器压力，加快页面加载速度。
20、使用webpack代码分析工具，方便针对性优化依赖和代码块。
21、同一个组件比如多个编辑页面切换时，本来的方案是使用watch.$route进行处理，经参考也可以在路由上加唯一key，保证切换路由重新渲染。参考http://www.jianshu.com/p/c315c9211146中的router-view
22、合理使用vuex，频繁切换的页面组件比如多个编辑页面，保存多个数据store，减少http请求。
23、加快webpack性能，参考地址https://jeffjade.com/2017/08/12/125-webpack-package-optimization-for-speed/中设置 test & include & exclude
24、生产环境采用webpack-parallel-uglify-plugin替换UglifyJsPlugin，提高webpack性能，参考地址https://jeffjade.com/2017/08/12/125-webpack-package-optimization-for-speed/中的增强代码代码压缩工具
25、src/api/config.js和package.json文件，实现自动分环境运行。 
