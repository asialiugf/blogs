
## ssh配置方式 
![](https://github.com/asialiugf/blogs/blob/master/image/ssh_jump.png)

#### 在 B 上 配置:
```
$ ssh -CfNg -L  B_port:D_ip:D_port -N C_user@C_ip -p C_port
```
运行这个命令时，会让你输入 C_user@C_ip 的 密码
运行成功后，会变成后台进程。
```
support@develop:~/.ssh$ ssh -CfNg -L  7003:10.26.92.137:222 -N 110.22.132.50 -p 28003

lgf@develop:~$ ssh -CfNg -L 7003:10.26.92.137:22 -N support@110.22.132.50 -p 28003
support@118.26.172.250's password: 
lgf@develop:~$ 
```
#### 在 A（使用者） 上
```
$ ssh  D_user@B_ip -p B_port
```
这样就可以从A直接登录到D上的D_user这个用户了。

## 参考！

https://blog.csdn.net/DiamondXiao/article/details/52474104
https://blog.csdn.net/cityzenoldwang/article/details/77097703
https://www.jianshu.com/p/8a087e806b1b

```
[root@i-222D71B9 ~]# ssh root@10.246.1.130 -p 22
root@10.246.1.130's password: 
Last login: Fri Aug 31 11:51:15 2018 from 10.246.1.132
[root@i-76EF01B8 ~]# 
```
