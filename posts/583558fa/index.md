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
