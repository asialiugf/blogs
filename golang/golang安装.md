### golang安装
* https://golangtc.com/download

### 下载
- https://studygolang.com/dl
### 安装
- https://blog.csdn.net/netdxy/article/details/79432017
root用户下：
执行：
```c
tar -C /usr/local -xzf go$VERSION.$OS-$ARCH.tar.gz
```
```c
vi /etc/profile
export GOPATH=/go
export GOROOT=/usr/local/go
export PATH=$PATH:$GOROOT/bin:$GOPATH/bin
```
```c
go version
```
#### download go1.11
```
wget https://studygolang.com/dl/golang/go1.11.linux-amd64.tar.gz
tar xvf go1.11.linux-amd64.tar.gz
mv ./go /usr/local/go-1.11
```
#### /etc/profile
```
export GOROOT=/usr/local/go-1.11
export GOBIN=$GOROOT/bin
export GOPATH=$HOME/gowork
export PATH=$PATH:$GOBIN:$GOPATH/bin
```

