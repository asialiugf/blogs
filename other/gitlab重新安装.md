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
### 汉化
- https://www.cnblogs.com/cheng95/p/8037865.html
- 看第三部分

下面的当前的操作
```js
cat /opt/gitlab/embedded/service/gitlab-rails/VERSION
11.0.2

#这里也是多方总结
git clone https://gitlab.com/xhang/gitlab.git
cd gitlab/
git fetch

gitlab-ctl stop

git diff origin/11-0-stable origin/11-0-stable-zh > /tmp/kkk
cd /opt/gitlab/embedded/service/gitlab-rails
git apply /tmp/kkk
patch -d/opt/gitlab/embedded/service/gitlab-rails -p1 < /tmp/kkk

#这步好像可以不用，我直接打上了
gitlab-ctl reconfigure
#启动
gitlab-ctl start
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

### 创建项目
用户可以创建普通的项目，他自己维护。

用户也可以创建组，在组内再创建项目，由小组成员共同维护。

- 用户登录后， 选择 group -- 再选择  new group
- 组创建完成后，可以点击这个组， 进去后，可以看到左侧有 member,merge request,setting等。
- 点击member添加小组成员。并选择成员的相应的权限。
- 在右侧，可以在这个小组内，创建相应的项目。
- 小组成员如果对某项目有修改，提交时，会产生一个 merge request，需要由小组的创建者来确认是否可以合并。

