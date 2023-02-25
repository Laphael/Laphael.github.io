# Git日常使用总结


本文用来记录在日常使用 Git 中学到的一些知识点，不定时更新。

## 常规设置

### 设置 Git 账号的邮箱和用户名

这是使用 Git 的第一步，如果没有设置，根本用不了 Git。

```
git config --global user.email "you@example.com"
git config --global user.name "Your Name"

```

### 生成 SSH 密钥

```
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"

```

## Git 子模块操作

### Git 克隆含有子模块的仓库

如果一个 Git 仓库含有子模块（submodule），现在想要在克隆主仓库的时候，想要一并克隆子模块，则在`git clone`命令加上`--recursive`参数即可。例如：

```
git clone --recursive https://github.com/chaconinc/MainProject
```

### 子模块更新之后同步操作

如果更新了 `submodule`，然后更新了父项目中依赖的版本号,则在同步的时候,需要在 `git pull`之后，调用`git submodule update`来更新` submodule `信息。

如果在`git pull`之后，忘记了调用`git submodule update`，那么极有可能再次把旧的 `submodule` 依赖信息提交上去。

因此,正确的同步子模块的步骤是:

 1. 在父目录执行`git pull`
 2. 执行`git status`，查看子模块是否有修改
 3. 执行`git submodule update`，来更新子模块。

### 子模块修改和提交

默认情况下，`git submodule update` 并不会将 `submodule` 切到任何 `branch`，因此 `submodule` 的 `HEAD` 是处于游离状态的 (‘detached HEAD’ state)。

所以在修改前，记得一定要用 `git checkout master` 将当前的`submodule`分支切换到 `master`，然后才能做修改和提交。

因此，正确的修改和提交流程是：

 1. 对 submodule 做一些修改
 2. 切到 submodule 的目录
 3. 执行git checkout master
 4. 执行 commit 和 push 命令。

