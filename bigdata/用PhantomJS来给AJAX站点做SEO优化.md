用PhantomJS来给AJAX站点做SEO优化
 AJAX
 SEO
腾讯问卷所有动态内容，全部由Ajax接口提供。

众所周知，大部分的搜索引擎爬虫都不会执行JS，也就是说，如果页面内容由Ajax返回的话，搜索引擎是爬取不到部分内容的，也就无从做SEO了。

先来看看效果
1459242677204.png

去年一整年，搜索引擎收录都少得可怜。

更致命的是，被收录的页面，其搜索引擎里面显示的标题是最原始的html标题，权重如此高的地方，却被收录了一个没什么用的标题。

在去年年底完成实施了预渲染服务后，收录量蹭蹭蹭的起来了，并且收录的标题也都全部正常了。

而这所有的一切，除了Nginx接入层的配置是需要改动业务代码外，其他全部都是旁路机制。也就是说，自己做一套，可以给所有同类型业务共用，同时不会影响现有业务的任何代码任何流程。

PhantomJS来解围
Ajax无法做SEO这个问题，困扰了我很久，后来发现PhantomJS这东西能在服务端解析HTML，瞬间这个问题不再是问题。

PhantomJS is a headless WebKit scriptable with a JavaScript API. It has fast andnative support for various web standards: DOM handling, CSS selector, JSON, Canvas, and SVG.

准备一个PhantomJS任务脚本
这里我命名为spider.js。

/*global phantom*/
"use strict";

// 单个资源等待时间，避免资源加载后还需要加载其他资源
var resourceWait = 500;
var resourceWaitTimer;

// 最大等待时间
var maxWait = 5000;
var maxWaitTimer;

// 资源计数
var resourceCount = 0;

// PhantomJS WebPage模块
var page = require('webpage').create();

// NodeJS 系统模块
var system = require('system');

// 从CLI中获取第二个参数为目标URL
var url = system.args[1];

// 设置PhantomJS视窗大小
page.viewportSize = {
    width: 1280,
    height: 1014
};

// 获取镜像
var capture = function(errCode){

    // 外部通过stdout获取页面内容
    console.log(page.content);

    // 清除计时器
    clearTimeout(maxWaitTimer);

    // 任务完成，正常退出
    phantom.exit(errCode);

};

// 资源请求并计数
page.onResourceRequested = function(req){
    resourceCount++;
    clearTimeout(resourceWaitTimer);
};

// 资源加载完毕
page.onResourceReceived = function (res) {

    // chunk模式的HTTP回包，会多次触发resourceReceived事件，需要判断资源是否已经end
    if (res.stage !== 'end'){
        return;
    }

    resourceCount--;

    if (resourceCount === 0){

        // 当页面中全部资源都加载完毕后，截取当前渲染出来的html
        // 由于onResourceReceived在资源加载完毕就立即被调用了，我们需要给一些时间让JS跑解析任务
        // 这里默认预留500毫秒
        resourceWaitTimer = setTimeout(capture, resourceWait);

    }
};

// 资源加载超时
page.onResourceTimeout = function(req){
    resouceCount--;
};

// 资源加载失败
page.onResourceError = function(err){
    resourceCount--;
};

// 打开页面
page.open(url, function (status) {

    if (status !== 'success') {

        phantom.exit(1);

    } else {

        // 当改页面的初始html返回成功后，开启定时器
        // 当到达最大时间（默认5秒）的时候，截取那一时刻渲染出来的html
        maxWaitTimer = setTimeout(function(){

            capture(2);

        }, maxWait);

    }

});
通过PhantomJS命令直接执行即可在终端中看到渲染后的html结构

phantomjs spider.js  'http://wj.qq.com/'
命令服务化
什么意思呢，因为上面是一个命令，没法很好的响应搜索引擎爬虫的请求，估我们要把他服务化。

PhantomJS自带一个Web Server Module，但总是不稳定，如前面文章所说时不时会假死。

我们就通过Node给他起一个简单的Web服务。

// ExpressJS调用方式
var express = require('express');
var app = express();

// 引入NodeJS的子进程模块
var child_process = require('child_process');

app.get('/', function(req, res){

    // 完整URL
    var url = req.protocol + '://'+ req.hostname + req.originalUrl;

    // 预渲染后的页面字符串容器
    var content = '';

    // 开启一个phantomjs子进程
    var phantom = child_process.spawn('phantomjs', ['spider.js', url]);

    // 设置stdout字符编码
    phantom.stdout.setEncoding('utf8');

    // 监听phantomjs的stdout，并拼接起来
    phantom.stdout.on('data', function(data){
        content += data.toString();
    });

    // 监听子进程退出事件
    phantom.on('exit', function(code){
        switch (code){
            case 1:
                console.log('加载失败');
                res.send('加载失败');
                break;
            case 2:
                console.log('加载超时: '+ url);
                res.send(content);
                break;
            default:
                res.send(content);
                break;
        }
    });

});
旁路服务
我们现在已经有了一个能跑预渲染的Web服务了，剩下就是要将搜索引擎爬虫的流量导入到这个预渲染的服务中，同时把结果再返回给搜索引擎爬虫。

我们使用Nginx这个接入层利器即可轻松解决这个问题。

# 定义一个Nginx的upstream为spider_server
upstream spider_server {
  server localhost:3000;
}

# 指定一个范围，默认 / 表示全部请求
location / {
  proxy_set_header  Host            $host:$proxy_port;
  proxy_set_header  X-Real-IP       $remote_addr;
  proxy_set_header  X-Forwarded-For $proxy_add_x_forwarded_for;

  # 当UA里面含有Baiduspider的时候，流量Nginx以反向代理的形式，将流量传递给spider_server
  if ($http_user_agent ~* "Baiduspider") {
    proxy_pass  http://spider_server;
  }
}
这个栗子里面仅仅对百度爬虫做了处理，可以自行把爬虫都补完整。

Free
说了这么多，我突然觉得这篇文章非常值钱。

因为，国外也有专门的服务端预渲染服务了，但他们统统要收费。

你可以根据本文的思路，自行部署一套旁路渲染服务。

附上一份新鲜收集的爬虫UA列表
360 【文档】
360Spider
HaoSouSpider
360Spider-Image
360Spider-Video
Baidu 【文档】
Google 【文档】
Googlebot
Googlebot-News
Googlebot-Video
Googlebot-Mobile
Sogou 【文档】
Sogou web spider
Sogou inst spider
Sogou Spider
