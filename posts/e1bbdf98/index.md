# Linux下安装utools

安装utools主要是为了它的词典插件。

最新的3.3版本，可以在kali下正常安装，但是启动时却报错：
```
Uncaught Exception:
Error: libcrypto.so.1.1:
...
```
可以看出来，是`libcrypto.so.1.1`的问题

在网上找了个最简单的解决方法，就是使用`wps`的`libcrypto.so.1.1`。

首先安装wps，然后执行
```
sudo cp /opt/kingsoft/wps-office/office6/libcrypto.so.1.1 /opt/uTools/
```

即可正常运行`utools`了。
