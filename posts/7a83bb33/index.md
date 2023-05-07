# Windows10关闭系统保留的7G空间


从windows 10 1903版本开始，微软为win10默认保留了大约7G左右的系统空间，用于防止因存储空间不足导致的更新失败问题。

通过下面几个命令，可以将这保留的7G空间释放出来：

以管理员打开powershell:

## 一、键入下条命令即可关闭，立即生效

```
DISM.exe /Online /Set-ReservedStorageState /State:Disabled
```

## 二、键入下条命令即可启用，立即生效

```
DISM.exe /Online /Set-ReservedStorageState /State:Enabled
```

## 三、查看当前保留空间的状态

```
DISM.exe /Online /Get-ReservedStorageState
```

