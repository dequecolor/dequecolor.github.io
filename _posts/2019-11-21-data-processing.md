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
```python
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
```
>>> X_scaled.mean(axis=0)
array([0., 0., 0.])

>>> X_scaled.std(axis=0)
array([1., 1., 1.])
```
preprocessing模块深入地提供了了一个类StandardScaler来进行转化，可以在训练集上计算均值和方差，在测试集上做出同样的转换（认为测试集和训练集数据特征一样）：
```python
>>> scaler = preprocessing.StandardScaler().fit(X_train)
>>> scaler
StandardScaler(copy=True, with_mean=True, with_std=True)

>>> scaler.mean_                                      
array([1. ..., 0. ..., 0.33...])

>>> scaler.scale_                                       
array([0.81..., 0.81..., 1.24...])

>>> scaler.transform(X_train)                           
array([[ 0.  ..., -1.22...,  1.33...],
       [ 1.22...,  0.  ..., -0.26...],
       [-1.22...,  1.22..., -1.06...]])
```
实例scaler可以应用于新数据做出转换：
```python
>>> X_test = [[-1., 1., 0.]]
>>> scaler.transform(X_test)                
array([[-2.44...,  1.22..., -0.26...]])
```
通过将with_mean = False或with_std = False传递给StandardScaler的构造函数，可以禁用居中或缩放。
