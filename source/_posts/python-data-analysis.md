---
title: 使用 Python 进行数据分析的基础方法
date: 2026-02-20 09:00:00
categories: 技术杂谈
tags:
  - Python
  - 数据分析
  - Pandas
index_img: /img/default.png
---

## 为什么用 Python 做数据分析

Python 凭借其简洁的语法和强大的数据科学生态系统，已经成为数据分析领域的首选语言。无论是数据清洗、统计分析还是可视化，Python 都有成熟的库来支持。

<!-- more -->

## 核心库介绍

### Pandas — 数据处理的核心

Pandas 提供了 `DataFrame` 和 `Series` 两种核心数据结构，让数据操作变得异常简单：

```python
import pandas as pd

# 读取 CSV 文件
df = pd.read_csv('sales_data.csv')

# 查看前几行
print(df.head())

# 基本统计信息
print(df.describe())

# 按条件筛选
high_sales = df[df['amount'] > 1000]
```

### 数据清洗

现实中的数据往往是不干净的，Pandas 提供了丰富的清洗工具：

```python
# 处理缺失值
df.dropna(inplace=True)  # 删除缺失值
df.fillna(0, inplace=True)  # 填充缺失值

# 去除重复数据
df.drop_duplicates(inplace=True)

# 类型转换
df['date'] = pd.to_datetime(df['date'])
df['amount'] = df['amount'].astype(float)
```

### Matplotlib — 数据可视化

```python
import matplotlib.pyplot as plt

# 绘制折线图
df.groupby('date')['amount'].sum().plot(kind='line')
plt.title('每日销售额趋势')
plt.xlabel('日期')
plt.ylabel('销售额')
plt.show()
```

### NumPy — 数值计算

```python
import numpy as np

# 创建数组
arr = np.array([1, 2, 3, 4, 5])

# 矩阵运算
matrix = np.random.randn(3, 3)
print(matrix.mean())  # 计算均值
print(matrix.std())   # 计算标准差
```

## 一个完整的数据分析流程

假设我们要分析某电商平台的销售数据：

```python
import pandas as pd
import matplotlib.pyplot as plt

# 1. 加载数据
df = pd.read_csv('sales.csv', parse_dates=['order_date'])

# 2. 数据清洗
df = df.dropna(subset=['amount'])
df = df[df['amount'] > 0]  # 剔除异常值

# 3. 特征工程
df['month'] = df['order_date'].dt.month
df['quarter'] = df['order_date'].dt.quarter

# 4. 分析：每月销售额
monthly_sales = df.groupby('month')['amount'].sum()

# 5. 可视化
plt.figure(figsize=(10, 6))
monthly_sales.plot(kind='bar', color='skyblue')
plt.title('月度销售额统计')
plt.xlabel('月份')
plt.ylabel('销售额')
plt.xticks(rotation=45)
plt.tight_layout()
plt.savefig('monthly_sales.png')
```

## 结语

Python 数据分析的门槛不高，但要想深入掌握，需要不断实践。建议从自己感兴趣的数据集入手，边做边学，这是最快的学习方式。
