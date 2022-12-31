# 修改Conda默认启动的Python环境

## 问题描述

安装完 Conda 之后，开启终端将默认进入 `base` 环境。

但是，`base` 经常不是我们所需要的环境。因此，每次进入终端都得 `conda activate` 到想要的环境，很麻烦。

## 解决方法

假如想让`conda`默认启动`py3s`这个python环境

### Linux

在`shell`的配置文件最后，添加`source activate py3s`

例如`zsh`，则
```
cd

vi .zshrc
```
在最后添加
```
source activate py3s
```

### windows
打开`powershell`
运行命令：
```
notepad $PROFILE
```
如果提示不存在则新建即可。


