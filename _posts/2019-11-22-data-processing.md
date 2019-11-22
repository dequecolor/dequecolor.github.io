---
title: "Sklearn数据处理"
date: 2019-11-22T16:26:34
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
```python
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
## 缩放特征到某个范围
另一种标准化方法是将特征缩放到给定的最小值和最大值之间，通常介于零和一之间，或者将每个特征的最大绝对值缩放到单位大小。这可以分别使用MinMaxScaler或MaxAbsScaler来实现。

这是将示例数据矩阵缩放到[0，1]范围的例子：
```python
>>> X_train = np.array([[ 1., -1.,  2.],
...                     [ 2.,  0.,  0.],
...                     [ 0.,  1., -1.]])
...
>>> min_max_scaler = preprocessing.MinMaxScaler()
>>> X_train_minmax = min_max_scaler.fit_transform(X_train)
>>> X_train_minmax
array([[0.5       , 0.        , 1.        ],
       [1.        , 0.5       , 0.33333333],
       [0.        , 1.        , 0.        ]])
```
然后，可以将转换器应用于在拟合期间未看到的一些新测试数据，将应用相同的缩放和移位操作，以与对训练数据执行的转换一致：
```python
>>> X_test = np.array([[-3., -1.,  4.]])
>>> X_test_minmax = min_max_scaler.transform(X_test)
>>> X_test_minmax
array([[-1.5       ,  0.        ,  1.66666667]])
```
可以对缩放器属性进行内部检查，以找到在训练数据上学习到的转换的确切性质：
```python
>>> min_max_scaler.scale_                             
array([0.5       , 0.5       , 0.33...])

>>> min_max_scaler.min_                               
array([0.        , 0.5       , 0.33...])
```
如果给MinMaxScaler一个明确的feature_range =（min，max），则完整公式为：
```python
X_std = (X - X.min(axis=0)) / (X.max(axis=0) - X.min(axis=0))

X_scaled = X_std * (max - min) + min
```
MaxAbsScaler的工作方式非常相似，但是通过除以每个特征中的最大值来将训练数据置于[-1，1]范围内。它适用于已经以零或稀疏数据为中心的数据。

这是在此缩放器上使用上一示例中的示例数据的方法：
```python
>>> X_train = np.array([[ 1., -1.,  2.],
...                     [ 2.,  0.,  0.],
...                     [ 0.,  1., -1.]])
...
>>> max_abs_scaler = preprocessing.MaxAbsScaler()
>>> X_train_maxabs = max_abs_scaler.fit_transform(X_train)
>>> X_train_maxabs                # doctest +NORMALIZE_WHITESPACE^
array([[ 0.5, -1. ,  1. ],
       [ 1. ,  0. ,  0. ],
       [ 0. ,  1. , -0.5]])
>>> X_test = np.array([[ -3., -1.,  4.]])
>>> X_test_maxabs = max_abs_scaler.transform(X_test)
>>> X_test_maxabs                 
array([[-1.5, -1. ,  2. ]])
>>> max_abs_scaler.scale_         
array([2.,  1.,  2.])
```
与scale一样，如果您不想创建对象，该模块还提供了便捷功能minmax_scale和maxabs_scale。
