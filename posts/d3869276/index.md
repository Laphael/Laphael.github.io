# Dell R730XD风扇调速

虽然Dell R730XD号称静音，但是默认的风扇转速还是让人爱不了。

但是好在，R730xd是支持风扇调速的，下面是具体方法

## 安装ipmitool

最简单的方法是安装一台linux的虚拟机，例如debian 12,`impitool`在很多linux下都在官方的软件仓库里。

## 关闭风扇自动调速

```
ipmitool -I lanplus -H 192.168.1.11 -U username -P passwd raw  0x30 0x30 0x01 0x00
```

- `192.168.1.11`改为你的R730xd的服务器地址
- `username`改为你的iDrac的用户名
- `passwd`改为你的iDrac的密码

**必须**先做这一步，不然下面的手动设置风扇转速不起作用

## 手动设置风扇转速

```
ipmitool -I lanplus -H 192.168.1.11 -U username -P passwd raw 0x30 0x30 0x02 0xff 0x0f
```

上面手动设置的风扇转速为15%。

我感觉15%是比较合适的，静音又能保障一定的散热能力。

