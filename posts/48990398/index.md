# Kubuntu 22.04设置sddm使用HiDPI

作为kde默认的登录管理器，`SDDM`居然不能在kde控制中心里设置HiDPI。

一番搜索后，找到了下面的解决方法：

新建一个文件 `/etc/sddm.conf`,添加下面内容：

```
[X11]
ServerArguments=-nolisten tcp -dpi 144
```

这里的144对应着150%的缩放,重启即可生效。
