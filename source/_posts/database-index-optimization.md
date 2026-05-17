---
title: 数据库索引原理与优化实践
date: 2026-05-06 16:00:00
categories: 后端技术
tags:
  - 数据库
  - MySQL
  - 性能优化
  - 索引
index_img: /img/default.png
---

## 索引的重要性

在数据库系统中，索引是提升查询性能最有效的手段之一。一个优秀的索引设计可以让查询速度提升几个数量级，而错误的索引设计则可能导致性能急剧下降。

<!-- more -->

## 索引的底层原理

### B+ 树结构

MySQL 的 InnoDB 引擎使用 B+ 树作为索引结构。B+ 树的特点：

- **所有数据存储在叶子节点**
- **非叶子节点只存储键值**，用于路由
- **叶子节点之间通过双向链表连接**，支持范围查询
- **树的高度通常为 2-4 层**，查询效率极高

```
                 [根节点]
                /    \
          [内部节点] [内部节点]
         /    |    \    |    \
      [叶子] [叶子] [叶子] [叶子] [叶子]
      (数据) (数据) (数据) (数据) (数据)
```

### 聚簇索引 vs 非聚簇索引

**聚簇索引（Clustered Index）：**
- 数据和索引存储在一起
- InnoDB 的主键索引就是聚簇索引
- 一张表只能有一个聚簇索引

**非聚簇索引（Secondary Index）：**
- 索引和数据分开存储
- 叶子节点存储的是主键值
- 查询时需要回表

## 索引优化策略

### 1. 选择合适的索引列

```sql
-- 经常出现在 WHERE 条件中的列
-- 经常用于 JOIN 的列
-- 经常需要排序的列

-- 好的索引
CREATE INDEX idx_user_email ON users(email);
CREATE INDEX idx_order_user_id ON orders(user_id);

-- 选择性低的列不适合建索引
-- 如性别、状态等只有少数几个值的列
```

### 2. 联合索引的最左前缀原则

```sql
CREATE INDEX idx_name_age ON users(name, age);

-- 能用到索引
SELECT * FROM users WHERE name = '张三';
SELECT * FROM users WHERE name = '张三' AND age = 25;

-- 用不到索引（跳过了最左列）
SELECT * FROM users WHERE age = 25;
```

### 3. 避免索引失效

```sql
-- 对索引列使用函数会导致索引失效
-- 避免
SELECT * FROM users WHERE YEAR(created_at) = 2026;

-- 改为
SELECT * FROM users WHERE created_at >= '2026-01-01'
  AND created_at < '2027-01-01';

-- 隐式类型转换也会导致索引失效
-- 如果 phone 是 varchar 类型
-- 避免
SELECT * FROM users WHERE phone = 13800000000;

-- 改为
SELECT * FROM users WHERE phone = '13800000000';
```

## 实战：慢查询优化

```sql
-- 原始慢查询（耗时 3.2s）
SELECT * FROM orders
WHERE status = 'completed'
  AND created_at > '2026-01-01'
ORDER BY amount DESC
LIMIT 20;

-- 添加复合索引
CREATE INDEX idx_status_amount_date
  ON orders(status, amount, created_at);

-- 优化后（耗时 0.03s）
```

## 查看索引使用情况

```sql
-- 查看查询是否使用了索引
EXPLAIN SELECT * FROM orders WHERE order_id = 10086;

-- 查看索引使用统计
SELECT * FROM sys.schema_index_statistics
WHERE table_schema = 'my_database';
```

## 总结

索引优化是数据库调优的核心技能。记住几点原则：

1. 不是索引越多越好，每个索引都有维护成本
2. 理解最左前缀原则，设计合理的联合索引
3. 定期使用 `EXPLAIN` 分析慢查询
4. 避免在大表上建立过多的索引
