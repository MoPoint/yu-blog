---
title: 我的开发环境配置分享
date: 2026-03-05 11:00:00
categories: 技术杂谈
tags:
  - 开发工具
  - VSCode
  - 效率
  - 终端
index_img: /img/default.png
---

## 前言

一个好的开发环境能显著提升编码效率和舒适度。经过多年的摸索和调整，我逐渐形成了一套适合自己的开发环境配置。今天就分享出来，希望能给大家一些参考。

<!-- more -->

## 终端：iTerm2 + Oh My Zsh

终端是程序员最常用的工具之一，我选择了 iTerm2 + Oh My Zsh 的组合。

### 主题

使用 Powerlevel10k 主题，配置简单且颜值在线：

```bash
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
```

然后在 `~/.zshrc` 中设置 `ZSH_THEME="powerlevel10k/powerlevel10k"`。

### 常用插件

```bash
plugins=(
  git
  zsh-autosuggestions
  zsh-syntax-highlighting
  web-search
  autojump
)
```

## 编辑器：VSCode

VSCode 是我主要使用的编辑器。以下是我必装的插件：

### 必备插件

- **GitLens** — Git 历史查看神器
- **Prettier** — 代码格式化
- **ESLint** — JavaScript 代码检查
- **Material Icon Theme** — 文件图标主题
- **Error Lens** — 行内错误提示
- **GitHub Copilot** — AI 编程助手

### 设置片段

```json
{
  "editor.fontSize": 14,
  "editor.fontFamily": "JetBrains Mono, Menlo, Monaco",
  "editor.minimap.enabled": false,
  "editor.formatOnSave": true,
  "workbench.colorTheme": "One Dark Pro",
  "terminal.integrated.fontSize": 13
}
```

## 包管理器

- **Homebrew** — macOS 的包管理器
- **nvm** — Node.js 版本管理
- **pyenv** — Python 版本管理

## Git 配置

```bash
# 别名
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.st status
git config --global alias.lg "log --oneline --graph --all"
```

## 效率工具

- **Alfred** — 快速启动和搜索
- **Raycast** — 现代化的 Alfred 替代品
- **Rectangle** — 窗口管理
- **BetterTouchTool** — 触控板手势增强
- **Snipaste** — 截图工具

## 总结

开发环境配置是一个持续优化的过程。没有最好的配置，只有最适合自己的配置。希望我的分享能给你一些灵感，如果有什么好用的工具推荐，欢迎留言交流！
