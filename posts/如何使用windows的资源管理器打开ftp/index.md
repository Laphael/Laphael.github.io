# 如何使用windows的资源管理器打开FTP

我是习惯用专用的FTP软件来连接FTP,比如`FlashFXP`等,

但是也有人喜欢用windows资源管理器连接FTP服务器，且不用老输入用户名、密码[^1]。

最简单的实现方法如下：

在桌面右键－新建－快捷方式－在对象位置框中输入  
```
explorer.exe ftp://user:pass@host/ 
```
然后下一步，随便输入一个名称，完成即可。

**注**：以上`user`为FTP用户名，`pass`为FTP密码，`host`为FTP服务器地址。

这样建立的快捷方式是用资源管理器打开（直接填地址会当成internet快捷方式用浏览器打开），且自动登录，并可修改快捷方式图标。

[^1]: 学自[这篇文章](http://shuai.be/archives/ftp-in-explorer/)
