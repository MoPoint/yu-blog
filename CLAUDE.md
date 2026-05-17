# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 项目简介

这是一个基于 [Hexo](https://hexo.io/) v8.1.2 构建的个人博客站点，使用 [Fluid](https://github.com/fluid-dev/hexo-theme-fluid) v1.9.9 主题。博客内容为中文，时区设置为 Asia/Shanghai。部署方式为 GitHub Pages（通过 `hexo-deployer-git`）。

## 常用命令

```bash
# 启动本地开发服务器（默认 http://localhost:4000）
npm run server

# 构建静态文件到 public/ 目录
npm run build

# 清理生成的静态文件
npm run clean

# 部署到远程仓库（GitHub Pages）
npm run deploy
```

```bash
# 完整发布流程
hexo clean && hexo generate && hexo deploy
```

## 创建内容

```bash
# 新建文章
hexo new "文章标题"

# 新建草稿
hexo new draft "草稿标题"

# 新建页面（如关于页）
hexo new page "页面名称"
```

文章源文件位于 `source/_posts/` 目录，使用 Markdown 格式，支持 Front-matter 定义元数据。

## 目录结构

```
yu-blog/
├── _config.yml            # Hexo 主配置（站点信息、URL、主题等）
├── _config.fluid.yml      # Fluid 主题配置（导航栏、首页布局、评论插件等）
├── _config.landscape.yml   # landscape 主题配置（未启用）
├── package.json            # 依赖管理
├── scaffolds/              # 文章/页面/草稿模板
│   ├── post.md
│   ├── page.md
│   └── draft.md
├── source/                 # 内容源文件
│   ├── _posts/             # 博客文章（Markdown）
│   └── img/                # 图片资源
├── themes/                 # 主题目录
│   └── fluid/              # Fluid 主题（通过 npm 安装）
├── public/                 # 构建输出（gitignore，不提交）
└── .github/
    └── dependabot.yml      # 每日 npm 依赖自动更新
```

## 架构说明

- **Hexo** 是一个静态站点生成器，将 Markdown 文件渲染为 HTML 页面
- **Fluid** 主题提供了完整的博客前端界面，配置项集中在 `_config.fluid.yml`
- 部署采用 `hexo-deployer-git` 插件，`hexo deploy` 会将 `public/` 目录推送到远程仓库
- 搜索功能基于 `hexo-generator-search` 插件（在 Fluid 主题内配置），生成 `local-search.xml` 索引文件
- 资源 CDN 通过 `_config.fluid.yml` 中的 `static_prefix` 统一配置，默认使用国内 BootCDN 镜像
- 评论系统支持多种后端（暂未启用），可在 `_config.fluid.yml` 中配置
- 主题支持暗色模式、打字机副标题动效、代码高亮（highlight.js）、图片懒加载等特性
