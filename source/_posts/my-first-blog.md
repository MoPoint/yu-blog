---
title: 我的第一篇博客：从零搭建个人网站
date: 2026-01-15 10:00:00
categories: 技术杂谈
tags:
  - Hexo
  - GitHub Pages
  - 博客
index_img: /img/default.png
---

## 为什么搭建个人博客

在互联网时代，拥有一个属于自己的博客是一件很有意义的事情。它不仅是记录技术成长的笔记，更是一个沉淀思考和分享知识的空间。

<!-- more -->

经过一番调研，我选择了 **Hexo + GitHub Pages** 的组合。原因很简单：

1. **免费** — GitHub Pages 提供免费的静态网站托管
2. **轻量** — Hexo 基于 Node.js，使用 Markdown 写作，专注内容
3. **灵活** — 丰富的主题和插件生态，可定制性强

## 搭建过程

### 环境准备

安装 Hexo 需要 Node.js 环境。确认安装好后，全局安装 Hexo：

```bash
npm install -g hexo-cli
hexo init my-blog
cd my-blog
npm install
```

### 选择主题

我选择了 **Fluid** 主题，一款简洁优雅的 Material Design 风格主题：

```bash
npm install hexo-theme-fluid
```

在 `_config.yml` 中设置 `theme: fluid`，并将主题配置复制到 `_config.fluid.yml`。

### 部署到 GitHub Pages

配置 `_config.yml` 的部署部分：

```yaml
deploy:
  type: git
  repo: https://github.com/username/username.github.io.git
  branch: gh-pages
```

然后执行：

```bash
hexo clean && hexo generate && hexo deploy
```

## 小结

至此，个人博客就搭建完成了。整个过程并不复杂，但只要坚持写作，这个小小的站点就会慢慢成长为有价值的知识库。

接下来我还会分享更多技术文章，敬请期待！
