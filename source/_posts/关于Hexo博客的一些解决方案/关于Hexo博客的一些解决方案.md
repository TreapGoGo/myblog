---
title: 关于Hexo博客的一些解决方案
date: 2023-06-28 21:19:47
tags:
---

1. 若`generate`或`deploy`建站时遇到bug，尝试运行`hexo clean`清空缓存。
2. 使用主题时，若主题是子模块，需要在博客根目录下新建`.gitmodules`并添加相关信息指向子模块。
3. 若`hexo server`后访问[http://localhost:4000/](http://localhost:4000/)，网页崩溃且只输出 `Cannot GET/`，应当尝试运行`npm install`确保包都是正常状态。
4. 公式包使用[hexo-renderer-markdown-it-plus](https://github.com/CHENXCHEN/hexo-renderer-markdown-it-plus.git)，同时必须将`MathJax Support`中`true`改成`false`，否则会冲突。若出现渲染出一个正确公式和一个错误公式，说明缺少`hexo-math`，应当运行`npm install hexo-math --save`。
5. 