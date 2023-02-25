# 在Ubuntu22.04上安装KVM虚拟化软件

>网上文章千千万，最后还得自己实践一遍

本文是亲自验证过可行的，系统是Ubuntu 22.04.1。

在行文时特意针对第一次接触kvm的人做了一些解释（其实也是写给自己看的，相当于注释了）。

## 更新系统

```shell
sudo apt update && sudo apt upgrade 
```

## 检查主机是否支持KVM虚拟化

```shell
egrep -c '(vmx|svm)' /proc/cpuinfo
```

上面命令的意思是，统计`/proc/cpuinfo`输出中出现`vmx`或者`svm`的次数。

如果不为0,则表示主机支持KVM虚拟化。

所以说，只要数字不为0就可以了。如果是0的话，说明你的电脑不支持KVM虚拟化功能，下面的内容也不用再看了。

## 安装KVM虚拟化软件

```shell
sudo apt install qemu-kvm virt-manager libvirt-daemon-system virtinst libvirt-clients bridge-utils
```

上面`virt-manager`软件是一个kvm前端图形界面，用来创建和管理虚拟机很方便。当然也有别的选择。

## 启动libvirtd服务

```shell
sudo systemctl start libvirtd
sudo systemctl start libvirtd
```

## 把当前用户加入到kvm和libvirt组

```shell
sudo usermod -aG kvm $USER
sudo usermod -aG libvirt $USER
```

## 创建一个网桥

用于kvm虚拟机进行网络连接
新建一个配置文件`01-netcfg.yaml`：

```
sudo vi /etc/netplan/01-netcfg.yaml
```

内容如下：

```text
network:
    ethernets:
       eno1:
          dhcp4: false
          dhcp6: false
    bridges:
        br0:
           interfaces: [eno1]
           dhcp4: false
           addresses: [192.168.8.33/24]
           macaddress: 18:c0:4d:2c:0d:09
           routes:
              - to: default
                via: 192.168.8.252
                metric: 100
           nameservers:
                addresses: [192.168.8.252]
           parameters:
               stp: false
           dhcp6: false
    version: 2
```

应用配置文件：

```shell
sudo netplan apply
```

上面的配置文件有几点注意事项：

1. 格式必须完全一样，连缩进的空格也不能错。
2. `eno1`是我的网卡的名称，这里需要改成你自己的网卡的名字
可以用`ip a`查看你自己的网卡的名称。
3. `addresses`、`macaddress`、`nameservers`和`routers`里的路由要根据实际情况改成你自己的。
4. 执行成功后，使用`ip a`查看会多出一个`br0`设备，同时原来的网卡，比如`eno1`会失去IP地址。这都是正常的，牵涉到一些网络知识，这里不再多说。

