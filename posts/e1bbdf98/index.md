# Linux下安装utools

`utools`是个快速启动工具，支持mac、win、linux三大平台，自带丰富的插件。

本人安装utools主要是为了它的词典插件，用来查词非常方便。

最新的3.3版本，在kali linux下可以正常安装，但是启动时却报错：
```
Uncaught Exception:
Error: libcrypto.so.1.1:
...
```
可以看出来，是`libcrypto.so.1.1`的问题。

在网上找了个最简单的解决方法，就是使用其它软件自带的`libcrypto.so.1.1`，比如`wps`的。

首先安装wps，然后查看`libcrypto.so`

```
dpkg -S libcrypto.so
```
输出如下：
```
sublime-text: /opt/sublime_text/libcrypto.so.1.1
wps-office: /opt/kingsoft/wps-office/office6/libcrypto.so
wps-office: /opt/kingsoft/wps-office/office6/libcrypto.so.1.1
libssl3:amd64: /usr/lib/x86_64-linux-gnu/libcrypto.so.3

```
可以看到，我安装的`sublime-text`和`wps`都带有`libcrypto.so.1.1`，这里使用`wps`的。

```
sudo cp /opt/kingsoft/wps-office/office6/libcrypto.so.1.1 /opt/uTools/
```

即可正常运行`utools`了。
