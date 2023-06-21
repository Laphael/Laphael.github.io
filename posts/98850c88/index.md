# 使用Hugo搭建个人网站(二)-LoveIt主题自定义搜索

LoveIt主题自带的搜索插件是lunr和algolia，这两个都不太好用。

在此，我们使用Hugo专用的搜索插件[hugo-search-fuse-js](https://github.com/kaushalmodi/hugo-search-fuse-js)来替代LoveIt主题自带的搜索。

## 安装hugo-search-fuse-js

1、网站根目录下，使用下面命令来安装：

```
git submodule add https://github.com/kaushalmodi/hugo-search-fuse-js themes/hugo-search-fuse-js
```

2、把`hugo-search-fuse-js`添加到站点配置文件`config.toml`里，如下所示：

`theme = ["hugo-search-fuse-js", "LoveIt"]`

hugo-search-fuse-js要在最前面，后面跟着的是主题的名字。

3、新建一个`content/search.md`文件

```
hugo new search.md
```

内容如下：

```
+++
title = "Search"
layout = "search"
outputs = ["html", "json"]
[sitemap]
  priority = 0.1
+++
```

4、修改页面模板文件`baseof.html`
把主题目录下的\themes\LoveIt\layouts\_default\baseof.html拷贝到站点根目录下的`layouts\_default\baseof.html`。

在拷贝后的`baseof.html`里插入两段代码：`{{ block "main" . }}{{ end }}`和`{{ block "footer" . }}{{ end }}`

如下所示：

```
<div class="wrapper">
    {{- partial "header.html" . -}}
    <main class="main">
        <div class="container">
   {{ block "main" . }}{{ end }}
            {{- block "content" . }}{{ end -}}
        </div>
    </main>
    {{- partial "footer.html" . -}}
 {{ block "footer" . }}{{ end }}
</div>
```

## 添加搜索按钮

在站点配置文件里添加一个新的按钮给搜索功能使用，如下：

```
   [[languages.zh-cn.menu.main]]
    identifier = "search"
    pre = "<i class='fas fa-fw fa-search'></i>"
    name = "搜索"
    url = "/search/"
    weight = 6
```

## 关闭LoveIt主题默认的搜索插件

在站点配置文件里把默认的搜索插件关闭，如下：

```
[params]
  [params.app]
    [params.search]
      enable = false
```

## 修改搜索页面的样式

如果对该插件生成的搜索页面样式不满意，可以自行修改。

在网站`assets\css\_custom.scss`文件里添加下面内容：

```
/* 搜索页面 */
.search {
    position: relative;
    padding-top: 3.5rem;
    padding-bottom: 1rem;
    width: 57.5%;
    margin: 0 auto;
    background: white;
    opacity: .95;
}
[theme=dark] .search {
    background: #3a3535;
}

[theme=dark] .search header,
.search header {
    background-color: #f8f8f8;
}

[theme=dark] .search header:hover,
.search header:hover {
    -webkit-box-shadow: none;
    box-shadow: none;
}

.search header h1 {
    padding-left: 1rem;
    background: white;
}
[theme=dark] .search header h1 {
    background: #3a3535;
}

[theme=dark] .search input,
.search input {
 height: initial;
    width: initial;
    color: initial;
 background-color: white;
 margin: 0 0 0 1rem;
 border-width: 2px;
    border-style: inset;
    border-color: initial;
    border-image: initial;
 -webkit-border-radius: 0;
    -moz-border-radius: 0;
    border-radius: 0;
}

.search #search-results {
    padding-left: 1rem;
    padding-right: 1rem;
}

[theme=dark] a:active, [theme=dark] a:hover {
    color: #2d96bd;
}

.search hr {
    margin-left: 1rem;
    margin-right: 1rem;
}
```

## 优化中文搜索效果

这个搜索功能借助了Fuse.js模糊搜索引擎，针对中文搜索需要做一些优化。

编辑`themes\hugo-search-fuse-js\static\js\search.js`，下面的示例已经添加了部分中文注释：

```
// Options for fuse.js
let fuseOptions = {
  shouldSort: true, // 是否按分数对结果列表排序
  includeMatches: true, //  是否应将分数包含在结果集中。0分表示完全匹配，1分表示完全不匹配。
  tokenize: true,
  matchAllTokens: true,
  threshold: 0.2, // 匹配算法阈值。阈值为0.0需要完全匹配（字母和位置），阈值为1.0将匹配任何内容。
  location: 0, // 确定文本中预期找到的模式的大致位置。
  /**
   * 确定匹配与模糊位置（由位置指定）的距离。一个精确的字母匹配，即距离模糊位置很远的字符将被视为完全不匹配。
   *  距离为0要求匹配位于指定的准确位置，距离为100则要求完全匹配位于使用阈值0.2找到的位置的20个字符以内。
   */
  distance: 100,
  maxPatternLength: 64, // 模式的最大长度
  minMatchCharLength: 2, // 模式的最小字符长度
  keys: [
    {name:"title",weight:0.8},
    {name:"tags",weight:0.5},
    {name:"categories",weight:0.5},
    {name:"contents",weight:0.4}
  ]
};
```

这里和中文搜索有关的主要就3个选项：

1. threshold
2. location
3. distance

threshold是阈值，这个参数搭配distance使用。如果阈值填了0.0，相当于distance没有意义。

location填0就行，distance填100就足够了，太大了会导致搜索到过多的结果。

最张参数如下：

```
  threshold: 0.2,
  location: 0,
  distance: 100

```

可以根据个人情况来修改这几个参数的值。

