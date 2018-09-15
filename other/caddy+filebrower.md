### 源码安装 

#### root下先安装 node yarn
```
 1000  yum install epel-release
 1001  yum install nodejs
 1002  which yarn
 1003  wget https://dl.yarnpkg.com/rpm/yarn.repo -O /etc/yum.repos.d/yarn.repo
 1004  yum install yarn
 1005  which yarn
 1006  which node
[root@dev ~]# 
```
#### filebrowser源码安装
```
git clone --recurse-submodules https://github.com/filebrowser/filebrowser
cd filebrowser
yarn install

```




# 免费资源部落

* https://www.freehao123.com/caddy-web-server-https-wangpan/

* https://www.moerats.com/archives/403/

# filebrowser
* https://filebrowser.github.io/configuration/#flags
