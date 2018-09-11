* elasticsearch install
* https://www.elastic.co/guide/en/elasticsearch/reference/current/zip-targz.html

#### 安装elasticsearch
```
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-6.4.0.zip
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-6.4.0.zip.sha512
shasum -a 512 -c elasticsearch-6.4.0.zip.sha512 
unzip elasticsearch-6.4.0.zip
cd elasticsearch-6.4.0/ 
```
#### .bashrc 环境变量
```
[charmi@dev ~]$ cat .bashrc
# .bashrc

# Source global definitions
if [ -f /etc/bashrc ]; then
        . /etc/bashrc
fi

# Uncomment the following line if you don't like systemctl's auto-paging feature:
# export SYSTEMD_PAGER=

# User specific aliases and functions
export JAVA_HOME=/usr/local/jdk1.8.0_181
export JRE_HOME=${JAVA_HOME}/jre 
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib 
export PATH=${JAVA_HOME}/bin:${JRE_HOME}/bin:$PATH 
export ES_HOME=/home/charmi/elasticsearch-6.4.0
[charmi@dev ~]$ 

```

### 安装 elasticsearch-analysis-ik 中文分词
直接下载，展开在your-es-root/plugins/ik这个目录下
* https://github.com/medcl/elasticsearch-analysis-ik/
```
1.download or compile
optional 1 - download pre-build package from here: https://github.com/medcl/elasticsearch-analysis-ik/releases
create plugin folder cd your-es-root/plugins/ && mkdir ik
unzip plugin to folder your-es-root/plugins/ik
```
