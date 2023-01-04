# Wsl和vmware共存的问题

、

## 问题描述
近期在使用wsl2和vmware时遇到了一些问题

如果要使用wsl2，则必须要在`windows功能`里开启`虚拟机平台`。

但是开启`虚拟机平台`后，打开vmware、virtualBox等虚拟机时会提示

- VMware Workstation与Device/Credential Guard不兼容
- VT不支持、
- 该主机cpu类型不支持虚拟化性能计数器，开启模块VPMC的操作失败，未能启动虚拟机

等等错误。

## 解决方法

解决方案很简单，就是把`wsl2`降为`wsl1`。

具体方法如下：

### 1.转换已经有的虚拟机

比如我现在的`kali-linux`虚拟机使用的是`wsl2`，想降为`wsl1`，则：

```
wsl --set-version kali-linux 1
```

### 2.删除`虚拟机平台`
管理员权限打开`powershell`，输入：
```
Disable-WindowsOptionalFeature -Online -FeatureName VirtualMachinePlatform
```
重启电脑。

