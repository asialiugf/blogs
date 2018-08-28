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

