---
title: "构建、编译Cython代码"
date: 2019-12-02T11:06:58
categories:
  - Cython
tags:
  - Cython
  - 编译
---

主要有以下几种方法构建、编译Cython代码：
- 写一个 distutils/setuptools 的 setup.py 文件，这个是常用的和推荐的方法。
- 使用 Pyximport，
