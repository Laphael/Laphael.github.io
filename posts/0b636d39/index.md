# Kali下python2 Python3 Pip的安装

最近在wsl里用kali。

由于wsl里的kali只提供了一个最基本的环境，python2、python3、pip2、pip3都需要自己安装，这里记录一下安装方法。

## python3和pip3
```
sudo apt install python3 python3-dev python3-pip
```

## python2和pip2
### python2安装
```
sudo apt install python2 python2-dev
```
### pip2安装
```
wget https://bootstrap.pypa.io/pip/2.7/get-pip.py

sudo python2 get-pip.py
```

这样，在使用的时候，只需要输入`python2`、`pip2`和`python3`、`pip3`即可。

如果想输入`python`的时候，默认调用`python3`,可以安装`python-is-python3`这个软件包
```
sudo apt install python-python3
```
