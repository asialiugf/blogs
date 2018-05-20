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
 sed -i s/UGG/UBEE/g `grep UGG -rl --include="*.h" ./` 
```

### 将换行换成了,
```
sed ':a;N;$!ba;s/\n/,/g' zz > zz1
```

### 查找
```
find .|xargs grep -ri "不能访问此页面" -l
```

### vim jsbeautify要安装node
```
 apt-get install nodejs-legacy
 apt-get install astyle
 
```

### ps + kill
```
kill -9 $(ps -ef | grep DataSer | grep -v grep | awk '{print $2}')
```

### du -h
du 使用详解 linux查看目录大小 linux统计目录大小并排序 查看目录下所有一级子目录文件夹大小 du -h --max-depth=1 |grep [
发表于 2014-05-05 - 浏览：6764 评论：0 收藏 0
常用命令

du -h --max-depth=1 |grep [TG] |sort   #查找上G和T的目录并排序

du -sh    #统计当前目录的大小，以直观方式展现



du -h --max-depth=1 |grep 'G' |sort   #查看上G目录并排序

du -sh --max-depth=1  #查看当前目录下所有一级子目录文件夹大小

du -h --max-depth=1 |sort    #查看当前目录下所有一级子目录文件夹大小 并排序



du -h --max-depth=1 |grep [TG] |sort -nr   #倒序排
