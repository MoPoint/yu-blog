---
title: Git 工作流最佳实践
date: 2026-03-28 10:30:00
categories: 技术杂谈
tags:
  - Git
  - 版本控制
  - 团队协作
index_img: /img/default.png
---

## 为什么需要规范的 Git 工作流

Git 是现代软件开发不可或缺的版本控制工具。但仅会几个基本命令远远不够，一个规范的工作流能帮助团队避免冲突、保持历史清晰，让协作更加高效。

<!-- more -->

## 常用工作流对比

### Git Flow

适合有固定发布周期的项目：
- `master` — 生产分支
- `develop` — 开发主分支
- `feature/*` — 功能分支
- `release/*` — 发布分支
- `hotfix/*` — 紧急修复分支

### GitHub Flow

适合持续部署的简单流程：
- `main` — 主分支，始终可部署
- `feature` — 功能分支，提交 PR 合并

### Trunk-Based Development

适合 CI/CD 成熟的团队：
- 所有人在主干分支开发
- 短生命周期的特性分支
- 频繁合并，避免长期分支

## 提交规范：Conventional Commits

规范的提交信息能让 changelog 自动生成和维护历史更加轻松：

```
feat: 添加用户登录功能
fix: 修复页面崩溃问题
docs: 更新 API 文档
style: 格式化代码
refactor: 重构数据库查询
test: 添加单元测试
chore: 更新依赖版本
```

## 实用技巧

### .gitignore

合理配置 `.gitignore`，避免将敏感信息和不必要的文件提交到仓库：

```
# 依赖
node_modules/
vendor/

# 构建产物
dist/
build/
public/

# 环境配置
.env
.env.local

# 系统文件
.DS_Store
Thumbs.db
```

### 善用 git stash

当需要切换分支但又不想提交当前改动时：

```bash
# 暂存当前修改
git stash save "WIP: 正在调试的问题"

# 恢复最近一次暂存
git stash pop

# 查看暂存列表
git stash list
```

### 交互式 rebase

合并多个提交，保持历史整洁：

```bash
git rebase -i HEAD~3
```

然后使用 `pick`、`squash`、`reword` 等命令整理提交历史。

## 团队协作建议

1. **保持分支简短** — 一个分支只做一件事，尽量在一天内合并
2. **及时同步** — 频繁 `git pull --rebase` 避免冲突积累
3. **PR 要有描述** — 清楚的 PR 描述比代码注释更有价值
4. **Code Review** — 至少由一人 Review 后再合并
5. **不要强制推送** — 特别是在共享分支上，除非你清楚在做什么

## 总结

Git 工作流没有银弹，选择最适合团队的方式并在实践中不断优化才是关键。重要的是：**保持一致性**，无论选择哪种工作流，团队都应该共同遵守。
