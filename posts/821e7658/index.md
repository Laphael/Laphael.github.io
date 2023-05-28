# Linux下设置ResilioSync

Resilio Sync在linux下的安装方式就不讲了，官方提供了大量发行版的安装包，根据自己的linux发行版下载安装即可。

## 管理界面

linux下，Resilio Sync必须使用web界面来进行管理，地址是: <https://localhost:8888/gui/>

首次登录时，会要求你进行注册。**注意**：在注册时，只需要填写用户名即可，**不必**填写密码。

## 用户和权限问题

默认情况下，Resilio Sync会新建一个名为`rslsync`的用户，来进行相关的同步和管理。

这种方式很不友好，因为在同步的时候，只能同步到`rslsync`这个用户权限的文件夹里。

虽然官方提供了方法来使用当前用户进行同步，但是在实际操作过程中还是有问题。

因此，还是推荐使用另一种官方提供的方法

## 使用当前用户来启动和管理Resilio Sync

### 修改resilio服务

```
sudo vi /usr/lib/systemd/user/resilio-sync.service 
```

找到：

```
WantedBy=multi-user.ta~~rget
```

修改成：

```
WantedBy=default.target
```

### 使用当前用户自动启动服务

```
sudo systemctl --user enable resilio-sync
```

