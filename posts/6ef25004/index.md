# Windows Terminal添加到右键菜单

## 下载`windows terminal`的图标
在[这里](https://github.com/microsoft/terminal/tree/main/res)下载。

## 创建文件夹
```
mkdir "%USERPROFILE%\AppData\Local\terminal"
```
把下载的图标放到新创建的`terminal`文件夹里。

## 写入注册表

新建一个注册表文件，内容如下：
```
Windows Registry Editor Version 5.00

[HKEY_CLASSES_ROOT\Directory\background\shell\在此处打开Windows Terminal]
"Icon"="%USERPROFILE%\\AppData\\Local\\terminal\\terminal.ico"

[HKEY_CLASSES_ROOT\Directory\background\shell\在此处打开Windows Terminal\command]
@="wt -d \"%V\""


```
双击导入注册表即可。

## 修改启动文件夹
打开`windows terminal`的`.json`配置文件，找到如下位置并修改：
```
"profiles": 
    {
        "defaults": {
            "startingDirectory":  null
        },
```

`"startingDirectory":  null`这一行是新添加的。

完工。
