# 正确安装WSL2的方法

近期重装系统,在安装wsl的时候,由于方法不正确,导致安装的linux运行在wsl1版本,而不是我想要的wsl2。

下面介绍一下正确安装wsl2，并设置wsl2为默认版本的方法，抄自[kali官方文档](https://www.kali.org/docs/wsl/win-kex/)。

`win`+`x` `a`，打开管理员的powershell

依次运行下面命令：
1. 

```
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux
```

2. 重启windows

3. 

```
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart

dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart

```

4. 重启windows

5. 设置默认版本为wsl2

```
wsl --set-default-version 2
```

6. 更新wsl

```
wsl --update
```

完工。
