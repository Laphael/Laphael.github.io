# LInux下使用交换分区文件替代交换分区


根据使用的磁盘格式不同，分两种设置方法：
- 非btrfs分区
- btrfs分区

# 非btrfs分区
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
sudo chmod 0600 /swapfile
```
启用交换分区文件：
```
sudo swapon /swapfile
```
## 启用交换分区文件
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
## 移除交换分区文件
执行下面的命令：

```
sudo swapoff /swapfile && sudo rm /swapfile
```

# btrfs分区
## 使用root账户
```
sudo -i
```
## 依次执行下面的命令
```
# truncate -s 0 /swapfile
# chattr +C /swapfile
# fallocate -l 4G /swapfile
# chmod 0600 /swapfile
# mkswap /swapfile
# swapon /swapfile
```
## 挂载到fstab
```
/swapfile        none        swap        defaults      0 0
```
