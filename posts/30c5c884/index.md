# CentOS7设置本地yum源

有时会遇到生产环境部署在内网上，无法使用在线yum的情况。

这时候就需要使用centos的安装镜像，挂载到本地后建立本地yum来安装软件了。

下面介绍具体方法

## 挂载ISO镜像文件

1. 如果使用的是虚拟化平台，已经把ISO镜像文件挂载到虚拟光驱了，则：
`mount /dev/cdrom /mnt`
2. 如果是把ISO文件上传到主机上，比如`/home`目录下，则：
`mount -o loop /home/CentOS-7-x86_64-DVD-2009.iso /mnt`

## 设置本地yum源

### 备份系统的repo文件

```
cd /etc/yum.repos.d 
mkdir bak 
mv *.repo ./bak 
```

### 新建本地yum源的repo文件

```
vi /etc/yum.repos.d/local.repo
```

内容如下：

```
[base] 
name=CentOS 
baseurl=file:///mnt 
enabled=1 
gpgcheck=0 
```

### 清除和建立缓存

```
yum clean all 
yum makecache 
```

