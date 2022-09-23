# Kali安装zsh及常用插件


>> kali在2020.4版本后,已经将zsh设置成了默认终端。
>> 但在一些特殊的环境下,比如wsl2里的kali,仍然使用的是bash

## 查看当前默认系统shell
`echo $SHELL`

## 安装zsh和oh-my-zsh
### zsh
`sudo apt install zsh`
### oh-my-zsh
`sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"`
- 需要系统中有`curl`和`git`
- 安装完oh-my-zsh后，会自动提示是否切换到zsh。
- 如果没有提示切换系统shell，使用`chsh -s /bin/zsh`来切换。

## 安装zsh常用插件
最有用的就是3个：
- 高亮插件`zsh-syntax-highlighting`、
- 自动实例插件`zsh-autosuggestions`、
- 跳转插件`autojump`

### zsh-syntax-highlighting
````shell
cd $ZSH/custom/plugins
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git
````

### zsh-autosuggestions
```shell
cd $ZSH/custom/plugins
git clone https://github.com/zsh-users/zsh-autosuggestions.git
```
### autojump
`sudo apt install autojump`

## 配置启用插件
安装完上面3个插件后，还需要一些配置才能起作用。

打开`~/.zshrc`，找到`plugins=(git)`这一行，修改成下面的内容：
`plugins=(git autojump zsh-autosuggestions zsh-syntax-highlighting)`

然后在文件的末尾添加下面的内容
```
[[ -s /usr/local/share/autojump/autojump.zsh ]] && . /usr/local/share/autojump/autojump.zsh
source $ZSH/custom/plugins/zsh-autosuggestions/zsh-autosuggestions.zsh
source $ZSH/custom/plugins/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh
```
## 配置文件生效
终端下运行命令:
`source ~/.zshrc`

