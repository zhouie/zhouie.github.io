---
title: 【分享】 Markdown-Here md-2-html渲染
categories:
  - 分享
  - 博客
tags:
  - Markdown
  - Tool-share
abbrlink: 201808031
date: 2018-08-03 12:36:45
---

> 写在前面

最近在编排微信公众号的时候，竟然发现这里没有支持 **代码块** 这种语块，更别谈 **高亮处理** 了，而且啊，一直也想不明白，微信公众号的编辑器怎么还没有支持 Markdown 这个目前最高效的写作语言模块。

试想一下，如果你正在通过 Markdown 来写文章，需要将此文章发布到公众号，那么，你可能就得这样：

* 第一步，删除 Markdown 特定的语法标记
* 第二步，将文字内容复制到公众号编辑框中再进行排版

这种方法，仅仅偶尔一次两次也没有问题，要是使用频繁的话，就太浪费时间和精力了。

当然了，也有一些手段可以将 Markdown 转换成 HTML ，再来将其中的内容复制到公众号编辑框中进行排版调整。
[Convert Markdown to HTML](https://markdowntohtml.com/)

综上，这里介绍这样一款浏览器插件 —— [Markdown Here](https://markdown-here.com/) ，通过其提供的一键渲染功能，可将 Markdown 格式直接达到（渲染）我们需要的样式。


### 安装

下载地址：[http://sina.lt/fCA8](http://sina.lt/fCA8)(也提供有 Firefox、Safari 插件)，需要翻墙。
备份地址：[Markdown Here for Chrome.crx](https://github.com/zhouie/Google-Chrome-Extensions/blob/master/Markdown%20Here%20for%20Chrome.crx)

### 渲染 CSS 样式

![](http://p7n85i5tr.bkt.clouddn.com/zhouie/img/Markdown-Here/1.png)

打开 Markdown Here 选项，将 **自定义的基本渲染CSS样式**直接覆盖到 Primary Styling CSS（**基本渲染CSS**）框中。

其中，至于自定义的基本渲染CSS样式，有着一定CSS知识的你完全可以根据自己的喜好来设置，当然，我这里也提供了一些[Gist Snippets](https://gist.github.com/zhouie) 以供学习参阅。

至于右边的 **语法高亮CSS**，提供有很多可选主题，就看你喜欢哪一款了。下面的 **预览框**中会自动保存修改并同步显示渲染效果预览。


### 多平台使用

打开微信公众号后台编辑器，直接复制带有 Markdown 语法的文章或者笔记到编辑器中（不再需要删除 Markdown 特定的语法标记），点击浏览器右上角 Markdown Here图标(也可以试试快捷键`CTRL+ALT+M`)，Markdown Here 会自动将你的文章进行了全新的渲染排版（转成HTML样式），文章标题层级、加粗、引用、表格、图片、代码块、甚至最不好弄的数学公式，一目了然。

得说一下，这里只是拿微信公众号为例来讲，其实 Markdown Here 这款插件能让 Markdown 内容，无需作任何调整就能在 微信公众号、CSDN、博客园、掘金、知乎、51CTO、WordPress 等多种平台正确显示渲染后的效果。

同时啊，Markdown Here 还可以用来渲染用 Markdown 写的邮件内容，似乎这才是它的推广方向...

![](http://p7n85i5tr.bkt.clouddn.com/zhouie/img/Markdown-Here/2.png)


### 推荐阅读

* [Md2All | Markdown排版利器](http://aclickall.com/)
* [程序猿DD | Markdown转换工具](http://blog.didispace.com/tools/online-markdown/)
* [如何排版微信公众平台的文章？ - 知乎](https://www.zhihu.com/question/23640203)
* [在线Markdown转微信公众号内容开源项目](http://md.barretlee.com/)
* [走进Markdown园子](https://zhouie.cn/posts/201804111/)