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
#### GOPATH
我们在安装Go的时候看到需要设置GOPATH变量，Go从1.1版本到1.7必须设置这个变量，而且不能和Go的安装目录一样，这个目录用来存放Go源码，Go的可运行文件，以及相应的编译之后的包文件。所以这个目录下面有三个子目录：src、bin、pkg

从go 1.8开始，GOPATH环境变量现在有一个默认值，如果它没有被设置。 它在Unix上默认为$HOME/go,在Windows上默认为%USERPROFILE%/go。

GOPATH允许多个目录，当有多个目录时，请注意分隔符，多个目录的时候Windows是分号，Linux系统是冒号，当有多个GOPATH时，默认会将go get的内容放在第一个目录下。

以上 $GOPATH 目录约定有三个子目录：
* src 存放源代码（比如：.go .c .h .s等）
* pkg 编译后生成的文件（比如：.a）
* bin 编译后生成的可执行文件（为了方便，可以把此目录加入到 $PATH 变量中，如果有多个gopath，那么使用${GOPATH//://bin:}/bin添加所有的bin目录）
以后我所有的例子都是以mygo作为我的gopath目录

* https://astaxie.gitbooks.io/build-web-application-with-golang/content/zh/01.2.html

* https://github.com/asialiugf/blogs/blob/db2e2c8b89bebd39d3a2179a65b47609c68c35a0/other/caddy%2Bfilebrower.md

#### 标准库
* https://studygolang.com/pkgdoc
* https://github.com/golang/gddo
* https://godoc.org/
