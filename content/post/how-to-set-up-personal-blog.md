---
author: "Night Space"
title: 如何利用Hugo,Fuse,Utterances,Github Pages,Github Actions搭建一个支持Marldown语法，搜索，评论，自动化部署，在线编辑的个人博客
date: 2021-12-03T12:55:30+08:00
lastmod: 2022-08-06T17:33:00+08:00
expiryDate: 2023-11-30T23:01:00+08:00
tags:
    - 工具
    - Web
categories:
    - 工具
    - Web
---
**本文部分内容已过时，笔者目前没有更新计划**

~~哪个男孩不渴望一个拥有一个个人博客呢~~

本文将会一步一步地介绍个人博客的搭建的全过程（也是[我的个人博客](https://NightSpaceC.github.io/blog/)的搭建过程）

注：如果有不明白的地方，可以看[我的个人博客的Github仓库](https://github.com/NightSpaceC/blog)

# Hugo

Hugo是一个静态网站生成器。当然，它也可以生成我们要搭建的个人博客。

### 安装

这一步十分简单，只需要前往[Hugo在Github上的Release页面](https://github.com/gohugoio/hugo/releases/)，下载对应的版本并解压到合适的位置就行了。

你也可以下载Deb包，再通过`dpkg -i <.deb文件名>`安装

如非必需，建议下载extended版本，因为在之后可能会用到

### 使用

首先，选定一个目录来存放你的博客，你需要作如下事情：
- 在该目录下创建`config.toml`文件，用于存储配置信息（为TOML格式）
- 在该目录下创建`themes`文件夹，用于存储主题
- 在该目录下创建`content`文件夹，用于存储文章

你需要找一个Hugo主题，并把它放在`themes`文件夹下（我使用了[hugo-theme-even](https://github.com/olOwOlo/hugo-theme-even)）

依照你所使用的主题的文档或示例，编写`config.toml`和在`content`文件夹下添加文章（有时可能还要编辑`themes/主题名/config.toml`，如[hugo-theme-air](https://github.com/syui/hugo-theme-air)）

[我的config.toml](https://github.com/NightSpaceC/blog/blob/master/config.toml)

一切就绪后，你可以在目录下运行`hugo server`来让Hugo构建网站，并可以通过[http://localhost:1313](http://localhost:1313/)访问

注1：要构建设置了`draft: true`的草稿文章，请使用`hugo server --draft`

注2：命令中的`hugo`视可执行文件位置而定，除非你使用Deb包，下文也依旧如此

### 发布

测试完成后，你可以使用`hugo`来发布。此时构建结果会输出到`public`文件夹

然后，你可以利用Web服务器和浏览器，来访问它们。

注：如果你真的要就这样把它发布出去，你必须在`config.toml`中加入`baseURL`参数，值为今后访问它时使用的URL，如`https://NightSpaceC.github.io/blog/`

# 部署在Github Pages上

在此之前，你需要
- 一个Github帐号
- 安装Git
- 知晓Git与Github的基本使用方法

以上不属于本文的主要内容，不再赘述

### 创建一个用于存放个人博客的仓库

建议仓库名为`<用户名>.github.io`

其余略

### 将个人博客推送至上一步创建的仓库

1. 进行前文所述的发布操作
2. 进入`public/`目录
3. 执行`git init`
4. 执行`git remote add origin <创建的仓库的地址，如https://github.com/NightSpaceC/blog.git>`
5. 执行`git add -A`
6. 执行`git commit -m "<提交描述>"`
7. 执行`git push`（此步骤可能要求你填写帐号和私人访问密钥）

其中3,4仅第一次推送时需执行

每一次博客内容的改变，都需要执行以上操作

### 配置Github Pages

前往`https://github.com/<Github用户名>/<创建的仓库名>/settings/pages`

设置Source部分的分支（此时应该只有一个，选择即可）和目录，点击Save

此时，你的博客访问地址将会在上方显示（用这个来配置之前说的`baseURL`）

至此，你的个人博客已基本完成。如果你满足于此，就可以关闭这个页面了，否则，请继续往下看

# 使用Fuse添加搜索功能

你可以使用Fuse在前端中实现搜索

但是，直接使用Fuse是非常繁琐的。所以，我找到了一个支持Fuse的Hugo主题：[hugo-search-fuse-js](https://github.com/kaushalmodi/hugo-search-fuse-js)

注：严格来说，它并不是一个独立的主题，而是可以将Fuse功能加入到已有的主题中

使用步骤：
1. 将[hugo-search-fuse-js](https://github.com/kaushalmodi/hugo-search-fuse-js)放在themes目录下（就像之前加入自己选择的主题一样）
2. 将`config.toml`中的`theme = "<你的主题名>"`改为`theme = ["hugo-search-fuse-js", "<你的主题名>"]`
3. 创建`content/search.md`文件作为搜索页面，内容之后再说
4. 依照[hugo-search-fuse-js](https://github.com/kaushalmodi/hugo-search-fuse-js)项目的说明对主题或[hugo-search-fuse-js](https://github.com/kaushalmodi/hugo-search-fuse-js)进行相应的更改（如，我需要在`themes/hugo-theme-even/layouts/_default/baseof.html`中加入`main`和`footer`块）

[我的content/search.md](https://github.com/NightSpaceC/blog/blob/master/content/search.md)：

```md
---
title: "🔍"               #设置标题
layout: "search"          #此行为必须
outputs: ["html", "json"] #此行为必须
menu: "main"              #添加到主菜单
weight: 60                #设置权重，决定了在主菜单中显示的顺序（权重越低越靠前）
---
```
[我的themes/hugo-theme-even/layouts/\_default/baseof.html](https://github.com/NightSpaceC/blog/blob/master/themes/hugo-theme-even/layouts/_default/baseof.html)：

```html
{{ if ne .Site.Params.version "4.x" -}}
  {{ errorf "\n\nThere are two possible situations that led to this error:\n  1. You haven't copied the config.toml yet. See https://github.com/olOwOlo/hugo-theme-even#installation \n  2. You have an incompatible update. See https://github.com/olOwOlo/hugo-theme-even/blob/master/CHANGELOG.md#400-2018-11-06 \n\n有两种可能的情况会导致这个错误发生:\n  1. 你还没有复制 config.toml 参考 https://github.com/olOwOlo/hugo-theme-even/blob/master/README-zh.md#installation \n  2. 你进行了一次不兼容的更新 参考 https://github.com/olOwOlo/hugo-theme-even/blob/master/CHANGELOG.md#400-2018-11-06 \n" -}}
{{ end -}}
<!DOCTYPE html>
<html lang="{{ .Site.Language }}">
<head>
  <meta charset="utf-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1">
  <title>
    {{- block "title" . -}}
      {{ if .IsPage }}{{ .Title }} - {{ .Site.Title }}{{ else }}{{ .Site.Title }}{{ end }}
    {{- end -}}
  </title>
  {{ partial "head.html" . }}
</head>
<body>
  {{ partial "slideout.html" . }}
  <div class="container" id="mobile-panel">
    {{ if not .Params.hideHeaderAndFooter -}}
    <header id="header" class="header">
        {{ partial "header.html" . }}
    </header>
    {{- end }}
    
    {{ block "main" . }}                      <!--此行由我添加-->
    <main id="main" class="main">
      <div class="content-wrapper">
        <div id="content" class="content">
          {{ block "content" . }}{{ end }}
        </div>
        {{ partial "comments.html" . }}
      </div>
    </main>
    {{ end }}                                 <!--此行由我添加-->

    {{ if not .Params.hideHeaderAndFooter -}}
    {{ block "footer" . }}                    <!--此行由我添加-->
    <footer id="footer" class="footer">
      {{ partial "footer.html" . }}
    </footer>
    {{ end }}                                 <!--此行由我添加-->
    {{- end }}

    <div class="back-to-top" id="back-to-top">
      <i class="iconfont icon-up"></i>
    </div>
  </div>
  {{ partial "scripts.html" . }}
</body>
</html>
```
至此，你可以通过访问`<你的baseURL>/search/`来进行搜索

# 使用Utterances添加评论功能

Utterances的添加方法比较多，需视情况而定

大部分情况下，你需要进行如下操作：
1. 创建一个仓库用于存放评论（也可以使用已有仓库，如之前创建的博客仓库）
2. 安装[Github App Utterances](https://github.com/apps/utterances)，Repository access中选择Only select repositories，选择用于存放评论的仓库
3. 在[Utterances](https://utteranc.es)生成JavaScript代码，在页面模板的合适位置添加

当然，如果你像我一样幸运，使用的主题对Utterances提供了支持，那么仅需根据文档或示例修改`config.toml`即可

# 利用Github Actions进行自动化部署与在线编辑

### 这有什么用？

这当然是有用的。例如：
1. 你不再需要用自己的电脑Build再Push
2. 使得编辑文章变得简单，甚至可以直接在Github的提供的网页接口上编辑

### 步骤

1. 创建`.github/workflows/deploy-gh-pags.yml`，内容如下：

```yml
name: GitHub Pages

on:
  push:
    branches:
      - master
  pull_request:

jobs:
  deploy:
    runs-on: ubuntu-latest
    concurrency:
      group: ${{ github.workflow }}-${{ github.ref }}
    steps:
      - uses: actions/checkout@v2
        with:
          submodules: true
          fetch-depth: 0

      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: 'latest'
          extended: true

      - name: Build
        run: hugo --minify

      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        if: ${{ github.ref == 'refs/heads/master' }}
        with:
          personal_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./public
          publish_branch: gh-pages
```
3. 删除所有Hugo自动生成的文件（包括public文件夹）
4. 执行`git init`
5. 执行`git remote add origin <仓库地址>`
6. 执行`git add -A`
7. 执行`git commit -m "<提交描述>"`
8. 执行`git push`

注：必须确保刚刚添加的文件处于`master`分支，否则请自行修改`.github/workflows/deploy-gh-pags.yml`

执行完上述操作，Github会自动自行第2步中编写的Github Actions配置文件，构建你的个人博客，并将`public`文件夹下的内容放在`gh-pages`分支，设置Github Pages地址

# 大功告成

现在，你有了一个如标题所说的个人博客。每当仓库里的内容改变，Github Actions会自动帮你构建并部署。还可以随心所欲地搜索、评论。

# Update

2021-12-5 12:30:00 +08:00: 由于认为Gitalk存在安全隐患，改用Utterances
