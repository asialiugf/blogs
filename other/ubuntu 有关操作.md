#### 查看版本
```
root@d:~#  lsb_release -a
LSB Version:    core-9.20160110ubuntu0.2-amd64:core-9.20160110ubuntu0.2-noarch:security-9.20160110ubuntu0.2-amd64:security-9.20160110ubuntu0.2-noarch
Distributor ID: Ubuntu
Description:    Ubuntu 16.04.3 LTS
Release:        16.04
Codename:       xenial
root@d:~# 
```
#### 查找 并 替换
```c
 sed -i s/uGG/uBEE/g `grep uGG -rl --include="*.cpp" ./`
```

### 将换行换成了,
```
sed ':a;N;$!ba;s/\n/,/g' zz > zz1
```
