# AlmaLinux9安装xrdp远程桌面

>>>系统是almalinux 9.0,桌面是gnome。

## 启用epel-release

```
sudo dnf in epel-release
sudo dnf up -y
```

## 安装xrdp

```
sudo dnf in xrdp -y
```

## 配置文件

```
sudo vi ~/.xinitrc
```

添加下面内容：

```
exec /usr/bin/gnome-session
```

## 启动xrdp服务

```
sudo systemctl start xrdp 
sudo systemctl enable xrdp 
```

## 添加防火墙例外

```
firewall-cmd --zone=public --permanent --add-port=3389/tcp
firewall-cmd --reload
```

## 客户端

在linux下，推荐使用`remmina`来连接远程桌面。`remmina`具备保存会话、用户名、密码和自动调整分辨率等功能，值得尝试

## 排错

现象：能够成功登录，但是登录后却是黑屏
解决：很大可能是没有退出远程机器的登录界面导致的。

如果是非服务器版本的windows，通过远程桌面登录后，会自动注销当前正在登录的用户。

Linux下没有这个功能，需要手动退出所有当前在图形界面下的登录，然后才能正常使用xrdp登录。

