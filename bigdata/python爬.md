### 中间件 八.Scrapy 学习下Spider中间件Spider Middlewares
* https://blog.csdn.net/beyond_f/article/details/74626311

### DormyMo/SpiderKeeper 监控
* https://github.com/DormyMo/SpiderKeeper
* https://www.v2ex.com/amp/t/330272
* https://www.v2ex.com/t/354532
* https://github.com/ioiogoo/scrapy-monitor/tree/master

### python爬虫scrapy之如何同时执行多个scrapy爬行任务
* https://blog.csdn.net/ff_smile/article/details/78871228

### Scrapy学习笔记(8)-使用signals来监控spider的状态
* http://jinbitou.net/2016/12/19/2271.html
### 在pipeline中,可以监听spider的启动和关闭,代码如下:
* https://my.oschina.net/u/2396236/blog/1801853

* http://baijiahao.baidu.com/s?id=1585117266335386391&wfr=spider&for=pc
# Scrapy爬虫入门教程十三 Settings（设置）
* https://blog.csdn.net/Inke88/article/details/61915020
# Scrapy爬虫框架（实战篇）【Scrapy框架对接Splash抓取javaScript动态渲染页面】
* https://www.cnblogs.com/518894-lu/p/9067208.html
* https://blog.csdn.net/mingzznet/article/details/51325053
# splash安装
* https://www.cnblogs.com/jclian91/p/8590617.html
# splash初体验
* https://www.jianshu.com/p/4052926bc12c

```python
 1003  which pip
 1004  pip install scrapy 
 1005   
 1006  pip list | grep pyasn
 1007  pip install --upgrade pyasn1
 1008  pip list | grep pyasn
 1009  pip install --upgrade pyasn1-modules  
 1010  pip list | grep pyasn
 1011  pip install scrapy 
 1012  pip install scrapyd
 1013  pyspider
 1014  pip install scrapyjs
 1015  history
[root@iZ23psatkqsZ ~]# 
```
```c
 1014  pip install scrapyjs
 1015  history
 1016  pip3 install scrapy-splash
 1017  yum install docker
 1018  service docker start
 1019  dock pull scrapinghub/splash
 1020  docker pull scrapinghub/splash
 1021  docker run -p 8050:8050 scrapinghub/splash
```
```python
[root@iZ23psatkqsZ ~]# docker run -p 8050:8050 scrapinghub/splash
2018-08-19 16:11:58+0000 [-] Log opened.
2018-08-19 16:11:59.152317 [-] Splash version: 3.2
2018-08-19 16:11:59.674561 [-] Qt 5.9.1, PyQt 5.9, WebKit 602.1, sip 4.19.3, Twisted 16.1.1, Lua 5.2
2018-08-19 16:11:59.674763 [-] Python 3.5.2 (default, Nov 23 2017, 16:37:01) [GCC 5.4.0 20160609]
2018-08-19 16:11:59.674904 [-] Open files limit: 1048576
2018-08-19 16:11:59.675022 [-] Can't bump open files limit
2018-08-19 16:11:59.915070 [-] Xvfb is started: ['Xvfb', ':1672607086', '-screen', '0', '1024x768x24', '-nolisten', 'tcp']
QStandardPaths: XDG_RUNTIME_DIR not set, defaulting to '/tmp/runtime-root'
2018-08-19 16:12:05.774052 [-] proxy profiles support is enabled, proxy profiles path: /etc/splash/proxy-profiles
2018-08-19 16:12:06.655100 [-] verbosity=1
2018-08-19 16:12:06.655308 [-] slots=50
2018-08-19 16:12:06.655454 [-] argument_cache_max_entries=500
2018-08-19 16:12:06.655977 [-] Web UI: enabled, Lua: enabled (sandbox: enabled)
2018-08-19 16:12:06.656100 [-] Server listening on 0.0.0.0:8050
2018-08-19 16:12:06.670048 [-] Site starting on 8050
2018-08-19 16:12:06.670233 [-] Starting factory <twisted.web.server.Site object at 0x7f567a3157f0>
```
