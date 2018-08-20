# 震惊！Selenium分手PhantomJS

## centos7 安装 Headless chrome + Selenium + ChromeDriver
* https://blog.csdn.net/zhuyiquan/article/details/79537623
* https://blog.csdn.net/u013849486/article/details/79466359
* https://blog.csdn.net/xds2ml/article/details/52982748
#### /etc/yum.repos.d/google.repo
```c
[root@iZ23psatkqsZ /tmp]# cat /etc/yum.repos.d/google.repo   
[google]
name=Google-x86_64
baseurl=http://dl.google.com/linux/rpm/stable/x86_64
enabled=1
gpgcheck=0
gpgkey=https://dl-ssl.google.com/linux/linux_signing_key.pub
[root@iZ23psatkqsZ /tmp]# 
```
#### yum install google-chrome-stable

```c
yum install  \
 ipa-gothic-fonts \
 xorg-x11-fonts-100dpi \
 xorg-x11-fonts-75dpi \
 xorg-x11-utils \
 xorg-x11-fonts-cyrillic \
 xorg-x11-fonts-Type1 \
 xorg-x11-fonts-misc -y
```
#### google-chrome-stable
```c
[root@iZ23psatkqsZ /tmp]# google-chrome-stable -version
Google Chrome 68.0.3440.106 
[root@iZ23psatkqsZ /tmp]# 
解决之。再次运行 
google-chrome-stable --no-sandbox --headless --disable-gpu --screenshot https://www.suning.com/。访问成功，生成截图。 
```
# https://sites.google.com/a/chromium.org/chromedriver/downloads
```
Latest Release: ChromeDriver 2.41
Supports Chrome v67-69
```
```c
 1140  wget https://chromedriver.storage.googleapis.com/2.41/chromedriver_linux64.zip
 1143  unzip chromedriver_linux64.zip 
 1147  mv chromedriver /usr/bin
```
## 注意要在 非root用户下运行
```python
[root@iZ23psatkqsZ /tmp]# cat ee.py
# -*- coding:utf-8 -*-
from selenium import webdriver
from pyvirtualdisplay import Display
 
display = Display(visible=0, size=(800,600))
display.start()
driver = webdriver.Chrome("chromedriver")
driver.get("http://www.baidu.com")
print (driver.page_source)
driver.quit()
display.stop()
[root@iZ23psatkqsZ /tmp]# 
```
## [rabbit@iZ23psatkqsZ /tmp]$ python3 ee.py > baidu.html

```python
[rabbit@iZ23psatkqsZ /tmp]$ python cc.py
习近平对首个“中国医师节”作出重要指示
[rabbit@iZ23psatkqsZ /tmp]$ cat cc.py
# coding=utf-8
from selenium import webdriver
from selenium.webdriver.chrome.options import Options

url="http://news.163.com/"
chrome_options = Options()
# specify headless mode
chrome_options.add_argument("--headless")
browser = webdriver.Chrome(chrome_options=chrome_options)
browser.set_page_load_timeout(300)
browser.set_script_timeout(300)
browser.get(url)
title=browser.find_elements_by_xpath('//div[@id="js_top_news"]/h2/a')
print (title[0].get_attribute('innerHTML'))
browser.quit()
[rabbit@iZ23psatkqsZ /tmp]$ 
```
## 使用slenium+chromedriver实现无敌爬虫
* https://blog.csdn.net/u010986776/article/details/79266448?from=singlemessage
## Python+Selenium WebDriver API：浏览器及元素的常用函数及变量整理总结
* https://www.cnblogs.com/yufeihlf/p/5764807.html

```python
[rabbit@iZ23psatkqsZ ~/github/gethtml]$ python mm.py
新闻
hao123
地图
视频
贴吧
学术
登录
设置
更多产品
百度
把百度设为主页关于百度About  Baidu百度推广
©2018 Baidu 使用百度前必读 意见反馈 京ICP证030173号  京公网安备11000002000001号 
百度一下，你就知道
[rabbit@iZ23psatkqsZ ~/github/gethtml]$ 
[rabbit@iZ23psatkqsZ ~/github/gethtml]$ cat mm.py
# 导入selenium的浏览器驱动接口
from selenium import webdriver

# 要想调用键盘按键操作需要引入keys包
from selenium.webdriver.common.keys import Keys

# 导入chrome选项
from selenium.webdriver.chrome.options import Options
# 创建chrome浏览器驱动，无头模式（超爽）
chrome_options = Options()
chrome_options.add_argument('--headless')
driver = webdriver.Chrome(chrome_options=chrome_options)

# 加载百度页面
driver.get("http://www.baidu.com/")
# time.sleep(3)

# 获取页面名为wrapper的id标签的文本内容
data = driver.find_element_by_id("wrapper").text
print(data)

# 打印页面标题 "百度一下，你就知道"
print(driver.title)

# 生成当前页面快照并保存
driver.save_screenshot("baidu.png")

# 关闭浏览器
driver.quit()
[rabbit@iZ23psatkqsZ ~/github/gethtml]$ 
```

```python

```
* https://blog.csdn.net/data_learn/article/details/78516246

## 滑块！！！
* http://www.aneasystone.com/archives/2018/03/python-selenium-geetest-crack.html


```python
[rabbit@iZ23psatkqsZ ~/github/gethtml]$ cat zg.py
# 导入selenium的浏览器驱动接口
from selenium import webdriver

# 要想调用键盘按键操作需要引入keys包
from selenium.webdriver.common.keys import Keys

# 导入chrome选项
from selenium.webdriver.chrome.options import Options
# 创建chrome浏览器驱动，无头模式（超爽）
chrome_options = Options()
chrome_options.add_argument('--headless')
driver = webdriver.Chrome(chrome_options=chrome_options)

driver.get("http://zzcg.ccgp.gov.cn/zbgg/index.jhtml")
mm = driver.find_element_by_css_selector("[class='List2 Top8']")
element = driver.find_element_by_xpath('//div[@class="List2 Top8"]/ul//li')
#driver.findElement(By.xpath("(//ul[@class='dropdown-menu inner selectpicker']/li)[last()]/a/span[@class='text']")).click()
#driver.findElement(By.xpath("(//div[@class='List2 Top8']/ul//li)[last()]")).click()
#xx = driver.find_elements_by_xpath("(//div[@class='List2 Top8']/ul//li)[last()]")  # ok!!
#driver.find_elements_by_xpath("(//div[@class='List2 Top8']/ul//li)[last()]/a[contains(text(),'Re-Submit')]").click()
#driver.find_element_by_xpath("(//div[@class='List2 Top8']/ul//li)[last()]/a[contains(text(),'Re-Submit')]").click()
#mm = driver.find_element_by_xpath("(//div[@class='List2 Top8']/ul//li)[last()]/a").click()
#mm = driver.find_element_by_xpath("(//div[@class='List2 Top8']/ul//li)[last()]")
mm = driver.find_elements_by_xpath("(//div[@class='List2 Top8']/ul//li)")
url = mm[5].find_element_by_xpath('.//a[@href]')
print( "333333333333333333")
#print(url)
print(url.text)
yy = url.get_attribute("href")
print(url.get_attribute("href"))
driver.get(yy)
ttime = driver.find_element_by_xpath("(//div[@class='TxtCenter Padding10'])")
print(ttime.text)

print(driver.title)

print( "888888888888888888")

print(driver.title)

print(len(mm))
#for lii in mm:
for i in range(len(mm)):
    link = mm[i-1].find_element_by_xpath('.//a[@href]')
    url = link.get_attribute("href")
    print(url)
    driver.get(url)
    tti = driver.find_element_by_xpath("(//div[@class='TxtCenter Padding10'])")
    print(tti.text)
    #link.click()

links = mm.find_elements_by_tag_name("a")
for link in links:
    #if not "_blank" in link.get_attribute("target") and ("google" in link.et_attribute("href") or not "http" in link.get_attribute("href")):  
    xx = link.click()
    print(xx)
    driver.back()

print(links)
#print(xx)

li=driver.find_elements_by_xpath('.//div[class="List2 Top8"]/ul//li')
print(li)

print ('*'*10)

#List<WebElement> webElements = driver.findElements(By.xpath("//div[@class='List2 Top8']/ul/li"));
# time.sleep(3)

# 获取页面名为wrapper的id标签的文本内容
#data = driver.find_element_by_id("wrapper").text
#print(data)
print(driver.title)
driver.save_screenshot("baidu.png")
driver.quit()
[rabbit@iZ23psatkqsZ ~/github/gethtml]$ 
```


# -------------------------------完美 分割线 ------------------------------------
### selenium + firefox
```python
  964  wget https://download-ssl.firefox.com.cn/releases/firefox/61.0/en-US/Firefox-latest-x86_64.tar.bz2
  965  wget https://github.com/mozilla/geckodriver/releases/download/v0.21.0/geckodriver-v0.21.0-arm7hf.tar.gz
  966  ll
  967  mv Firefox-latest-x86_64.tar.bz2 ./source/
  968  mv geckodriver-v0.21.0-arm7hf.tar.gz ./source/
  969  cd sour*
  970  ll
  971  tar xvf Firefox-latest-x86_64.tar.bz2 
  972  ll
  973  tar xvf geckodriver-v0.21.0-arm7hf.tar.gz 
  974  ll
  975  mv geckodriver /usr/bin
  976  cd firefox/
  977  ll
  978  ls
  979  cd ..
  980  mv firefox/ /usr/local
  981  cd /usr/local/firefox/
  982  ll
  983  cd /etc
  984  ll
  985  vi profile
  986  cd 
  987  ll
  988  cd source
  989  ll
  990  rm geckodriver-v0.21.0-arm7hf.tar.gz 
  991  wget https://github.com/mozilla/geckodriver/releases/download/v0.21.0/geckodriver-v0.21.0-linux64.tar.gz
  992  ll
  993  tar xvf geckodriver-v0.21.0-linux64.tar.gz 
  994  ll
  995  mv geckodriver /usr/bin
  996  pip list | grep fir
  997  pip list | grep sele
  998  firefox -version
  999  firefox --version
 1000  ll
 1001  history
[root@iZ23psatkqsZ ~]# 
```
## 处理export DISPLAY
```
 1034  firefox -version
 1035  Xvfb :99 -ac &
 1036  export DISPLAY=:99    
 1037  python mm.py
```
## 测试脚本
```python
[rabbit@iZ23psatkqsZ ~/github/gethtml/firefox]$ cat kk.py
#! /usr/bin/env python3
# coding=utf-8
'''
Created on 2016-8-16
@author: Jennifer
Project:使用Firefox浏览器
'''
from selenium import webdriver 

driver = webdriver.Firefox()
driver.get('http://www.baidu.com')
#driver.find_element_by_id('kw').send_keys('Selenium')
#driver.find_element_by_id('su').click()
print(driver.title)

driver.quit()
[rabbit@iZ23psatkqsZ ~/github/gethtml/firefox]$ 
```
# -------------------------------完美 分割线 ------------------------------------


### CentOS 7.x环境下搭建: Headless chrome + Selenium + ChromeDriver 实现自动化测试
* https://blog.csdn.net/zhuyiquan/article/details/79537623

### install phantomjs
* http://phantomjs.org/quick-start.html
``` go
  987  cd github/
  988  wget https://bitbucket.org/ariya/phantomjs/downloads/phantomjs-2.1.1-linux-x86_64.tar.bz2
  989  ll
  990  bzip2 -d phantomjs-2.1.1-linux-x86_64.tar.bz2
  991  ll
  992  tar xvf phantomjs-2.1.1-linux-x86_64.tar 
  993  ll
  994  mv phantomjs-2.1.1-linux-x86_64 /usr/local/phantomjs
  995  cd /usr/local/phantomjs
  996  ll
  997  cd ./bin
  998  ll
  999  vi /etc/profile
```

### python 爬虫
* https://www.cnblogs.com/zhaof/p/7173094.html
* http://python.jobbole.com/86405/
* http://python.jobbole.com/89164/?utm_source=blog.jobbole.com&utm_medium=relatedPosts
* http://www.cnblogs.com/zhaof/p/7092400.html

### pyspider
##### Pyspider框架 —— Python爬虫实战之爬取 V2EX 网站帖子
* http://developer.51cto.com/art/201611/520688.htm
##### 源码分析
* https://blog.csdn.net/weixin_37947156/article/details/76487954
* http://docs.pyspider.org/en/latest/Quickstart/
* http://www.pyspider.cn/book/.html
* http://www.pyspider.cn/book/pyspider/self.crawl-16.html
##### pyspider的使用
* https://blog.csdn.net/weixin_37947156/article/details/76495144

##### Python3实现抓取javascript动态生成的html网页功能示例 selenium
* https://www.jb51.net/article/121810.htm

##### http://f2er.info/article/29 则个才是终极方案
* https://www.jianshu.com/p/d4c1e9565488
* http://f2er.info/article/29
##### 改百度首页
* https://blog.csdn.net/tuposky/article/details/50531195
