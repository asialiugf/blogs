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

#### go环境以及依赖包
GOPATH=/home/charmi/gowork 表示，使用 go get 会将依赖包放到 /home/charmi/gowork/src下。

在src这里，手动建立 golang.org/x目录，然后在这个目录下
执行以下动作：
```
  911  git clone https://github.com/golang/crypto.git
  914  git clone https://github.com/golang/net.git
  918  git clone https://github.com/golang/text.git
  919  git clone https://github.com/golang/sys.git
```
然后再 到上面执行编译 go build -a


```
[charmi@dev ~]$ cd gowork
[charmi@dev ~/gowork]$ ll
总用量 0
drwxrwxr-x. 2 charmi charmi 29 9月  15 14:53 bin
drwxrwxr-x. 3 charmi charmi 24 9月  15 14:17 pkg
drwxrwxr-x. 7 charmi charmi 96 9月  15 15:39 src
[charmi@dev ~/gowork]$ 
[charmi@dev ~/gowork]$ cd src
[charmi@dev ~/gowork/src]$ ll
总用量 4
drwxrwxr-x. 41 charmi charmi 4096 9月  15 15:29 github.com
drwxrwxr-x.  3 charmi charmi   18 9月  15 15:30 go.etcd.io
drwxrwxr-x.  3 charmi charmi   14 9月  15 15:50 golang.org
drwxrwxr-x.  2 charmi charmi    6 9月  15 15:39 google.golang.org
drwxrwxr-x.  4 charmi charmi   36 9月  15 15:29 gopkg.in
[charmi@dev ~/gowork/src]$ 
[charmi@dev ~/gowork/src]$ env | grep GO
CGO_ENABLED=0
GOROOT=/usr/local/go
GOPATH=/home/charmi/gowork
[charmi@dev ~/gowork/src]$ 
```




# 免费资源部落

* https://www.freehao123.com/caddy-web-server-https-wangpan/

* https://www.moerats.com/archives/403/

# filebrowser
* https://filebrowser.github.io/configuration/#flags
