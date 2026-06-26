---
title: hexo+volantis个人博客搭建问题记录
date: 2025-09-28 22:37:24
tags:
- 问题记录
categories:
- 技术学习
---

### 来源

Volantis 是一个功能丰富、高度模块化的 Hexo 博客主题。得益于其强大的模块化特性，您可以轻松搭建一个极简风格的博客，也可以仿照官网搭建一个多人协作的、包含文档模块的大体量综合型博客。

> *<a href="https://volantis.js.org/" target="_blank">点击volantis教程学习！</a>*
### 快速开始

    hexo new [layout] <title>
&nbsp;&nbsp;&nbsp;&nbsp;post是默认的布局，但你也可以提供自己的布局。 您可以通过编辑 _config.yml 中的 default_layout 设置来更改默认布局。具体示例:

    hexo new post "文章标题"
如果你想新建一个页面（比如“关于我”），可以用：

    hexo new page "about"

写完文章后，生成静态文件并运行在本地：

    hexo generate
    hexo server

部署到远端仓库
- 如果你之前在 _config.yml 里配置过 deploy 部分（比如用 hexo-deployer-git），只需要：
    
    hexo deploy

运行要清除旧的缓存文件，使用hexo clean清除掉Hexo生成过程中的缓存文件和旧的静态文件，保证下一次生成和部署时用的是最新内容   

    hexo clean

为什么需要 hexo clean
- 如果你修改了文章、主题或配置，但旧的缓存还在，可能导致生成的页面不更新或出现错误。

- 有时部署失败（比如你遇到的 Spawn failed），就是因为缓存和生成内容不一致。

所以常见的修复流程就是：

    hexo clean
    hexo g
    hexo d


### 安装流程

<font size = 3, color="teal">环境配置条件</font>

    Hexo: 5.4 ~ 6.x
    hexo-cli: 4.3 ~ latest
    node.js: 16.x LTS ~ latest LTS
    npm: 8.x ~ latest LTS

<font size = 3, color="teal">快速体验</font>

~~~
git clone https://github.com/volantis-x/demo.git && cd demo && npm i && hexo s
~~~

<font size = 3, color="teal">下载与安装</font>

1.  在 blog/_config.yml 文件中找到并修改

        theme: volantis

2.  下载主题
    - 在终端中输入：
    ~~~
    npm i hexo-theme-volantis
    ~~~


### 配置与使用

<font size = 3, color="teal">站点配置</font>

```yaml
# _config.yml - 博客配置文件

# 网站标题
title: my blog
```

<font size = 3, color="teal">主题配置</font>

 to be continue