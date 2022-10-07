# SELinux相关问题的解决

为了服务器的安全性考虑,rhel系列默认会启用的SELinux最好不要关闭,这也导致了一些坑的出现,下面记录一下解决方法。

## 更改SSH端口号后,ssh服务无法启动

这个问题是因为selinux里，只默认开放了22端口。如果只在`/etc/ssh/sshd_config`里修改了端口号，就会出现上面的问题。

### 解决方法

在selinux里开放对应的端口号,这里以2394为例：
```shell
semanage port -a -t ssh_port_t -p tcp
```
详细改端口的教程看这里.

## ValueError: 没有管理SELinux 策略或者无法访问存储

这个问题的详细解释看[这里](https://blog.siphos.be/2014/10/migrating-to-selinux-userspace-2-4-small-warning-for-users/)。

大体意思是：由于SELinux版本升级，新版本的SELinux的策略模型存储在`/var/lib/selinux`，而不是之前的`/etc/selinux`里。

由于各方面的原因（主要是` portage_t`域名必须被移除），SELinux在版本升级的时候，并不会自动修改这个存储路径，需要手动进行修改。

作者也说了，出现上面的错误，并不意味着系统崩坏，也不会影响SELinux正常起作用，只会影响` semanage`或者`setsebool`这些工具的使用。

而且手动修改只需要运行一次就可以了。

### 解决方法

1. 重建一下SELinux的存储路径

```
/usr/libexec/selinux/semanage_migrate_store
```

2. 删除旧的策略模型：
```
rm -rf /etc/selinux/mcs/modules
```

3. 重启服务器就可以了。
