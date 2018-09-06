
![](https://github.com/asialiugf/blogs/blob/master/image/ssh_jump.png)

在 B 上 配置:
```
support@develop:~/.ssh$ ssh -CfNg -L  7003:10.246.92.132:22 -N 118.26.172.250 -p 20003

lgf@develop:~$ ssh -CfNg -L 7003:10.246.92.132:22 -N support@118.26.172.250 -p 20003
support@118.26.172.250's password: 
lgf@develop:~$ 


```

https://blog.csdn.net/DiamondXiao/article/details/52474104
https://blog.csdn.net/cityzenoldwang/article/details/77097703
https://www.jianshu.com/p/8a087e806b1b

```
[root@i-222D71B9 ~]# ssh root@10.246.1.130 -p 22
root@10.246.1.130's password: 
Last login: Fri Aug 31 11:51:15 2018 from 10.246.1.132
[root@i-76EF01B8 ~]# 
```
