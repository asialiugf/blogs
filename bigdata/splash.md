### root 安装开启 splash

* https://www.jianshu.com/p/bf10d259255c
* https://www.cnblogs.com/zhonghuasong/p/5976003.html
* https://github.com/scrapy-plugins/scrapy-splash

```
# pip install scrapy-splash
# docker pull scrapinghub/splash
# docker run -p 8050:8050 scrapinghub/splash
```

### scrapy 配置 setting.py

```
#SPLASH_URL = 'http://121.199.28.62:8050'
SPLASH_URL = 'http://localhost:8050'

DOWNLOADER_MIDDLEWARES = {
    'scrapy_splash.SplashCookiesMiddleware': 723,
    'scrapy_splash.SplashMiddleware': 725,
    'scrapy.downloadermiddlewares.httpcompression.HttpCompressionMiddleware': 810,
}

SPIDER_MIDDLEWARES = {
    'scrapy_splash.SplashDeduplicateArgsMiddleware': 100,
}

DUPEFILTER_CLASS = 'scrapy_splash.SplashAwareDupeFilter'

HTTPCACHE_STORAGE = 'scrapy_splash.SplashAwareFSCacheStorage'
```

### spider.py 样例
```
[rabbit@iZ23psatkqsZ ~/github/gethtml/scrapy/bigdata/bigdata/spiders]$ cat splash.py 
import sys
import chardet
import importlib
import scrapy
from scrapy_splash import SplashRequest
importlib.reload(sys)
#sys.setdefaultencoding('utf-8')

class MySpider(scrapy.Spider):
    name = "splash"
    start_urls = ['http://www.gzzbw.cn/trade/bulletin/?id=45398',]

    def start_requests(self):
        for url in self.start_urls:
            yield SplashRequest(url, self.parse, args={'wait': 0.5})

    def parse(self, response):
        print("--------------------------------------------------")
        chardet.detect(response.body)
        decodeData = response.body.encode('UTF-8')
        #response.body.decode('gbk')
        #decodeData = response.body.decode("gbk")
        #print(decodeData)
        print(response.body)
        #gbkContent = response.body.decode(chardet.detect(response.body)['encoding'])
        #utf8Content = gbkContent.encode('utf-8')
        #print (utf8Content)
        print("--------------------------------------------------")
        # response.body is a result of render.html call; it
        # contains HTML processed by a browser.
        # ...
[rabbit@iZ23psatkqsZ ~/github/gethtml/scrapy/bigdata/bigdata/spiders]$ 
```
