### filebrower 源码安装 
* https://github.com/filebrowser/community/blob/master/builds.md
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

#### 安装 rice (用于嵌入静态资源）
```
# Install rice tool if not present
if ! [ -x "$(command -v rice)" ]; then
  go get github.com/GeertJohan/go.rice/rice
fi

# Embed the assets using rice
cd lib
rice embed-go
```
```
# go get github.com/GeertJohan/go.rice/rice
[root@dev ~/go/src/github.com/GeertJohan/go.rice/rice]# go build
mv ./rice /usr/bin
```


#### filebrowser源码安装

##### 下载
```
git clone --recurse-submodules https://github.com/filebrowser/filebrowser
cd filebrowser
yarn install

```
##### 编译前端
```
cd cd filebrowser
cd frontend
# Get dependencies up-to-date
yarn install
# Build the frontend
yarn build
```
##### 编译后端
```
cd filebrowser
cd lib
rice embed-go
cd ../cli
go get -v ./...
cd ../cli
export CGO_ENABLED=0
go build -a
cd ..
cp cli/filebrowser ./
```



# 免费资源部落

* https://www.freehao123.com/caddy-web-server-https-wangpan/

* https://www.moerats.com/archives/403/

# filebrowser
* https://filebrowser.github.io/configuration/#flags
