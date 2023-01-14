# 使用Hugo搭建个人网站(七)-Hugo和Github Action正确修改文章的最后更新日期

偏向技术或者工具使用方面的博客，难免会随着时间和技术的更新，对文章进行一些修改。

使用hugo一年多以来，总体来说非常满意，但是一直没有解决一个问题，就是显示文章的最后修改时间。

今天又想起这个长期以来的"bug"，搜索了一下，发现已经有解决方法了。

## 修改网站配置文件

在网站的配置文件 `config.toml`里添加如下字段:

``` toml
[frontmatter]
  lastmod = [":git", "lastmod", ":fileModTime"]
```

另外，还需要启用`GitInfo`功能：

```toml

enableGitInfo = true
```

## 修改Github Action 配置文件

完整的配置文件看[这里](https://dnwzlx.com/posts/95a48970/)

需要注意的是下面这些：

```toml
      - name: Checkout
        uses: actions/checkout@v3 # 引用actions/checkout这个action，与所在的github仓库同名
        with:
          submodules: true  # Fetch Hugo themes (true OR recursive) 获取submodule主题
          fetch-depth: 0    # Fetch all history for .GitInfo and .Lastmod
      - name: Disable quotePath
        run: git config --global core.quotePath false
```

### fetch-depth: 0

`fetch-depth` 的默认值是 1，也就是说它默认只会拉取分支最近的一次 commit，这可能会导致一些文章的 `GitInfo` 变量无法获取到。

设置为 0 代表拉取所有分支的所有提交。

### Disable quotePath

默认情况下，当文件名中包含中文的时候，git 会使用引号`" "`把文件名括起来。

但是这会导致 action 中无法读取 `:GitInfo` 变量，所以这里需要关闭`quotePath`功能。

这样就可以正确显示文章最后更新时间了，问题解决。

