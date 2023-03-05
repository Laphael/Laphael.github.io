# Ubuntu禁用错误报告

Ubuntu在日常使用时，经常会弹出一个错误报告窗口，提示你“XXX已经崩溃，是否发送错误信息”之类的内容。

这个挺烦人的，毕竟在windows下我也从来没发送过系统错误报告。

出现这个弹窗，是因为ubuntu默认安装并启用了`Appport`这个服务。如果不想提示错误汇报，禁用这个软件就可以解决了。

1. 编辑Apport配置文件

```shell
sudo vi /etc/default/apport
```

2. 修改配置

把`enabled=1`改成`enabled=0`

重启生效。

