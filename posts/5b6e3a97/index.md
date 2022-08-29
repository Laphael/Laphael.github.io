# Hugofy新建文章出现错误的解决方法

前文说过，我使用hugo的`page bundle`的方式来组织整个网站，并且使用`vscode`来编辑和管理网站。

vscode有一个荒野无灯大佬开发的名为`hugofy`的插件，这个插件可以很好的支持`page bundle`模式。

以前使用这个插件一直正常，今天下载了新的0.101版本的`hugo`，在使用这个插件新建文章时，却提示`Error: could not determine content directory for "/posts/xxx.md"`。

网上搜索了半天，也没发现有用的信息，最后突然想到，会不会是`hugo`版本的问题呢？

于是下载了hugo的0.84版本，再一试，果然一切正常了。

