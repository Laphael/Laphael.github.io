# Linux下使用miniconda的一些设置

在`kali`的`zsh`环境中使用`miniconda`，遇到一些问题，这里记录一下解决的过程。

对于其它的linux来说，应该是通用的。

## 使用命令行安装miniconda
```
mkdir -p ~/miniconda3
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O ~/miniconda3/miniconda.sh
## 如果使用bash，则把下面命令中的zsh替换成bash：
zsh ~/miniconda3/miniconda.sh -b -u -p ~/miniconda3
rm -rf ~/miniconda3/miniconda.sh
## 如果使用zsh,则执行：
~/miniconda3/bin/conda init zsh
## 如果使用bash，则执行：
~/miniconda3/bin/conda init bash
```

## oh-my-zsh主题配置
默认情况下，使用双行显示的oh-my-zsh主题，会在第一行的前面显示`(base)`的标志:
![](default-base.png)  
这就非常的不美观，因此需要对oh-my-zsh的主题做一些修改。

这里以`gnzh`主题为例。
1. 取消掉`miniconda`默认的环境显示：
```
conda config --set changeps1 False
```
2. 添加下面代码到gnzh主题的配置文件中
```
local conda_prompt='$(conda_prompt_info)'
conda_prompt_info() {
    if [ -n "$CONDA_DEFAULT_ENV" ]; then
        echo -n "%{$terminfo[bold]$fg[yellow]%}($CONDA_DEFAULT_ENV) %{$reset_color%}"
    else
        echo -n ''
    fi
}

```
注意：`$conda_prompt`一定要写成函数的形式，否则PROMPT只会在启动`zsh`的时候获取一次`conda`环境信息，后续切换环境就不会再改变了。

3. 在gnzh主题配置文件的合适地方添加`${conda_prompt}`

例如下面：
```
PROMPT="╭─${user_host}${conda_prompt}${current_dir}\$(ruby_prompt_info)${git_branch}
╰─$PR_PROMPT "
RPROMPT="${return_code}"
```
退出并重新进入`shell`，即可看到效果。

## 默认不启用conda的base环境
安装完`miniconda`后，每次登录系统都会自动启用默认的`base`环境，这就取代了系统自带的`python2`和`python3`。

如果想使用系统自带`python`环境，有两种方法，推荐使用方法2：
- 方法1:
手动退出base环境回到系统自带的环境
```
conda deactivate 
```
- 方法2:
取消`miniconda`的`base`环境自启动设置：
```
conda config --set auto_activate_base false
```
要进入`base`的话，可以手动运行命令`conda activate base`

加外，如果想让`miniconda`的`base`环境恢复自启动，可以：
```
conda config --set auto_activate_base true
```
