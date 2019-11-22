---
title: "Sklearn数据处理"
date: 2019-11-21T23:23:11
categories:
  - 数据处理
tags:
  - Python
  - Sklearn
toc: true
---

Python中的Sklearn模块内置了经典的机器学习算法，以及数据处理方法。本文主要介绍Sklearn的数据处理部分的功能和用法。

# 为什么需要数据处理
在机器学习里面，绝大部分模型对数据的是否标准化有很大的差异，一般来说，机器学习模型都会受益于数据的标准化。
# Python Sklearn数据处理
## 包
sklearn.preprocessing
## 标准化方法和类
scale函数提供了一个快速和易于使用的方式去转换数据为均值为0，标准差为1的标准数据
```Python
>>> from sklearn import preprocessing
>>> import numpy as np
>>> X_train = np.array([[ 1., -1.,  2.],
...                     [ 2.,  0.,  0.],
...                     [ 0.,  1., -1.]])
>>> X_scaled = preprocessing.scale(X_train)

>>> X_scaled                                          
array([[ 0.  ..., -1.22...,  1.33...],
       [ 1.22...,  0.  ..., -0.26...],
       [-1.22...,  1.22..., -1.06...]])
```
转换后的数据具有0均值，和单位方差
```Python
>>> X_scaled.mean(axis=0)
array([0., 0., 0.])

>>> X_scaled.std(axis=0)
array([1., 1., 1.])
```

