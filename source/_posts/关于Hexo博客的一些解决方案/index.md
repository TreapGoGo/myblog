---
title: 关于Hexo博客的一些解决方案
date: 2023-06-28 21:19:47
tags:
---

1. 若`generate`或`deploy`建站时遇到bug，尝试运行`hexo clean`清空缓存。
2. 使用主题时，若主题是子模块，需要在博客根目录下新建`.gitmodules`并添加相关信息指向子模块。
3. 若`hexo server`后访问[http://localhost:4000/](http://localhost:4000/)，网页崩溃且只输出 `Cannot GET/`，应当尝试运行`npm install`确保包都是正常状态。
4. 公式包使用[hexo-renderer-markdown-it-plus](https://github.com/CHENXCHEN/hexo-renderer-markdown-it-plus.git)，同时必须将`MathJax Support`中`true`改成`false`，否则会冲突。若出现渲染出一个正确公式和一个错误公式，说明缺少`hexo-math`，应当运行`npm install hexo-math --save`。
5. 有时候会遇见在netlify上建站的时候， `hexo-img-locator` 模块找不到图片的问题，这时候应该注意做以下两个检查：
   1. 检查图片所在的文件是不是被放到 `.gitignore` 里面了，如果是的话，这些图片根本不会被推送到远程仓库里面去，自然就要不存在路径
   2. 检查图片的相对路径使用的分隔符是正斜杠 `/` 还是反斜杠 `\` ，在本地 Windows 系统可能差别不大， `hexo-img-locator` 可以正常替换，但是在 Netlify 的类 Unix 系统中只能识别正斜杠 `/` 为文件路径的分隔符。