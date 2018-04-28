---
title: 基于Travis CI实现Hexo在Github和Coding的同步自动化部署
categories:
  - 博客
tags:
  - 日志
  - NodeJS
  - Git
  - CI持续集成
  - Travis CI
abbrlink: 732638714
date: 2018-04-27 11:09:41
---

## 完成Hexo主题安装和配置

如果您还没有安装[Hexo](https://hexo.io/zh-cn/)环境，请参考[Hexo文档](https://hexo.io/zh-cn/docs/index.html)安装，也给出这样一篇[百度经验](https://jingyan.baidu.com/article/d8072ac493ce16ec95cefd2a.html)仅供参考，这个网上博文一堆，而且操作也很简单。

因为我采用的是[Indigo 主题](https://github.com/yscoder/hexo-theme-indigo)，而且这个主题还很详细地给出了配置安装本主题的详细[教程步骤](https://github.com/yscoder/hexo-theme-indigo/wiki)(3.0 以上`Hexo`版本)

[Indigo 主题](https://github.com/yscoder/hexo-theme-indigo)，一个`Material Design`风格的`Hexo`主题..预览：<https://yscoder.github.io/>..

[NexT 主题](https://github.com/iissnan/hexo-theme-next/blob/master/README.cn.md)，一个高质量并且优雅的`Hexo`主题，[NexT使用文档](http://theme-next.iissnan.com/)..

当然了，`Hexo`主题很多，更多可访问[这里](https://hexo.io/themes/)


## 基于Travis CI实现同步部署

> 参考链接

在这里要特别感谢一下这位CSDN认证`博客专家`[PayneQin](https://blog.csdn.net/qinyuanpei)以下两篇博文对总个过程的帮助！

* [持续集成在Hexo自动化部署上的实践](https://blog.csdn.net/qinyuanpei/article/details/78381008)
* [基于Travis CI实现Hexo在Github和Coding的同步部署](https://blog.csdn.net/qinyuanpei/article/details/79388983)

当然还有这篇[手把手教你使用Travis CI自动部署你的Hexo博客到Github上](https://blog.csdn.net/woblog/article/details/51319364)..


> 补充链接 | Travis CI自动部署Hexo系列

* [Travis自动部署Hexo一](https://jingyan.baidu.com/article/359911f5a3744657fe030683.html)
* [Travis自动部署Hexo二](https://jingyan.baidu.com/article/27fa7326ae44c046f8271f83.html)
* [Travis自动部署Hexo三](https://jingyan.baidu.com/article/db55b609d94c414ba30a2f0e.html)
* [Travis自动部署Hexo四](https://jingyan.baidu.com/article/215817f7b606e81eda142388.html)
* [Travis自动部署Hexo五](https://jingyan.baidu.com/article/b87fe19e57eb54521935684d.html)
* [Travis自动部署Hexo六](https://jingyan.baidu.com/article/bea41d43a6cd12b4c41be64e.html)
