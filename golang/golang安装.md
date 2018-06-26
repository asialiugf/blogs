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
