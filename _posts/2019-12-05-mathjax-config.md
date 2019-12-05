---
title: "jekyll使用mathjax编辑数学公式"
date: 2019-12-05T11:16:26
categories:
  - 博客配置
tags:
  - mathjax
  - 数学公式支持
---

[参考链接][1]

### 实际操作

将`_pages`文件夹里的`default.html`的头部标签内添加一下js脚本：
```html
<script type="text/x-mathjax-config">
  MathJax.Hub.Config({tex2jax: {inlineMath: [['$','$'], ['\\(','\\)']]}});
</script>
```






[1]: http://www.atjiang.com/markdown-present-math-formula-with-mathjax/
