---
title: 【分享】 Qiniu_upload windows工具
categories:
  - 分享
  - 博客
tags:
  - 七牛云
  - 图床
  - Tool-share
abbrlink: 201808033
date: 2018-08-03 23:11:31
---

介绍一款 **提升markdown贴图体验的实用小工具**，支持 `windows` 及 `mac`，基于 `AutoHotkey` 和 `qshell` 实现，支持本地文件、截图及网络图片一键上传至七牛云，并获取图片的 markdown引用 至剪贴板（`Ctrl+Alt+V`），并自动粘贴到当前编辑器。

* 支持各种格式图片上传
* 支持截图及网络图片直接复制上传
* 支持各种格式单个文件上传
* `AutoHotkey`开放源码，完全免费
* 安装使用非常简单

### 参考文档
* [windows版本markdown一键贴图工具（2.x及以上版本）](http://jverson.com/2017/05/28/qiniu-image-v2/)
* [AutoHotkey&qshell实现图片一键上传七牛并返回markdown引用（适用1.x版本）](https://jverson.com/2016/08/30/autohotkey-markdown-uploadImage/)
* [AutoHotkey中文帮助](http://ahkcn.sourceforge.net/docs)
* [You can use the following modifier symbols to define hotkeys](https://autohotkey.com/docs/Hotkeys.htm#Symbols)
* [Powershell-Scripts for dumping images from clipboard](https://github.com/octan3/img-clipboard-dump)

### 安装

1、 下载 qimage-win
首先从 [github](https://github.com/jiwenxing/qimage-win/releases) 下载最新的release版本(推荐使用2.*正式版)，并解压到任意目录，在`qimage-win`文件夹中看到的目录结构应该如下：

![](https://i.loli.net/2019/02/24/5c729db110375.png)

其中`dump-clipboard-png.ps1`是保存截图的`powershell`脚本，`qiniu-image-upload.ahk`即完成文件上传的`AutoHotkey`脚本。

2、 安装 AutoHotkey

[AutoHotkey](https://autohotkey.com/)官网下载安装最新版本，这是一款免费的、Windows平台下开放源代码的 **热键脚本语言**，利用其通过 **自定义热键** 触发一系列系统调用从而完成自动化操作。

这款软件的安装很简单，正常安装Next就行。具体软件更多学习内容可参照——[AutoHotkey中文帮助](http://ahkcn.sourceforge.net/docs)

![](https://i.loli.net/2019/02/24/5c729db1e25f8.png)

3、 注册七牛云账号并创建一个bucket

还没有七牛云帐号的朋友，😉欢迎通过我的邀请注册链接：[https://portal.qiniu.com/signup?code=3lowm9s25ur82](https://portal.qiniu.com/signup?code=3lowm9s25ur82)完成七牛云的注册，注册后会发邮件到你邮箱，通过邮件激活并登录即可。

> ps：注册的时候，你要填一项个人网站域名，如果没网站的，就请随便填一个真实存在的网站吧，`www.baidu.com`也是可以的哦~

这个部分的操作请参照 [Md2All的云图床](https://zhouie.cn/posts/201808032/#Md2All%E7%9A%84%E4%BA%91%E5%9B%BE%E5%BA%8A)

注意，在新建七牛云存储空间(bucket)时，选择存储区域上要多加留意，[参照文档](https://developer.qiniu.com/kodo/manual/1671/region-endpoint)

| 存储区域 | 地域简称 | 上传域名 |
| :-: | :-: |:-: |
| 华东 | `z0` | 服务器端上传：`http(s)://up.qiniup.com` 客户端上传： `http(s)://upload.qiniup.com` |
| 华北 | `z1` | 服务器端上传：`http(s)://up-z1.qiniup.com` 客户端上传：`http(s)://upload-z1.qiniup.com` |
| 华南 | `z2` | 服务器端上传：`http(s)://up-z2.qiniup.com` 客户端上传：`http(s)://upload-z2.qiniup.com` |
| 北美 | `na0` | 服务器端上传：`http(s)://up-na0.qiniup.com` 客户端上传：`http(s)://upload-na0.qiniup.com` |
| 东南亚 | `as0` | 服务器端上传：`http(s)://up-as0.qiniup.com` 客户端上传：`http(s)://upload-as0.qiniup.com` |

4、 配置文件

`qimage-win` 文件夹中打开`settings.ini`文件，可以看到:

![](https://i.loli.net/2019/02/24/5c729db119e46.png)

这里的配置项都需要根据自己的七牛云账号以及所创建的 bucket 进行修改，后面的两个配置项为可选配置，默认情况下可以暂时先不管。

`ACCESS_KEY` & `SECRET_KEY`

这是`qshell`操作个人账号的账号凭证，登陆七牛账号后在`个人面板->密钥管理`中查看，或者直接访问`https://portal.qiniu.com/user/key`查看。

`BUCKET_NAME` & `BUCKET_DOMAIN`

在`对象存储->存储空间列表`中选择或新建一个存储空间即 bucket，点击该 bucket 在右边看到一个测试域名，该域名`bucketDomain`便是图片上传后的访问域名，特别注意域名不要少了前面的`http://`头部。

![](https://i.loli.net/2019/02/24/5c729db18a0e1.gif)

完成文件上传的`AutoHotkey`脚本中默认的快捷键是`^!V`，即`Ctrl+Alt+V`(其中，`^`代表`Ctrl`，`!`为`Alt`)，如果您希望修改为其它自己更习惯的快捷键，直接对`qiniu-image-upload.ahk`脚本修改即可生效。

关于`hotkey`的符号与按键对应关系请参考 [You can use the following modifier symbols to define hotkeys](https://autohotkey.com/docs/Hotkeys.htm)

### 运行

配置完成后，以管理员身份运行`qImage.exe`，这时便可以参照下面的效果显示，使用`Ctrl+Alt+V`尝试一键上传图片至七牛云指定bucket了。

### 效果展示

本地图片文件上传

![](https://i.loli.net/2019/02/24/5c729db1a6e5d.gif)

截图上传

![](https://i.loli.net/2019/02/24/5c729db19e933.gif)

其它文件上传

![](https://i.loli.net/2019/02/24/5c729db20647e.gif)

### Q&A

如果以上操作完成后，没有按照预期达到图片上传的效果，可以先自己调试找一下原因，一般报错信息会打印在cmd命令行中，但是cmd窗口一闪而过可能看不清楚，这时候需要将`settings.ini`配置文件中的可选参数`DEBUG_MODE = false`改为`DEBUG_MODE = true`以打开调试模式，再次尝试，这时候cmd窗口不会自动关闭，便可以看到具体的报错信息从而对症下药解决问题。

使用过程中的任何问题可以在github中提交[issues](https://github.com/jiwenxing/qiniu-image-tool-win/issues)

#### 常见问题一: 七牛云UP_HOST有误

具体的错误信息如下所示：

> Uploading G:\Users\Cooper\Desktop\9999.png => markdown : 201705082057_244.png …
> Progress: 100% 
> Put file error, 400 incorrect region, please use up-z2.qiniu.com, Reqid: 0wUAAGp6j6faorwU
> Last time: 0.43 s, Average Speed: 415.6 KB/s

在七牛云的官方文档中`UP_HOST`为非必填项，配置文件中使用的是默认值`http://up.qiniu.com`，而每个区域所对应的的七牛云上传域名都不同，当与你所在空间区域不匹配时便会报以上的错误信息
不过很友善的是，错误信息中已经给出了建议的`UP_HOST`，例如上面的错误信息就给出明确的提示 `please use up-z2.qiniu.com`，这时将可选配置项`UP_HOST = http://up.qiniu.com`修改为`UP_HOST = http://up-z2.qiniu.com`，保存即可。

参照上面的`存储区域-上传域名`表格，不难看出出现这种报错的原因其实是因为，默认情况下，bucket的默认存储区域是 华东 ，对应`http(s)://up.qiniup.com`，但我之前在新建bucket时将 存储区域 选择在了 华南 地区，显然对应配置文件中自然需要修改成`http://up-z2.qiniu.com`!

#### 常见问题二: powershell执行权限问题

具体的错误信息如下所示：

> set-executionpolicy : 对注册表项“HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\PowerShell\1\ShellIds\Microsoft.PowerShell”的访问被拒绝。 要更改默认(LocalMachine)作用域的执行策略，请使用“以管理员身份运行”选项启动 Windows PowerShell。要更改当前用户的执行策略，请运行 “Set-ExecutionPolicy -Scope Current User”。

这是`powershell`执行权限问题，重新以管理员权限运行`qImage.exe`即可。


### 推荐阅读 !imp

* 1小时编写一个支持七牛上传的 markdown 客户端 —— [ 技术实现篇 ](https://zhaopeng.me/1174.html)，[ 代码优化篇 ](https://zhaopeng.me/1207.html) ， [ 打包发布篇 ](https://zhaopeng.me/1218.html)，[ res](https://github.com/zhaopengme/ndpediter)，[ 备份地址](https://gitee.com/imzhpe/ndpediter)

* [qiniu-image-tool | Markdown 一键贴图工具 v.2.x ](https://jverson.com/2017/05/28/qiniu-image-v2/)，[ v.1.x ](https://jverson.com/2016/08/30/autohotkey-markdown-uploadImage/)，[ res ](https://github.com/jiwenxing/qimage-win)，[ issues ](https://github.com/jiwenxing/qimage-win/issues)，[ download](https://github.com/jiwenxing/qimage-win/releases)

* [qimage-mac | 使用alfred在markdown中愉快的贴图 ](https://jverson.com/2017/04/28/alfred-qiniu-upload/)，[ res ](https://github.com/jiwenxing/qimage-mac)，[ download ](https://github.com/jiwenxing/qimage-mac/releases)

* [QiniuUpload | 一个方便用七牛做图床然后插入markdown的小工具 ](https://www.cnblogs.com/haoliuhust/p/6084102.html)，[ res ](https://github.com/HaoLiuHust/QiniuUpload)，[ doc ](https://developer.qiniu.com/kodo/sdk/1237/csharp)

* [在Atom下配置并使用MarkDown全教程 ](https://blog.csdn.net/qq_31915279/article/details/61824114)，[ 更多参考1 ](https://blog.csdn.net/fjinhao/article/details/78577212)，[ 更多参考2 ](https://www.jianshu.com/p/af4d34d39797)

* [一个码字工作者的正确书写发文姿势 —— MWeb + Markdown Here + 七牛 实现一键发布](https://www.jianshu.com/p/c859ead1b493)

* [Laravel项目中使用markdown编辑器及图片粘贴上传七牛云 ](http://www.chairis.cn/blog/article/15)，[ 备份地址 ](https://www.cnblogs.com/ChainZhang/p/7058904.html)

* [qiniu upload files谷歌浏览器插件](https://www.jianshu.com/p/44d818f781a7)

* [Python实现七牛云图片和文件上传脚本实现](https://blog.csdn.net/sylan15/article/details/78453205)，[res](https://github.com/sylan215/upload-to-qiniu)