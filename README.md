# Dawnqi's Blog

这是一个基于 Hexo 8 和 Volantis 主题搭建的个人博客项目。仓库主要保存博客源码、主题配置、文章 Markdown 和部署配置，生成后的静态文件位于 `public/`，不会提交到 Git。

## 技术栈

- Hexo: `8.0.0`
- 主题: `hexo-theme-volantis`
- 运行环境: Node.js + npm
- 部署插件: `hexo-deployer-git`
- 文章目录: `source/_posts/`

## 目录结构

```text
.
├── _config.yml               # Hexo 站点配置
├── _config.volantis.yml      # Volantis 主题配置
├── package.json              # npm 脚本和依赖
├── scaffolds/                # Hexo 文章、页面模板
├── source/                   # 博客源码内容
│   ├── _posts/               # 博客文章
│   ├── about/                # 关于页
│   ├── categories/           # 分类页
│   ├── tags/                 # 标签页
│   └── images/               # 图片资源
└── themes/                   # 本地主题目录
```

## 本地启动

首次启动前先安装依赖：

```bash
npm install
```

启动本地预览服务：

```bash
npm run server
```

默认访问地址为：

```text
http://localhost:4000
```

## 常用命令

```bash
npm run clean    # 清理 Hexo 缓存和 public 目录
npm run build    # 生成静态站点
npm run server   # 本地预览
npm run deploy   # 部署到 _config.yml 中配置的 Git 仓库
```

等价的 Hexo 命令：

```bash
npx hexo clean
npx hexo generate
npx hexo server
npx hexo deploy
```

## 写作流程

新建文章：

```bash
npx hexo new post "文章标题"
```

文章会生成在 `source/_posts/` 目录。写完后建议执行：

```bash
npm run clean
npm run build
npm run server
```

本地检查无误后再提交代码或部署。

## 部署说明

当前 `_config.yml` 中的部署目标为：

```yaml
deploy:
  type: git
  repository: https://github.com/dawnqi12-cyber/dawnqi12-cyber.github.io.git
  branch: main
```

部署前请确认本机已经配置好 GitHub 凭据，并且目标仓库存在。执行：

```bash
npm run deploy
```

Hexo 会将生成后的静态页面推送到配置的仓库分支。

## 维护建议

- `node_modules/`、`public/`、`.deploy*/` 和 `db.json` 属于生成或缓存内容，不应提交。
- 修改主题或站点配置后，先运行 `npm run clean` 再重新生成。
- `_config.yml` 中的 `url` 当前仍是示例地址，正式发布前建议改成个人站点真实地址。
- 重要文章建议使用英文或拼音文件名，避免不同系统之间出现编码问题。
