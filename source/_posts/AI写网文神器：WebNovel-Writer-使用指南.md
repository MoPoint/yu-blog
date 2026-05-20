---
title: AI写网文神器：WebNovel Writer 使用指南
date: 2026-05-20 19:00:00
tags:
  - AI
  - 写作
  - 工具
  - Claude
categories:
  - 技术分享
---

## 前言

作为一名技术博主兼业余网文爱好者，我一直想找个趁手的 AI 辅助写作工具。市面上有不少 AI 写作产品，但大多存在两个痛点：

- **遗忘**：写到后面忘了前面的设定，角色名字都能搞错
- **幻觉**：AI 胡编乱造，与已有剧情矛盾

最近在 GitHub 挖到了一个宝藏开源项目 —— **[WebNovel Writer](https://github.com/lingfengQAQ/webnovel-writer)**，一个基于 Claude Code 构建的 AI 辅助网文写作系统，Star 数已达 4.2k。花了一周时间深度体验后，我来给大家分享一下它的使用心得。

<!-- more -->

## 项目简介

WebNovel Writer 是一个开源（GPL v3）的 AI 辅助小说创作系统，运行在 **Claude Code** 之上，专为网文写作者打造。它的核心卖点是能写出**长达 200 万字**的长篇网文，同时有效对抗 AI 写作的两大顽疾——遗忘和幻觉。

项目由开发者 **lingfengQAQ** 维护，主要使用 Python（92.4%）编写，配合 JavaScript 前端界面。

## 核心特性

### 1. 故事系统（Story System）

这是项目的灵魂所在。它采用了一种"主链"架构，将真实数据与展示数据分离：

- **`.story-system/`**——存放主设定、卷集、章节、评论等源数据
- **`.webnovel/`**——存放投影后的状态、索引、摘要等只读数据

每当你完成一个章节，系统会自动触发投影更新，确保所有衍生数据保持同步。

### 2. RAG 检索增强生成

RAG 是解决遗忘问题的关键。每次写作前，系统会自动检索相关的角色档案、剧情线索和世界观规则，注入到当前会话的上下文中。这需要配置两个服务：

- **Embedding API**（推荐 Qwen3-Embedding-8B，部署在 ModelScope）
- **Re-ranker API**（推荐 Jina）

### 3. 37 种网文题材模板

内置 37 种网文题材的角色模板，通过 `references/genre-profiles.md` 配置。无论你想写玄幻、都市、科幻还是历史，都有现成的模板可以直接套用。

### 4. Hook / 追读力系统

这是 v5.3 引入的有趣功能。系统会追踪读者的追读体验，包括：

- 钩子（Hook）设计
- 爽点（Cool-point）分布
- 微观回报节奏
- 债务追踪

帮助你维持叙事的张力，避免剧情平淡如水。

## 安装与配置

### 第一步：安装 Claude Code 插件

```bash
claude plugin marketplace add lingfengQAQ/webnovel-writer --scope user
claude plugin install webnovel-writer@webnovel-writer-marketplace --scope user
```

### 第二步：安装 Python 依赖

```bash
python -m pip install -r https://raw.githubusercontent.com/lingfengQAQ/webnovel-writer/HEAD/requirements.txt
```

### 第三步：初始化小说项目

进入 Claude Code 后，运行：

```
/webnovel-init
```

系统会引导你填写书名、题材、主角信息，然后自动创建项目目录。

### 第四步：配置 RAG

```bash
cp .env.example .env
```

编辑 `.env` 文件，填入 Embedding 和 Re-ranker 的 API 地址和密钥。

## 使用体验

初始化完成后，就可以开始写作了。常用的命令有：

| 命令 | 说明 |
|------|------|
| `/webnovel-plan 1` | 规划第 1 卷的大纲 |
| `/webnovel-write 1` | 写第 1 章 |
| `/webnovel-review 1-5` | 审阅第 1~5 章 |
| `/webnovel-preflight` | 系统健康检查 |
| `/webnovel-dashboard` | 打开写作仪表盘 |

实际的写作体验相当流畅。AI 会记住你之前的设定——某个配角在第 10 章提过的细节，到了第 50 章还能准确引用，不再需要手动翻前面的章节去核对。

仪表盘界面也做得不错，可以直观地看到项目状态、实体关系图、章节内容和追读力指标，而且是预编译好的，无需额外构建。

## 技术架构

从技术角度看，这个项目的设计也很值得学习：

- **提交驱动的流水线**：接受的章节会触发投影更新，而不是直接修改源数据链
- **内存暂存板**（v5.5.5）：写作前注入相关上下文，写作后提炼并持久化，形成一个长期记忆循环
- **Bye-Bye 遗忘**：通过 RAG + 记忆循环的组合拳，基本解决了长文本下的遗忘问题

## 总结

WebNovel Writer 是我目前见过最专业的 AI 辅助网文写作工具，它的设计理念——用工程思维解决创作问题——让我印象深刻。

当然，它也有一定的上手门槛：需要配置 Claude Code 环境、RAG 服务等。如果你是纯粹的写作者，可能更倾向于零配置的在线产品；但如果你是技术背景的创作者，或者对 AI 辅助写作的技术实现感兴趣，那这个项目绝对值得一试。

**相关链接：**

- [GitHub 仓库](https://github.com/lingfengQAQ/webnovel-writer)
- [GPL v3 许可证](https://www.gnu.org/licenses/gpl-3.0.html)
- [Claude Code](https://claude.ai/code)
