# KDE播放Samba共享里的视频文件


## 问题描述
使用`debian 11 `+ `KDE` 桌面，通过samba协议连接到NAS后，发现无法播放NAS上的视频文件。

## 解决方法
安装`kio-fuse`即可
```
sudo apt install kio-fuse
```

debian确实是稳定，在chrome和nvidia驱动方面非常出色，不比windows差，但是在一些小的易用性方面，还是有坑
