---
title: JavaScript 异步编程入门指南
date: 2026-02-08 14:30:00
categories: 前端技术
tags:
  - JavaScript
  - 异步编程
  - Promise
index_img: /img/default.png
---

## 为什么需要异步编程

JavaScript 是单线程语言，这意味着它一次只能执行一个任务。如果所有操作都是同步的，那么像网络请求、文件读取这样的耗时操作就会阻塞主线程，导致页面卡顿。

<!-- more -->

异步编程就是为了解决这个问题而生的。

## 回调函数（Callback）

回调函数是最早的异步处理方式：

```javascript
function fetchData(callback) {
  setTimeout(() => {
    callback('数据加载完成');
  }, 1000);
}

fetchData((data) => {
  console.log(data);
});
```

但回调方式有一个严重的问题：**回调地狱（Callback Hell）**。当多个异步操作嵌套时，代码会变得难以维护。

## Promise 登场

Promise 是 ES6 引入的异步解决方案，它让异步代码更加优雅：

```javascript
const fetchData = () => {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve('数据加载完成');
    }, 1000);
  });
};

fetchData()
  .then(data => console.log(data))
  .catch(error => console.error(error));
```

### 链式调用

Promise 最大的优势是可以链式调用：

```javascript
getUser()
  .then(user => getPosts(user.id))
  .then(posts => getComments(posts[0].id))
  .then(comments => console.log(comments))
  .catch(err => console.error(err));
```

## Async/Await

ES2017 引入的 async/await 让异步代码看起来像同步代码一样直观：

```javascript
async function loadData() {
  try {
    const user = await getUser();
    const posts = await getPosts(user.id);
    const comments = await getComments(posts[0].id);
    console.log(comments);
  } catch (error) {
    console.error('加载失败:', error);
  }
}
```

## 实际应用：并发请求

当需要同时发起多个请求时，可以使用 `Promise.all`：

```javascript
async function loadDashboard() {
  const [users, posts, stats] = await Promise.all([
    fetch('/api/users').then(r => r.json()),
    fetch('/api/posts').then(r => r.json()),
    fetch('/api/stats').then(r => r.json()),
  ]);

  return { users, posts, stats };
}
```

## 总结

从回调函数到 Promise，再到 async/await，JavaScript 的异步编程方式越来越人性化。掌握这些基础，是成为一名合格前端开发者的必修课。
