# LInux下使用交换分区文件替代交换分区

## 增加交换分区文件
依次执行下面的命令，来新建启用交换分区

新建交换分区文件，大小为 8G：（bs*count=文件大小）
```
sudo dd if=/dev/zero of=/swapfile bs=1M count=8192
```

格式化交换分区文件：
```
sudo mkswap /swapfile
```
修正`/swapfile`的文件权限
```
chmod 0600 /swapfile
```
启用交换分区文件：
```
sudo swapon /swapfile
```

开机自动挂载:

编辑`/etc/fstab`文件
```
sudo vi /etc/fstab 
```
添加下面内容: 
```
/swapfile swap swap defaults 0 0
```

验证是否有交换分区
```
free -m
```
## 移除 swap 分区文件
执行下面的命令：

```
sudo swapoff /swapfile && sudo rm /swapfile
```
