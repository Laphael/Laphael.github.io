# Windows Terminal打开wsl报错的解决方法

## 问题描述

使用wsl安装完kali-linux后，从windows terminal下启动，报如下错误：
![](wsl-error.png)  

## 解决方法

打开windows terminal的kali-linux配置,修改启动的命令为`wsl.exe ~  -d kali-linux`，即中间添加一个`~`即可。
![](setting.png)  
