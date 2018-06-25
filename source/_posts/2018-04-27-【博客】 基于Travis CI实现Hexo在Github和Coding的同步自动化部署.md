---
title: 【博客】 基于Travis CI实现Hexo在Github和Coding的同步自动化部署
categories:
  - 博客
tags:
  - Hexo
  - NodeJS
  - Git
  - Travis CI
abbrlink: 201804261
date: 2018-04-26 11:09:41
---

## 完成Hexo主题安装和配置

如果您还没有安装[Hexo](https://hexo.io/zh-cn/)环境，请参考[Hexo文档](https://hexo.io/zh-cn/docs/index.html)安装，也给出这样两篇博文————[百度经验](https://jingyan.baidu.com/article/d8072ac493ce16ec95cefd2a.html)和[2018最新版hexo+Github搭建个人博客教程](https://blog.csdn.net/qq_32454537/article/details/79482908)仅供参考，这个去网上一搜到处都有，而且操作也很简单，就不赘述了。

我采用的是[Indigo 主题](https://github.com/yscoder/hexo-theme-indigo)，而且这个主题还很详细地给出了配置安装本主题的详细[教程步骤](https://github.com/yscoder/hexo-theme-indigo/wiki)(3.0 以上`Hexo`版本)

[Indigo 主题](https://github.com/yscoder/hexo-theme-indigo)，一个`Material Design`风格的`Hexo`主题..预览：<https://yscoder.github.io/>..

[NexT 主题](https://github.com/iissnan/hexo-theme-next/blob/master/README.cn.md)，一个高质量并且优雅的`Hexo`主题，[NexT使用文档](http://theme-next.iissnan.com/)..

当然了，`Hexo`主题很多，更多可访问[官网](https://hexo.io/themes/)以及[Hexo's GitHub WiKi](https://github.com/hexojs/hexo/wiki)


## 基于Travis CI实现同步部署

### 参考内容

在这里要特别感谢一下这位CSDN认证`博客专家`[PayneQin](https://blog.csdn.net/qinyuanpei)以下两篇博文对总个过程的帮助！

* [持续集成在Hexo自动化部署上的实践](https://blog.csdn.net/qinyuanpei/article/details/78381008)
* [基于Travis CI实现Hexo在Github和Coding的同步部署](https://blog.csdn.net/qinyuanpei/article/details/79388983)

当然还有这篇[手把手教你使用Travis CI自动部署你的Hexo博客到Github上](https://blog.csdn.net/woblog/article/details/51319364)..

以及Travis CI自动部署Hexo系列文章

* [Travis自动部署Hexo一](https://jingyan.baidu.com/article/359911f5a3744657fe030683.html)
* [Travis自动部署Hexo二](https://jingyan.baidu.com/article/27fa7326ae44c046f8271f83.html)
* [Travis自动部署Hexo三](https://jingyan.baidu.com/article/db55b609d94c414ba30a2f0e.html)
* [Travis自动部署Hexo四](https://jingyan.baidu.com/article/215817f7b606e81eda142388.html)
* [Travis自动部署Hexo五](https://jingyan.baidu.com/article/b87fe19e57eb54521935684d.html)
* [Travis自动部署Hexo六](https://jingyan.baidu.com/article/bea41d43a6cd12b4c41be64e.html)


### 相关链接

* [Hexo的创建者@tommy351 | Twitter](https://twitter.com/tommy351)

* [hexo专区文章](https://blog.csdn.net/ganzhilin520/article/category/7398650)

* [Hexo主题集锦](https://github.com/hexojs/hexo/wiki)

* [利用CI自动部署hexo博客](http://wenjunjiang.win/2018/02/11/%E5%88%A9%E7%94%A8CI%E8%87%AA%E5%8A%A8%E9%83%A8%E7%BD%B2hexo%E5%8D%9A%E5%AE%A2/)

* [Travis CI - 项目持续集成好伴侣](https://mp.weixin.qq.com/s/finrODi4BfYPACSzZ0tUuA)

* [Hexo Plugins](https://hexo.io/plugins/)

* [有哪些好看的 Hexo 主题？——知乎](https://www.zhihu.com/question/24422335)


原谅我最近很累，没打算去一点一点去码字记录下之前整个完整的过程了，所以呢，😅贴了一面A4大小的链接烂文...

不过还是按照我的路线写清了过程

如果之后会有什么问题的话，我会在本文后面补充说明，嗯就是这样

如果还有更新的话，我也会在[详细的更新日志](https://zhouie.cn/posts/201804271/)中给出


## 待补充

🙏希望少出问题啦