### openldap
```c
yum -y install openldap compat-openldap openldap-clients openldap-servers openldap-servers-sql openldap-devel migrationtools
 slapd -VV
 yum install openldap-servers openldap-clients
 service slapd start
 cd /etc/openldap/slapd.d/cn=config/
 yum install -y phpldapadmin
 
```

```c
 yum install ansible
[root@localhost ~]# rpm -qa | grep epel
epel-release-7-11.noarch
[root@localhost ~]# 
[root@localhost ~]# yum remove epel-release-7-11.noarch
yum install ansible
ls /etc/ansible

```

```c
[root@localhost tmp]# ssh-keygen  
[root@localhost tmp]# ssh-copy-id -i ~/.ssh/id_rsa.pub -p 22 support@192.168.66.253
[root@localhost tmp]# ssh-copy-id -i ~/.ssh/id_rsa.pub -p 22 support@192.168.66.254
[root@localhost tmp]# ssh -p 22 support@192.168.66.253
[root@localhost tmp]# ssh -p 22 support@192.168.66.254
```
