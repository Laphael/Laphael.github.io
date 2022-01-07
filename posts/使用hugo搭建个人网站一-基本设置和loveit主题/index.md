# 使用Hugo搭建个人网站(一)-基本设置和LoveIt主题

>本文使用的软件的版本如下：  
>
>hugo:  0.91.2-extended  
>
>loveit主题:  0.2.10  

**特别注意**:hugo必须使用**extended**版本

关于Hugo的安装和网站的生成，本文不再详述,网上教程一大堆。  

本文主要介绍在安装过程中遇到一些问题及解决方法。  

## 设置自定义CSS文件
生成网站后,需要在网站根目录下创建一个自定义的文件：`assets\css\_custom.scss`。  

以后一些主题没提供的页面，所需要的css都保存在这个文件里，最典型的就是自定义搜索的页面。  

## 安装LoveIt主题
我喜欢用[LoveIt](https://github.com/dillonzq/LoveIt)主题，安装方式有：

1. 去官网下载zip文件，解压到`themes`目录下
2. 用`git clone`的方式
3. 用`git submodule`的方法

在这里采用`git submodule`的方法。

进入网站根目录后，运行命令：  

```
git init
git submodule add https://github.com/dillonzq/LoveIt themes/LoveIt
```
## 修改config.toml
`config.toml`是整个网站的配置文件，位于网站的根目录下。

生成网站后，自带的`config.toml`太简单了，基本没啥用,我们需要用LoveIt主题目录下的`config.toml`来替代。

把网站根目录下的`themes\LoveIt\exampleSite\config.toml`文件，拷贝到网站的根目录下。

网站首次运行前，需要对`config.toml`进行一些修改，确保首次运行网站时不会报错。  

**错误1**：关于module  

**现象**:  `Error: module "LoveIt" not found; either add it as a Hugo Module or store it in xxx`  

**原因**：`config.toml`的第10行设置了主题路径。  

**解决**: 把第10行注释掉就可以了。  
```
# 主题目录
#themesDir = "../.."
```
**错误2**：关于git  

**现象**:在运行`hugo server`的时候，有和git有关的错误提示

**原因**：这是因为在`config.toml`中，启用了  
```
# 是否使用 git 信息
enableGitInfo = true
```
**解决**：两种方法：

1. 在网站根目录下运行`git init`。
2. 把`enableGitInfo`属性改成`false`:
```
# 是否使用 git 信息
enableGitInfo = false
```

## 新建页面
有一些页面需要自己创建,比如说网站常见的`关于`(about)页面。

方法很简单，运行
```
hugo new about.md
```
生成的文件存放于`conten/posts`下。
## 新建文章
使用下面命令:
```
hugo new posts/my-first-post.md
```
这样就创建了一篇名字为`my-first-post`的文章，存放在网站根目录的`content\posts`下。  
这种新建文章的方式不推荐，有更好的方式，看这里。

## 本地预览
运行命令：
```
hugo server
```
在浏览器输入<http://localhost:1313>来预览。

`hugo serve`的默认运行环境是 `development`, 由于本地 `development` 环境的限制, 评论系统, CDN 和 fingerprint 不会在 `development` 环境下启用。
如果想开启这些特性，使用下面的命令来开启：
```
hugo serve -e production
```
至此，一个基本的Hugo网站就搭建起来了。


