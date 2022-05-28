# 解决Hugo网站使用LoveIt主题后图片刷新不出来的问题

hugo博客部署在github page上。

最近几天发现，博客的文章和disqus评论都是正常的，唯独涉及图片的部分，一直显示Loading加载不出来。

一番查找后，发现锅在LoveIt主题上，确切的说，是在LoveIt主题使用的cdn--`https://cdn.jsdelivr.net`上。

## 问题的根源
LoveIt主题使用的图片加载技术叫`lazysizes` ，它的官方解释说明是这样的：

* 基于`lazysizes`自动转换图片为懒加载

我的理解是，`lazysizes`可以自动把图片压缩成设置好的大小，并且按需加载。这样就能够加快网页的打开速度了。

`lazysizes`依赖的核心js是` lazysizes.min.js`。

而` lazysizes.min.js`存放于`https://cdn.jsdelivr.net`上。如果`https://cdn.jsdelivr.net`出现访问问题，图片自然也就加载不出来。

公共CDN属于基础网络设施，全球有大量的项目依赖于它，一般不会出现问题。

很不幸的是，这次恰恰就是`https://cdn.jsdelivr.net`出现了问题，而且好像只有在中国大陆出现了问题。

> 更进一步，是只有移动、电信的网络出现了问题，联通访问还是正常的（我为什么会知道？因为我有电信、联通、移动三网的环境:stuck_out_tongue:）。

这就是此次Hugo网站加载不出来图片的根本原因。

## 解决方法
知道问题根源了,解决方法也很简单--换一个cdn就可以了。

最简单的方法是：

还是使用`jsdelivr.net`的cdn，但是换一个域名，目前`gcore.jsdelivr.net`这个域名是可以使用的。

具体步骤：

编辑文件

`themes\LoveIt\assets\data\cdn\jsdelivr.yml`

修改第二行,原来是:

`libFiles: https://cdn.jsdelivr.net/npm/`

修改为:

`libFiles: https://gcore.jsdelivr.net/npm/`

问题解决




