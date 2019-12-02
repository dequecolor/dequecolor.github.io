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
- 使用 Pyximport，就像导入 .py 文件一样导入 .pyx 文件（会在背后使用 distutils 自动编译和构建 Cython 代码），这个比建立 setup.py 文件更加简单，但是只能用于简单编译，如果要指定特定的编译选项，请使用 setup.py 文件。
- 手动运行 cython 命令行实用程序从 .pyx 文件产生 .c 文件，然后手动编译 .c 文件变成适用于 python 导入的共享对象库或者Dll。（这些人工步骤主要用于调试和实验）
- 使用 jupyter notebook 或者 sage notebook。这是最简单的方法去学习 Cython、运行 Cython 代码
