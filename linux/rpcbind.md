目前，越来越多的项目不再是单机，而是趋向于分布式部署，所以在分布式部署就需要文件共享，例如A服务器上传的图片，希望在B服务器上也可以访问。因此就需要跨机器共享文件，在这里就简单的采用nfs+rpcbind实现跨机器的文件共享。

1、安装nfs和rpcbind

      检查自己的电脑是否已经默认安装了nfs和rpcbind：

# rpm -aq | grep nfs

nfs-utils-1.2.3-54.el6.x86_64

nfs4-acl-tools-0.3.3-6.el6.x86_64 

nfs-utils-lib-1.1.5-9.el6.x86_64

# rpm -aq | grep rpcbind 

rpcbind-0.2.0-11.el6.x86_64 

