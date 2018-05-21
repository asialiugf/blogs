# 各种安装包
```c++
apt-get update
apt-get upgrade
apt-get install vim-gui-common  
apt-get install vim-runtime  
apt-get install git
apt-get install libssl-dev
apt-get lib32ncurses5 lib32z1
apt-get install cmake
apt-get install uuid-dev


```
# vim安装
在普通用下：
```c++
tar xvf vim.tar.gz
```
# git 
在相关的项目目录下执行以下命令。
```c++
git config credential.helper store
```

# uWS 安装
```c++
 git clone https://github.com/uNetworking/uWebSockets.git
 cd uWebsockets
 make
 make install
 ```
 
 # grpc安装
 #### protobuf 安装
 ```c++
$ git clone https://github.com/google/protobuf.git
$ cd protobuf
$ git submodule update --init --recursive
$ ./autogen.sh
$ ./configure
$ make
$ make check
$ sudo make install
$ sudo ldconfig # refresh shared library cache.
 ```
 #### grpc安装
 ```c++
 $ git clone -b $(curl -L https://grpc.io/release) https://github.com/grpc/grpc
 $ cd grpc
 $ git submodule update --init
 $ make
 $ [sudo] make install
 ```
 
