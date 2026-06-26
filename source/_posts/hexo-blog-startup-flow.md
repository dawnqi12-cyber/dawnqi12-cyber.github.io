---
title: Hexo 个人博客项目启动流程
date: 2026-06-26 22:00:00
tags:
- Hexo
- Volantis
- 博客搭建
categories:
- 技术学习
---

这篇文章记录当前个人博客项目的启动、写作、预览和部署流程。项目基于 Hexo 8，使用 Volantis 主题，适合在换电脑、重新拉取仓库或交给别人协作时快速恢复开发环境。

## 一、准备环境

本项目需要先安装 Node.js 和 npm。建议使用 Node.js LTS 版本，安装完成后在终端检查：

```bash
node -v
npm -v
```

如果能正常输出版本号，就可以进入项目目录继续操作。

## 二、安装依赖

进入博客源码目录后，执行：

```bash
npm install
```

这个命令会根据 `package.json` 和 `package-lock.json` 安装 Hexo、Volantis 主题、渲染器、部署插件等依赖。

项目中常用依赖包括：

- `hexo`
- `hexo-theme-volantis`
- `hexo-server`
- `hexo-deployer-git`
- `hexo-renderer-marked`
- `hexo-renderer-stylus`

## 三、本地启动预览

安装完成后执行：

```bash
npm run server
```

浏览器访问：

```text
http://localhost:4000
```

如果页面能够正常打开，就说明博客已经在本地启动成功。

## 四、常用命令

项目在 `package.json` 中配置了几个常用脚本：

```bash
npm run clean
npm run build
npm run server
npm run deploy
```

它们分别对应：

```bash
hexo clean      # 清理缓存和 public 目录
hexo generate   # 生成静态页面
hexo server     # 启动本地预览
hexo deploy     # 部署到远程仓库
```

平时修改配置、主题或文章后，如果发现页面没有更新，可以先执行：

```bash
npm run clean
npm run build
npm run server
```

这样可以避免旧缓存影响预览结果。

## 五、新建和编辑文章

新建文章使用：

```bash
npx hexo new post "文章标题"
```

Hexo 会在 `source/_posts/` 目录生成对应的 Markdown 文件。文章开头一般包含 front matter，例如：

```markdown
---
title: 文章标题
date: 2026-06-26 22:00:00
tags:
- Hexo
categories:
- 技术学习
---
```

正文直接使用 Markdown 编写即可。写完后先本地预览，确认排版、图片、代码块和链接都正常。

## 六、项目目录说明

当前博客源码中比较重要的目录如下：

```text
.
├── _config.yml               # Hexo 站点配置
├── _config.volantis.yml      # Volantis 主题配置
├── package.json              # 依赖和 npm 脚本
├── scaffolds/                # 新文章和页面的模板
├── source/
│   ├── _posts/               # 博客文章
│   ├── about/                # 关于页
│   ├── categories/           # 分类页
│   ├── tags/                 # 标签页
│   └── images/               # 图片资源
└── themes/                   # 本地主题文件
```

通常只需要重点维护 `source/_posts/`、`source/images/`、`_config.yml` 和 `_config.volantis.yml`。

## 七、部署到 GitHub Pages

当前站点配置中使用 `hexo-deployer-git` 部署，目标仓库配置在 `_config.yml`：

```yaml
deploy:
  type: git
  repository: https://github.com/dawnqi12-cyber/dawnqi12-cyber.github.io.git
  branch: main
```

部署前需要确认 GitHub 仓库存在，并且本机已经配置好 GitHub 账号权限。确认无误后执行：

```bash
npm run deploy
```

Hexo 会先生成静态页面，再把生成结果推送到配置的 GitHub 仓库分支。

## 八、推荐完整流程

日常写作和发布可以按这个顺序来：

```bash
npm install
npx hexo new post "新文章标题"
npm run clean
npm run build
npm run server
npm run deploy
```

如果只是修改已有文章，跳过新建文章那一步即可。

## 九、注意事项

- `node_modules/` 是依赖目录，不需要提交到 Git。
- `public/` 是生成后的静态文件目录，不建议提交到源码仓库。
- `db.json` 是 Hexo 生成的缓存文件，也不需要提交。
- 修改主题配置后建议先执行 `npm run clean`。
- `_config.yml` 里的 `url` 应该在正式发布前改成真实博客地址。

以上流程可以覆盖从本地启动到写作发布的大部分场景。以后换电脑或重新部署时，按照这篇文章执行即可快速恢复博客环境。
