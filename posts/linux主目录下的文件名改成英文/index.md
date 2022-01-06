# Linux主目录下的文件名改成英文


之所以要改成英文名称,是为了在终端下操作方便。

一、打开终端，在终端中输入命令:
```bash
export LANG=en_US
xdg-user-dirs-gtk-update
```
跳出对话框询问是否将目录转化为英文路径,同意并关闭。

二、在终端中输入命令:
```bash
export LANG=zh_CN
```
重新启动系统，系统会提示更新文件名称，选择不再提示,并取消修改。
