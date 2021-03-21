---
title: 关于Normal Equation
date: 2019-10-23 14:37
tags:
- Machine Learning
---

已知数据集，x表示自变量，price(y)表示因变量

| x0   | Size（x1） | Number of bedrooms (x2) | Number of floors (x3) | Age of home (x4) | Price (y) |
| ---- | ---------- | ----------------------- | --------------------- | ---------------- | --------- |
| 1    | 2194       | 5                       | 1                     | 45               | 460       |
| 1    | 1416       | 3                       | 2                     | 40               | 232       |
| 1    | 1534       | 3                       | 2                     | 30               | 315       |
| 1    | 852        | 2                       | 1                     | 36               | 178       |

<!--more-->
由此，抽出X矩阵，Y矩阵

![矩阵抽象](https://github.com/georgezhou314/imageRepo/raw/master/ML/m1.png)

#### 现在需要求出系数矩阵 θ

**公式推导**

![公式推导](https://github.com/georgezhou314/imageRepo/raw/master/ML/m2.png)
**Matlab中描述**

X'的英文是：X primes

```matlab
pinv(X'*X)*X'*y
```

