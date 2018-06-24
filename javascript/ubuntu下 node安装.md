### ubuntu下 node安装
- https://nodejs.org/en/download/package-manager/
```java
# curl -sL https://deb.nodesource.com/setup_10.x | sudo -E bash -
# apt-get install -y nodejs 
# apt-get install -y build-essential
# npm install -g vue-cli
```
##### yarn安装
- https://yarnpkg.com/zh-Hans/docs/install#debian-stable
```c
# curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
# echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
# apt-get update 
# apt-get install yarn
```
```c
vuejs@iZ23psatkqsZ:~$ node -v
v10.5.0
vuejs@iZ23psatkqsZ:~$ npm -v
6.1.0
vuejs@iZ23psatkqsZ:~$ 
```
