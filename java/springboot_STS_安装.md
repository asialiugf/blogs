### springboot 网站
* https://spring.io/

这个网页中间部分，找到
 - Spring Boot Reference Manual
 - Getting Started Guide

点击 “Getting Started Guide” 进入到
- https://spring.io/guides/gs/spring-boot/
- Spring Tool Suite (STS)

在这个网页中下载
- https://spring.io/tools/sts/all

### 安装
- https://www.cnblogs.com/jedjia/p/spring_boot.html

### maven安装
* http://maven.apache.org/download.cgi
* https://www.cnblogs.com/eagle6688/p/7838224.html

1. 前往https://maven.apache.org/download.cgi下载最新版的Maven程序：
2. 将文件解压到D:\Program Files\Apache\maven目录下:
3. 新建环境变量MAVEN_HOME，赋值D:\Program Files\Apache\maven
4. 编辑环境变量Path，追加%MAVEN_HOME%\bin\;

#### 修改 C:\D盘\apache-maven-3.5.4\conf\settings.xml
增加以下配置，采用阿里源。
```js

    <mirror>
        <id>alimaven</id>
        <name>aliyun maven</name>
        <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
        <mirrorOf>*</mirrorOf>
    </mirror>      
```
