# WSL修改linux发行版的主机名

wsl安装了多个linux发行版后，主机名统一都是windows的主机名，这导致各个发行版之间不是很好区分。

下面提供一个方法，能够单独修改wsl里的各个linux发行版的主机名，而不必修改windows的主机名。

在`/etc`目录下新建一个文件`wsl.conf`，即`/etc/wsl.conf`,内容如下:

```
 [network]
 generateHosts = false
 hostname = kali
```
上面的文件里,把`kali`换成你想要的主机名即可。

修改完成后，需要重启wsl方可生效。
```
wsl --shutdown
```
再打开wsl里的linux，就能看到效果了。

