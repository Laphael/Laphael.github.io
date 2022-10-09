# Almalinux8修改默认的SSH端口号

>> 主要涉及修改的地方有SELinux、Firewall等。

这里以`2394`端口为例。

## 安装ssh服务

```
sudo dnf in openssh-server
```
## 修改sshd配置文件

```
sudo vi /etc/ssh/sshd_config
```
在`#port 22`下面新添加一行
```
Port 2394
```
## SELinux里开放端口
```
semanage port -a -t ssh_port_t -p tcp 2394
```
查看已经开放的端口
```
semanage port -l|grep ssh
```

> 如果出现`ValueError: 没有管理SELinux 策略或者无法访问存储`，解决方法在[这里](https://dnwzlx.com/posts/9a31e845/)。

## firewall开放端口
```
firewall-cmd --zone=public --permanent --add-port=2394/tcp
```
> 一定要有`--zone=public`这个参数，不然开放端口后也连接不上。

重载firewall
```
firewall-cmd --reload
```

## 启用ssh服务
```
systemctl enable ssh    ##开机自启动
systemctl restart ssh   ##重新启动ssh
```

## 关闭默认的端口
```
firewall-cmd --permanent --zone=public --remove-port=22/tcp
```

## 重启服务器

连接即可。

## 几个问题的解决

### 第一次可以连接ssh，但以后再连接则不行
问题描述：初次安装完系统后，启动sshd服务，不用修改任何配置，第一次的时候可以直接连接到服务器，以后就不行了。

解决：这个问题出现的根本原因还未深究，但是解决方法很简单：

修改`/etc/ssh/sshd_config`,把`#Port 22`前面的`#`去掉,即改成`Port 22`即可。

### 已经在firewall-cmd里放行了端口，但还是无法连接
问题描述：一直显示 `Connection timed out`

解决:这个问题,是由于在放行端口之后,没有执行`firewall-cmd --reload`造成的。

如果不执行`firewall-cmd --reload`命令，即使添加端口后显示success,并且重启了服务器,但是`firewalld`的规则依旧不生效。

所以，切记在添加需要放行的端口后，要执行一次`firewall-cmd --reload`命令。

### semanage命令找不到

安装下面软件包即可
```
dnf in policycoreutils-python-utils
```

### almalinux最小化安装默认的几个软件

最小化安装的almalinux,默认安装了`openssh`、`openssh-server`、`firewall-cmd`、`firewalld`，对于管理来说，只需要再安装上述的`semanage`即可。很是贴心和方便。

