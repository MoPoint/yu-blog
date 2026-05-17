---
title: 理解 RESTful API 设计原则
date: 2026-03-18 15:00:00
categories: 后端技术
tags:
  - API
  - REST
  - 架构
  - 后端
index_img: /img/default.png
---

## 什么是 RESTful API

REST（Representational State Transfer）是一种软件架构风格，RESTful API 则是基于这种风格设计的 Web API。它不是一种协议，而是一组设计原则和约束条件。

<!-- more -->

## 核心原则

### 1. 资源导向的 URL

RESTful API 围绕资源（Resources）设计，每个资源通过唯一的 URL 标识：

```
GET    /users          # 获取用户列表
GET    /users/123      # 获取单个用户
POST   /users          # 创建用户
PUT    /users/123      # 更新用户
DELETE /users/123      # 删除用户
```

### 2. HTTP 方法语义化

不同的 HTTP 方法对应不同的操作：

| 方法 | 操作 | 幂等 |
|------|------|------|
| GET | 获取资源 | 是 |
| POST | 创建资源 | 否 |
| PUT | 全量更新 | 是 |
| PATCH | 部分更新 | 是 |
| DELETE | 删除资源 | 是 |

### 3. 使用正确的状态码

```http
200 OK          — 请求成功
201 Created     — 资源创建成功
204 No Content  — 删除成功
400 Bad Request — 请求参数错误
401 Unauthorized — 未认证
403 Forbidden   — 无权限
404 Not Found   — 资源不存在
500 Server Error — 服务器错误
```

## 实践示例

### 好的设计

```
GET    /api/v1/articles?page=1&size=20
GET    /api/v1/articles/42
POST   /api/v1/articles
PUT    /api/v1/articles/42
DELETE /api/v1/articles/42

GET    /api/v1/articles/42/comments
POST   /api/v1/articles/42/comments
```

### 避免的设计

```
GET    /api/getArticles       # 动词代替了名词
POST   /api/updateArticle     # 方法名代替了 HTTP 方法
GET    /api/articleDetails/42 # 过于冗长
POST   /api/delete_article    # 应使用 DELETE 方法
```

## 版本管理

API 版本管理有多种策略：

**URL 路径中带版本号（推荐）：**
```
/api/v1/users
/api/v2/users
```

**请求头指定版本：**
```
Accept: application/vnd.api+json;version=1
```

## 分页与过滤

```http
GET /api/v1/articles?page=2&size=20
GET /api/v1/articles?sort=-created_at
GET /api/v1/articles?category=tech&status=published
```

响应中应包含分页信息：

```json
{
  "data": [...],
  "meta": {
    "page": 2,
    "size": 20,
    "total": 156,
    "total_pages": 8
  }
}
```

## 结语

RESTful API 设计看似简单，但要做好需要深入理解其设计哲学。好的 API 设计能让前后端协作更高效，也维护起来更轻松。
