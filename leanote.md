## 开源leanote地址：
https://github.com/leanote
## 安装
https://github.com/leanote/leanote/wiki


由于GFW的原因，无法下载websocket源码，其实该源码在git上也有，只要下载下来，然后再GOPATH中写上相应的路径就可以通过编译，具体步骤如下 ：

1. clone git上的代码到本地,比如clone到了家目录(~/)
git clone https://github.com/golang/net.git
2. 在GOPATH中创建相应的目录,比如GOPATH=~/test
cd ~/test
mkdir -p src/golang.org/x/net/
3. 拷贝websocket源码到上面创建的目录
cd ~/test
cp -r ~/net/websocket/  src/golang.org/x/net/websocket
4. 手动build revel cmd
go build github.com/revel/cmd/revel
5.此时会在GOPATH中生成二进制文件revel，将revel拷贝到PATH中即可完成安装

#### 启动
```
$ mongod  --dbpath ./data
seenote@asiamiao:~/go$ ./revel run github.com/leanote/leanote
```
