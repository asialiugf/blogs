### 移除
```c
# Stop gitlab and remove its supervision process
sudo gitlab-ctl uninstall

# Debian/Ubuntu
sudo dpkg -r gitlab-ce

# Redhat/Centos
sudo rpm -e gitlab-ce
```
### 安装gitlib-ce
- https://about.gitlab.com/installation/#ubuntu

注意安装社区版本 gitlab-ce
```c
sudo apt-get install -y curl openssh-server ca-certificates
sudo apt-get install -y postfix
curl -sS https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh | sudo bash
sudo EXTERNAL_URL =“http://gitlab.example.com”apt-get install gitlab-ce

```
### 修改密码
- https://blog.csdn.net/yin138/article/details/51394868
```c
[root@svr34 bin]# gitlab-rails console production
Loading production environment (Rails 4.2.5.2)
irb(main):001:0> user = User.where(id: 1).first
=> #<User id: 1, email: "admin@example.com", ...
irb(main):002:0> user.password=12345678
=> 12345678
irb(main):003:0> user.password_confirmation=12345678
=> 12345678
irb(main):004:0> user.save!
=> true
irb(main):005:0> quit
```
### 操作维护
```js
root@henrongyi:/opt/gitlab# vi /etc/gitlab/gitlab.rb
root@henrongyi:/opt/gitlab# gitlab-ctl stop
root@henrongyi:/opt/gitlab# gitlab-ctl restart
```

### 界面修改
以root用户登录，点击 admin area，进入配置，Appearance settings，修改logo，首页。

修改 Sign in/Sign up pages: 添加相关链接。
```js
欢迎来到技术服务之家。


[操作指引]( http://192.168.66.254/users/techservice/projects)

```
```
