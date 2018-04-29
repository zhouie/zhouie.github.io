---
title: 为GitHub Pages自定义域名并添加SSL-开启https强制
categories:
  - 博客
tags:
  - SSL
  - HTTPS
abbrlink: 201804231
date: 2018-04-23 10:01:44
---

## 直奔主题

### 为什么要使用https协议？

1. 提高网站访问安全性，网络连接都是加密的 (PS：虽然`SSL`并不是无懈可击的，但是我们应该尽可能提高窃听成本)。

2. 目前越来越多的浏览器会判断当前站点支不支持https协议，提高用户信任度，体验好.

   > 例如：据外媒报道，从2017年1月份正式发布的Chrome 56开始，Google将把某些包含敏感内容的HTTP页面标记为`“不安全”`，比如含有密码或支付表单信息。Google最终目标是将所有打破了https连接的HTTP页面，用特定的`红色图标`给标记出来... 此举旨在唤起用户有关HTTP连接不安全、容易受到中间人攻击等危险的意识。通过HTTP连接发送的数据（比如密码和支付细节），会被通网络下别有用心的人轻易拦截。

3. Mozilla也在做着同样的事情——其中就有通过提供免费的TSL证书，以便其为网站服务部署https连接的`Let's Encrypt项目`。根据Chrome的遥测数据，在该浏览器每天加载的网页中，有近一半是通过https连接的。

4. 福利：使用了https之后，如果网站的访客是从其他已经使用了https的网站上跳转过来，你就能在`Google Analytics`中获取更完整的来源信息。(说到这，不得不吐槽一下”墙”，它导致 `Google Analytics`的信息延迟长达一天以上，最后我不得不再将 `Baidu Analytics` 纳入我的选择。)

5. 使用https之后，谷歌、百度等搜索排名权值（PR等）也会有相对提升。

6. And 个人原因，作为一个有个性并略有强迫症的耿直Boy，终于在前一阵子给自己的博客弄了个人域名zhouie.cn(阿里云)，怎么可以没有一把小小的`绿锁头`？！

默认情况下使用GitHub Pages的给定域名则支持http和https两种协议，但是如果使用自定义域名的话，则只能通过`http://`访问，也就是说我们在`Github上搭建 Hexo 或Jekyll 主题博客`后，通过`CNAME`绑定个人域名后，我们只能通过`http://`域名来访问。如果访问`https://XXX.github.io/`(即原来的GitHub Pages域名)将会被重定向到`我们的自定义域名`。但若直接访问`https://我们的自定义域名`，浏览器会报`SSL_DOMAIN_NOT_MATCHED`警告。

>附：`在Github上搭建 Hexo 或Jekyll 主题博客`，这个网上教程很多，提供一篇我之前有写过的一篇教程[Jekyll 搭建个人博客](https://zhouie.cn/posts/201802151/)。


那么怎么给自己的域名加上`https`呢？这个时候就需要使用第三方网站的证书了。而GitHub Pages并不支持上传SSL证书。

我在Google、Bbaidu上找相关解决办法时，又看到这样一个帖子：[GitHub Pages 自定义域名启用 SSL，各位有什么建议？](https://www.v2ex.com/t/379653)。

从网上提供的信息来看，目前方案主要有两种：

* 自己购买证书，部署到一台机器，机器部署`nginx`，`stunnel`等代理软件，反向代理到GitHub pages站点，域名指向代理机器IP(收费的SSL服务肯定是比免费的更加周到，一般收费的SSL都会提供端到端的加密，但是价格不菲，不推荐，不过这里还是分享一篇相关教程[一步步教你把HTTP网站免费转成HTTPS网站](https://zhuanlan.zhihu.com/p/29644657))；

* 使用CloudFlare提供方案1类似的设置(推荐)


---
## 借用CloudFlare 给自己的域名加个 s

[CloudFlare](https://www.cloudflare.com/) 是一家CDN提供商，它提供了`免费的https服务`(但不是应用SSL证书)。实现模式就是，用户到CDN服务器的连接为`https`，而CDN服务器到GithubPage服务器的连接为`http`，就是在CDN服务器那里加上反向代理。

> 在网上还找到这样一种解释
> Cloudflare 提供一种被他们称之为`Universal SSL`的服务，可以让任意 `HTTP `站点支持 `HTTPS`。它的原理是当访客使用 `HTTPS`访问站点的时候，从访客到 Cloudflare 这段是加密的，然后从 Cloudflare 到站点这段是明文的。虽然不是全程加密，但是也能很大程度上解决中间人，如果从 Cloudflare 到站点的信道相对可靠的话

1. 注册并登录CloudFlare，并将自己域名下的`name server`修改为CloudFlare的`name server`。
2. 在CloudFlare的DNS设置域名匹配到自己的GithubPage(启用动态DNS加速)。
3. 在CloudFlare的`Crypto`设置SSL为`Flexible`(等待一定时间实现建立连接后，就可以通过`https`来访问自己的 GithubPage )。
4. 在CloudFlare的`Page Rules`中设置路由规则。一般情况下，利用`Always use https`设置两条规则，规则链接分别为`http://域名/*` 与`http://域名/`(开启https强制跳转)。

还有其他的一些，例如Cloudflare还提供免费的`CDN`和`缓存技术`，让浏览者有更好的体验~~

### 详细步骤

* 创建CloudFlare帐户，并添加网站

首先你已经有自己的`自定义域名`的GitHub Pages ，我的 GitHub Pages CNAME文件写的是 `zhouie.cn`

> 明确一下我们的实现目标： 当访客输入`zhouie.cn` 强制跳转使用https，访问`wwww.zhouie.cn` 也会跳转到`https://www.zhouie.cn`

如果你还没有Cloudflare账号，[点击注册](https://www.cloudflare.com/a/sign-up)

* 登陆后，[点击这里](https://www.cloudflare.com/a/add-site) 增加你的域名，如下图，输入你的域名，例如 `zhouie.cn` 并点击`Begin Scan`

![](https://dha4w82d62smt.cloudfront.net/items/1p3D360D0G3y1z1C363y/Image%202018-04-23%20at%206.45.37%20PM.png)

> 注意不要写`WWW`前缀，大约60秒即可完成域名解析扫描。完成后点击`Continue Setup` 继续下一步

* 你看到`DNS记录（包括子域）列表`之后，按照下图提示设置后，其中`cname`是为了重定向`www`备的，点击`Continue`下一步

![](https://dha4w82d62smt.cloudfront.net/items/3l0J1B3N3z2U142b1I0v/Image%202018-04-23%20at%206.54.04%20PM.png)

* 选择免费计划，然后下一步~

![](https://dha4w82d62smt.cloudfront.net/items/3k0s2u3h1E3Y2W0M2J0o/Image%202018-04-23%20at%206.55.56%20PM.png)

> 到你域名控制面板修改`cloudflare`给出的域名服务器，我这里以`阿里云`为例

* 在`域名管理台`点击`管理`

![](https://dha4w82d62smt.cloudfront.net/items/2d1w3U2z2U3R2q0Q2N1R/Image%202018-04-23%20at%207.02.00%20PM.png)

![](https://dha4w82d62smt.cloudfront.net/items/2u3o1Q3z383R0O0Z2F14/Image%202018-04-23%20at%207.11.28%20PM.png)

> 注：虽然官方说明，域名服务器修改最长需要72小时生效，但我用域名做了自己测试，大约只需要 5~10 分钟，看到 **Status: Active** 即可

![](https://dha4w82d62smt.cloudfront.net/items/1t3O2b3t2Y2K2g1M303h/Image%202018-04-23%20at%207.17.33%20PM.png)

* 设置SSL
点击 `crypto `菜单 , 然后设置`Flexible SSL` ，如下图

![](https://dha4w82d62smt.cloudfront.net/items/3E1j3m3a1X3i362v431x/Image%202018-04-23%20at%207.19.14%20PM.png)

* 添加www重定向到https://yicodes.com

![](https://dha4w82d62smt.cloudfront.net/items/0o1p3o0t0G2Y0N2h2B3x/Image%202018-04-23%20at%207.23.29%20PM.png)

* 添加自动重定向到 SSL页面

![](https://dha4w82d62smt.cloudfront.net/items/2d2V021G3V2J3i2a201I/Image%202018-04-23%20at%207.26.11%20PM.png)

* 添加SSL的教程就此完成，一般需要5~30分钟生效！！！ 
![](https://dha4w82d62smt.cloudfront.net/items/3C1w0i0V2A0U241k0n0u/Screen%20Recording%202018-04-23%20at%2007.37.38.99%20PM.gif)

### 小绿锁出现了，哈哈

![](https://dha4w82d62smt.cloudfront.net/items/1v1B1F0h3f371Y3W1G3Q/Image%202018-04-23%20at%208.23.01%20PM.png)


---
## 补充[[转载]](https://steffan.cn/)
### 为什么Github Pages不支持为自定义域名添加SSL证书呢？
当我们访问自定义域名，DNS指向的地址(`Github Pages`的`IP`)会在我们的仓库中寻找`cname`文件，判断目标文件和请求来源地址的域名一致后，`Github Pages`会将`errorlife.github.io`的内容返回给用户。
![](https://dha4w82d62smt.cloudfront.net/items/2F1j3N1e1Y0T2n0w3I19/Image%202018-04-23%20at%207.52.59%20PM.png)

![](https://dha4w82d62smt.cloudfront.net/items/3F3j0e1H1H0l1R2C1Q3j/Image%202018-04-23%20at%207.56.43%20PM.png)

* Ok，这是一个比较完整的过程，但是在解析的途中，看上面两张图(tracert一下这个”裸奔”站好了 :P)，你会发现请求的地址是“美国Fastly公司CDN网络节点”，很明显这是Github的CDN，而且它只支持80端口，不支持443端口，免费的CDN一般不支持,而https默认就是使用443端口。

* 所以我们就知道Github Pages在解析域名和原生地址所分配的是不同的CDN，前者是不支持https的，而后者恰恰相反。

### 思考我们牺牲了什么
> 天下没有免费的午餐，不劳而获始终都是泡沫。所以在这个过程中我们究竟牺牲了什么？

1. 我们必须`修改域名的DNS服务器`为Cloudflare提供的地址才能使用这项服务；
2. 无法进行`coding+github双线部署`，除非你使用Cloudflare的`收费服务`，或者`反向代理`，更多思路详见`v2ex`提问的一个[帖子](https://www.v2ex.com/t/344686)。


### 检查是否引用外部资源
这个问题在我这儿并没有出现过，可是从网上教程来看，因为以往(一两年前)引用的某些外部资源还是`http`的，会出现一些问题，比如：

> 生效后我发现我的网站是不太安全的状态，而且当我点击加载脚本之后，从不太安全的状态变成了不安全的状态。

v友的解决办法：
>按下`F12`，尴尬的事情发生了，原来是配置文件和`palceholder`在作怪，由于`多说`的头像用的还是`http`，巧的是，没多久，多说就下线关门了。

![](https://dha4w82d62smt.cloudfront.net/items/133s2D1e2O2z163v0y0g/Image%202018-04-23%20at%208.17.06%20PM.png)

>这就要求我们对https协议理解通透，之后google了一下，发现https并不支持这种第三方引入资源。


---
## 参考资料
* [cloudflare官方使用指南](https://blog.cloudflare.com/secure-and-fast-github-pages-with-cloudflare/)
* [keanulee Blog](https://blog.keanulee.com/2014/10/11/setting-up-ssl-on-github-pages.html)
* [sheharyar Blog](https://sheharyar.me/blog/free-ssl-for-github-pages-with-custom-domains/)
* [Pages 博客 HTTPS 化尝试与 Universal SSL](https://blog.jamespan.me/2015/04/17/github-and-gitcafe-pages)
* [为自定义域名的GitHub Pages添加SSL 完整方案](https://www.yicodes.com/2016/12/04/free-cloudflare-ssl-for-custom-domain/)
* [Hexo支持https访问](https://www.mdslq.cn/archives/40865889.html)
* [github pages绑定了个人域名，怎么使用https访问而证书不报错呢？-知乎](https://www.zhihu.com/question/33495825)
* [为绑定域名的 GitHub Pages 启用 HTTPS](http://mazhuang.org/2016/05/21/enable-https-for-github-pages/)>>>>>>没有测试过，不过似乎已经不能用了。