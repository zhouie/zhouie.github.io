---
title: 【分享】 Md2All Markdown排版利器
categories:
  - 分享
  - 博客
tags:
  - Markdown
  - 图床
  - Tool-share
abbrlink: 201808032
date: 2018-08-03 17:31:22
---

### 关于Md2All

> 有这样一位程序猿，也同样被公众号的Markdown排版折磨得苦不堪言，花了差不多一个星期的时间，研究各种现有的工具，但并没有找到较为满意的解决方案。
> 因为这样程序猿比较喜欢较真，于是就萌生了自己写一个公众号Markdown排版工具的想法，这就有了现在的Md2All。虽然开发的过程也因遇到的各种奇芭的坑而艰苦异常，但结果还是令人满意的，起码上面提到的问题都解决了。

[Markdown排版利器：Md2All](http://md.aclickall.com)

* 支持 **一键排版**和自定义css；
* 支持边编辑边预览；
* 支持左右滚动联动；
* 对IT人士特别友好，支持80多种代码主题；
* 支持通用的Markdown语法和部分扩展语法(如：表格，任务列表，latex数学公式，注脚等...)，并对html，css样式有很好的支持；
* 能让Markdown内容，无需作任何调整就能一键复制到微信公众号、博客园、掘金、知乎、csdn、51cto、wordpress、hexo等平台。

**一键排版** 中提供了几种常用的排版样式模版，也提供了足够多的注释，让初学者也能很容易根据注释中的提示个性化自己的样式。
如果你觉得现有的样式模板不适合你，那就大胆尝试去改吧，就算改错了也就”恢复预设值“就OK了，所以不用担心啊。ps：记得保存了才会生效啊。

Md2All教程请参考：[https://www.cnblogs.com/garyyan/p/8329343.html](https://www.cnblogs.com/garyyan/p/8329343.html)


### 整体体验

![](http://p7n85i5tr.bkt.clouddn.com/zhouie/img/Md2All/1.gif)


### Md2All的云图床

#### 准备步骤

1、注册七牛云帐号

为什么用七牛云呢？
七牛云技术已相当成熟，在图片存储管理上已经有些年头了，从知乎上来看，在国内目前的图床服务上属于靠前的服务商了，而且，目前很多国外的图床服务还是被墙没法使用，具体可供大家参考选择的[图床工具分享](https://zhouie.cn/posts/201804241/)

还没有七牛云帐号的朋友，😉欢迎通过我的邀请注册链接：[https://portal.qiniu.com/signup?code=3lowm9s25ur82](https://portal.qiniu.com/signup?code=3lowm9s25ur82)完成七牛云的注册，注册后会发邮件到你邮箱，通过邮件激活并登录即可。

> ps：注册的时候，你要填一项个人网站域名，如果没网站的，就请随便填一个真实存在的网站吧，`www.baidu.com`也是可以的哦~

2、新建七牛云存储空间(bucket)

> 现在七牛云的新建存储空间要个人身份认证，这里建议用支付宝认证，十分钟左右就可以收到认证通过的信息了。ps：认证通过后会有10G/月的免费空间，这肯定是够用了吧...

按照 `登录七牛云->管理控制台->对象存储->新建存储空间` 操作顺序新建一个存储空间即bucket

![](http://p7n85i5tr.bkt.clouddn.com/zhouie/img/Md2All/5.png)

点击该 bucket，在右边看到一个测试域名，该域名是图片上传后的访问域名即 `bucketDomain`。这里要特别注意域名不要少了前面的`http://`头部。

3、设置云图床

3.1、`Acess Key`和`Secret Key`

![](http://p7n85i5tr.bkt.clouddn.com/zhouie/img/Md2All/6.png)

3.2、`BucketName`和`BucketDomain`

![](http://p7n85i5tr.bkt.clouddn.com/zhouie/img/Md2All/7.png)

3.3、在Md2All中设置七牛图床

在 Md2All 中点击左上角的`图片`图标，然后参考下面的1,2,3,4和上图的1,2,3,4对应，并记得保存设置

![](http://p7n85i5tr.bkt.clouddn.com/zhouie/img/Md2All/8.png)

4、测试一下

按照下面的显示效果动图操作一番，看看`内容管理`界面是否成功上传。

![](http://p7n85i5tr.bkt.clouddn.com/zhouie/img/Md2All/9.png)

#### 效果展示

* 直接把图片拖到编辑框

注意：Markdown内容是插入到光标处，为了避免不知内容插入到那儿，拖图片前，请先点一点光标

![](http://p7n85i5tr.bkt.clouddn.com/zhouie/img/Md2All/2.gif)

* 截图，直接复制粘贴

![](http://p7n85i5tr.bkt.clouddn.com/zhouie/img/Md2All/3.gif)

* 点击上传图片选择

这个功能就不录屏了，相信聪明的你一定知道怎样做的

![](http://p7n85i5tr.bkt.clouddn.com/zhouie/img/Md2All/4.png)


### 显示Latex公式

> 目前国内很多的 Markdown编辑环境中还不支持功能强大的 Latex公式，以前最通常的做法是这样的：找一个支持 Latex公式 的网站（如CSDN)，先把 Latex公式 显示出来，然后再一个个去截屏保存。如果一两条公式也就算了，可如果公式很多，那工作量也是让人头痛。
>
> 原本一篇好好的 Markdown文档，本来打算复制过来直接发布的，却因为不支持 Latex公式 的原因，又得重新整理一次，这真的是很浪费时间。

为了解决 Latex公式的显示问题，Md2All新增了对 Latex公式 到各平台显示的完美支持。

直接支持在 Markdown中包含 Latex公式，一键 **复制**就能转换到各平台都能正常显示的 Latex公式内容。

其实它是这样做的：一键 **复制**直接把 Latex公式 转换成 图片，再次复制 就能把包含转换好的 Latex公式的内容直接复制出来，完美解决了跨平台不支持Latex公式的显示问题，免去了截图保存再上传的这种麻烦。

![](http://p7n85i5tr.bkt.clouddn.com/zhouie/img/Md2All/10.gif)


### CSS样式

注意，Md2All 中用的都是标准的css样式，而且在样式文件中有很多的注释。

举个例子来说，如果我想改`###`这个`h3`标题的效果，那就只要改样式文件中以`h3`为开头的样式的内容即可。如：

从 **默认样式**中把整个样式内容复制到 **最爱样式**中然后再进行修改。因为所有的样式模板都是从 **默认样式**中修改过来的，另外，其它的样式模板目前还有可能在不断改进中，所以在 **最爱样式**中定义自己的样式是最好的。

```css
h3 {
  font-size: 1.3em;
}
```

改为

```css
h3 {
  font-size: 2.3em;
  color:#159957
}
```

也可以直接在后面添加，后面的会覆盖前面的，如:

```css
h3 {
  font-size: 1.3em;
}
h3 {
  font-size: 2.3em;
  color:#159957
}
```

#### 全局样式自定义

![](http://p7n85i5tr.bkt.clouddn.com/zhouie/img/Md2All/11.gif)

可以看到，修改`output_wrapper{}`下的样式后，就直接影响了整个文档的显示效果，而每个样式的修改，从英文单词和GIF效果中已能很清楚地看到。

可能有人会有疑问，不是说全局的吗，为什么最上面的引用块的样式没有变？这是因为，更具体定义的元素，无论放在前面还是后面，也不会被范围更大的定义覆盖，如：

```css
blockquote
{
color:#ffffff;
}
output_wrapper
{
color:#00000;
}
```

`output_wrapper`的`color`不会覆盖`blockquote`（引用块）的。原因是`output_wrapper`是针对所有的，而`blockquote`只是针对引用块。
好吧，既然说了引用块`blockquote`，那就接着`blockquote{}`来说吧！

#### 引用块样式自定义

![](http://p7n85i5tr.bkt.clouddn.com/zhouie/img/Md2All/12.gif)

有了上面的介绍后，这儿看起来应该没压力了吧，就是改`blockquote{}`,说白了就是英语单词的事了。

#### 段落样式自定义

![](http://p7n85i5tr.bkt.clouddn.com/zhouie/img/Md2All/13.gif)

#### 粗体、斜体、删除线样式自定义

![](http://p7n85i5tr.bkt.clouddn.com/zhouie/img/Md2All/14.gif)

可以看到，对于斜体`em{}`，我把`font-style:itaic`拿掉就不斜，对于删除线`del{}`,只要添加`text-decoration:none;`就会把删除线可掉，对于强调`strong{}`可以添加`font-weight:normal;`把粗体去掉。这样你就可以把它作为其它的作用了啊。

#### 分隔线样式自定义

![](http://p7n85i5tr.bkt.clouddn.com/zhouie/img/Md2All/15.gif)

#### 行内代码样式自定义

行内代码，也就是改`code{}`

![](http://p7n85i5tr.bkt.clouddn.com/zhouie/img/Md2All/16.gif)

**行内代码**和 **代码块**一般情况都是用于代码显示，不过行内代码是可以和其它的内容放在同一行的，所以一般用行内代码来显示一些自己要突出的内容；而代码块，就是独立为显示一段代码的区域。

#### 代码块样式自定义

![](http://p7n85i5tr.bkt.clouddn.com/zhouie/img/Md2All/17.gif)

看到这儿，就算工具栏没了 **代码紧凑**的功能，也应该知道怎样实现了，另外，代码高亮也可以随心所欲地去定义了！

#### 标题样式自定义

写markdown时，我比较喜欢用`h3`，即`###`，因为一般情况下，`h3`默认的`font-size`对我来说，大小刚好合适。

但这并没有什么参考性，自己把握好喜好就行。

标题酷酷的改进

![](http://p7n85i5tr.bkt.clouddn.com/zhouie/img/Md2All/18.gif)

标题首字突出的改进

![](http://p7n85i5tr.bkt.clouddn.com/zhouie/img/Md2All/19.gif)

标题上下边框的改进

![](http://p7n85i5tr.bkt.clouddn.com/zhouie/img/Md2All/20.gif)