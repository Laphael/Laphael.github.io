# 使用命令行删除windows Defender

在使用windows server的时候，可以使用命令行来删除windows defender.

## 卸载 Windows Server 2016 或 Windows Server 2019 上的 Microsoft Defender 防病毒

```
Uninstall-WindowsFeature -Name Windows-Defender
```

## 关闭 Microsoft Defender 防病毒 GUI

```
Uninstall-WindowsFeature -Name Windows-Defender-GUI
```

注意，上面这条关闭GUI的命令，在windows server 2022下无效。

