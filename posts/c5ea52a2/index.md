# 手动编译vyos最新稳定版

vyos官方只有night-daily的镜像，没有稳定版的镜像。

自己手动编译的话，vyos的官方文档又写的不明不白的，感觉有故意的成份。

于是网上找了篇文章，简单明了的介绍了如何编译安装最新稳定LTS版本。

## vyos不同版本对应的编译环境
官方推荐是使用`debian`来搭建编译环境.

vyos版本和debian版本对应关系如下：

VyOS 1.3(equuleus) .<---> . Debian 10 (Buster) 
VyOS 1.4(sagitta, current) . <---> . Debian 11 (Bullseye)

## 安装Debian 10虚拟机
使用vmware，详情不再赘述。

不会安装虚拟机的话也就不用再想着手动编译vyos了。

因为要编译最新的lts 1.3.2版本，所以这里安装debian 10.
## 给debian 10安装docker环境

```
$ sudo apt-get update
$ sudo apt-get install -y apt-transport-https ca-certificates curl gnupg2 software-properties-common
$ curl -fsSL https://download.docker.com/linux/debian/gpg | sudo apt-key add -
$ sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/debian $(lsb_release -cs) stable"
$ sudo apt-get update
$ sudo apt-get install -y docker-ce

##  添加当前用户到docker组
$ sudo usermod -aG docker <Your username>
```

## 编译ISO镜像
目前，最新的稳定版是`1.3.2 LTS`

```
git clone -b 1.3.2 --single-branch https://github.com/vyos/vyos-build
cd vyos-build
docker run --rm -it --privileged -v $(pwd):/vyos -w /vyos vyos/vyos-build:current bash
./configure --architecture amd64 --build-by "j.randomhacker@vyos.io" --build-type "release" --version "LTS 1.3.2"
sudo make iso
```

## 编译好的ISO镜像下载
在github上找到了个repo，专门发布编译好的lts镜像，地址在[这里](https://github.com/naa0yama/vyos-build-lts)。
