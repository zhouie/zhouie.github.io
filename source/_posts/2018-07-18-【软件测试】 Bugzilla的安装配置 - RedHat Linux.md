---
title: 【软件测试】 Bugzilla的安装配置 - RedHat Linux
categories:
  - 软件测试
tags:
  - Bugzilla
  - 缺陷管理
  - 实习
abbrlink: 201807181
date: 2018-07-18 22:13:31
---

### 写在前面

这次长沙实习中接触到了 “Linux环境下安装配置Bugzilla” 这个需求，其实最开始也是不明所以的，连Bugzilla是什么都不清楚，也是安装教程稀里糊涂地给装好了。至于安装教程下面也会讲到，不过在这之前，先看一下Bugzilla系统究竟是什么吧？

> Bugzilla是Mozilla公司向我们提供的一个开源的免费缺陷跟踪工具,它能够为你建立一个完善的Bug跟踪体系，包括报告Bug、查询Bug记录并产生报表、处理解决、管理员系统初始化和设置四部分

因为Bugzilla是开源的，所以在网上也有很多基于此开发出的在线缺陷跟踪站点；我这里讲的是如何在Linux环境下安装配置Bugzilla，当然，在windows平台下依然可以成功安装使用，这个有兴趣的可以自行去尝试一下，我个人觉得没必要。毕竟将服务端部署在Linux环境下维护起来更加灵活方便一些。

注意：以下操作是在虚拟机上进行的...

### 准备点什么
#### 下载

* [Red Hat Linux光盘映像文件](https://pan.baidu.com/s/1jGPsHFOiDGD47gHHXhhfdA)，提取码：skny
* [Bugzilla光盘映像文件](https://pan.baidu.com/s/1xENl7EHPPNOlkxAIlWpXQg)，提取码：3f4f

#### 安装环境

* 操作系统：
    - 发行版本 Red Hat Linux Realease 9（Shrike）
    - 内核版本 Linux 2.4.20-8 #1 Thu Mar 13 17:54:28 EST 2003 i686 i686 i386 GNU/Linux
* Perl解释器：Perl-5.8.0-88
* 数据库：MySQL Ver 12.22 Distrib 4.0.21 ,for pc-linux
* Web服务器(Apache)：httpd-2.0.40-21

补充说明：

1、如何查看发行版本、内核版本

查看内核版本

```shell
uname -a            #显示电脑以及操作系统的相关信息
或
cat /proc/version   #正在运行的内核版本
```

查看发行版本

```shell
cat /etc/redhat-release
或
cat /etc/issue
```

> 说明一下：
> 内核版本 Linux 2.4.20-8 #1 Thu Mar 13 17:54:28 EST 2003 i686 i686 i386 GNU/Linux
> 其中，2是主版本，4是次版本（偶数代表稳定版本，奇数代表测试版本），20是release version，8是revised version。

2、如何查看perl解释器、MySQL数据库、Apache服务器版本

```shell
rpm -q perl         #perl解释器
rpm -q httpd        #Apache服务器版本
mysql --version     #MySQL数据库
```

### 正式安装配置
#### 提前准备

##### 安装Apache(httpd服务)

注意，这需要挂载至磁盘1，也就是`shrike-i386-disc1.iso`磁盘下，至于怎么挂载，请参照下面的动图来操作即可。

```shell
mount /dev/cdrom /mnt/cdrom
```
挂载之后，再去安装httpd服务（因为Linux镜像文件自带httpd服务的安装包，所以没去在线下载，当然了在线下载也是可以的），因为我之前已经安装过一次了，所以，再次安装它会提示我`Already installed`。

![](https://i.loli.net/2019/02/24/5c729679b3fcb.gif)

##### 修改IP信息

修改IP地址确保虚拟机与主机网络地址相同（前三个字段），这样互相才能ping通。因为我的网络IP地址是随机分配的，所以我这里选择临时修改IP地址方式。当然网上也有更多一些其他方式，自查

```shell
ifconfig eth0 192.168.1.103 netmask 255.255.255.0
```

![](https://i.loli.net/2019/02/24/5c7296794d331.png)

#### 第一阶段

将磁盘挂起至`/mnt/cdrom`目录下，挂起步骤如上所示（ps：`umount /mnt/cdrom` 解开挂载指令）

```shell
mount /dev/cdrom /mnt/cdrom
```

![](https://i.loli.net/2019/02/24/5c729679cc472.gif)

磁盘挂载后，这个目录`/mnt/cdrom`下有着这些文件：(3个MySQL.rpm文件、gcc.rpm文件、perl文件夹、buzilla压缩包)

![](https://i.loli.net/2019/02/24/5c7296794ebcc.png)

之后我们需要将`/mnt/cdrom`中的文件复制cp到`/tmp`目录下

```shell
mkdir /tmp/perl
cp /mnt/cdrom/* /tmp/
cp /mnt/cdrom/perl/* /tmp/perl/
```

![](https://i.loli.net/2019/02/24/5c72967957cbe.png)

解压bugzilla文件包，然后将文件移到`/var/www/html/bugzilla`目录下（ps:`/var/www/html`为apache的 docroot，可以在`httpd.conf`中修改`DocumentRoot "/var/www/html"`这条语句来换个位置）

![](https://i.loli.net/2019/02/24/5c72967952169.png)
![](https://i.loli.net/2019/02/24/5c729679505bd.png)

注意：记住上面提到的几个目录：`/tmp`、`/tmp/perl`、`/var/www/html/bugzilla`，在整个安装过程中可能会在这几个目录来回切入切出，所以心里要有点数：每个目录下的文件有哪些？是干嘛的？

#### 第二阶段

下面就算是正式进入Perl模块的安装了，很多操作可能是重复的，所以有点耐心吧

主要安装的组件有哪些呢？可以通过这个命令来实时看一下自己还需要安装哪些

![](https://i.loli.net/2019/02/24/5c7296e936292.png)
![](https://i.loli.net/2019/02/24/5c7296e9347f2.png)
![](https://i.loli.net/2019/02/24/5c7296e939aa3.png)

* AppConfig
* CGI
* TimeDate
* DBI
* MySQL
* DBD(要在MySQL装了之后再装)
* gcc
* gd
* GD
* Template-toolkit
* GDTextUtil
* GDGraph
* Chart

perl指令说明
`perl Makefile.PL`(生成Makefile安装文件，没生成功的，删除Makefile文件、再装)、`make`(编译)、`make test`(测试能不能安装，有无系统或文件依赖) 、`make install`(安装)、`cd  /var/www/html/bugzilla`(返回bugzailla看看是否安装成功)、`perl checksetup.pl |more`(或 `./checksetup.pl|more`，看到Appconfig为OK就是安装好了)

注意：`make test`、`cd  /var/www/html/bugzilla`、`perl checksetup.pl |more`这些在接下来的安装过程中是用来检查安装情况的，是可以不用每次都敲一遍的。

安装下面的顺序来安装即可

(一)、安装AppConfig-1.56
进入/tmp/perl,解压AppConfig-1.56.tar.gz，然后进入AppConfig-1.56文件，perl Makefile.PL、make、make test 、make install、cd /var/www/html/bugzilla、perl checksetup.pl |more

![](https://i.loli.net/2019/02/24/5c7296e92ea66.png)
![](https://i.loli.net/2019/02/24/5c7296e93b480.png)

(二)、安装CGI.pm-3.05
进入/tmp/perl,解压CGI.pm-3.05.tar.gz，然后进入CGI.pm-3.05,perl Makefile.PL、make、make test 、make install、cd /var/www/html/bugzilla、perl checksetup.pl |more

![](https://i.loli.net/2019/02/24/5c7296e93057a.png)
![](https://i.loli.net/2019/02/24/5c7296e937f9f.png)
![](https://i.loli.net/2019/02/24/5c7296e931cd6.png)

(三)、安装TimeDate-1.16
进入/tmp/perl,解压TimeDate-1.16.tar.gz，然后进入TimeDate-1.16,perl Makefile.PL、make、make test 、make install、cd  /var/www/html/bugzilla、perl checksetup.pl |more

![](https://i.loli.net/2019/02/24/5c729748e2199.png)
![](https://i.loli.net/2019/02/24/5c729748ea40e.png)

(四)、安装DBI-1.45
进入/tmp/perl,解压DBI-1.45.tar.gz，然后进入DBI-1.45,perl Makefile.PL、make、make test 、make install、cd /var/www/html/bugzilla、perl checksetup.pl |more

这里就不放截图了哦，过程步骤同上面是类似的。

(五)、安装MySQL-client-4.0.21-0.i386.rpm、MySQL-devel-4.0.21-0.i386.rpm、MySQL-server-4.0.21-0.i386.rpm

![](https://i.loli.net/2019/02/24/5c72974983945.png)

(六)、安装DBD-mysql-2.9004(要在安装了数据库之后安装)
进入/tmp/perl,解压DBD-mysql-2.9004.tar.gz，然后进入DBD-mysql-2.9004.,unset LANG(清掉设置，如有Makefile文件将它删掉)、perl Makefile.PL、make、make test 、make install、cd  /var/www/html/bugzilla、perl checksetup.pl |more

![](https://i.loli.net/2019/02/24/5c729749869c7.png)
![](https://i.loli.net/2019/02/24/5c72974985168.png)

(七)、安装gcc -3.2.2-5.i386.rpm

![](https://i.loli.net/2019/02/24/5c729749da754.png)

(八)、安装gd-2.0.33
进入/tmp/perl,解压gd-2.0.33.tar.gz，然后进入gd-2.0.33,./configure、make、make check、make install、cd /var/www/html/bugzilla、perl checksetup.pl |more

![](https://i.loli.net/2019/02/24/5c7297864888c.png)
![](https://i.loli.net/2019/02/24/5c729787eb935.png)

(九)、安装GD-2.30
进入/tmp/perl,解压GD-2.30.tar.gz ，然后进入GD-2.30,perl Makefile.PL、make、make test、make install、cd /var/www/html/bugzilla、perl checksetup.pl |more

这里就不放截图了哦，过程步骤同上面是类似的。

(十)、安装Template-Toolkit-2.14
进入/tmp/perl,解压Template-Toolkit-2.14.tar.gz，然后进入Template-Toolkit-2.14,perl Makefile.PL、make、make test、make install、cd  /var/www/html/bugzilla、perl checksetup.pl |more

这里就不放截图了哦，过程步骤同上面是类似的。

(十一)、安装GDTextUtil -0.86
进入/tmp/perl,解压GDTextUtil -0.86.tar.gz，然后进入GDTextUtil -0.86,perl Makefile.PL、make、make test、make install、cd  /var/www/html/bugzilla、perl checksetup.pl |more

这里就不放截图了哦，过程步骤同上面是类似的。

(十二)、安装GDGraph-1.43
进入/tmp/perl,解压GDGraph-1.43.tar.gz，然后进入GDGraph-1.43,perl Makefile.PL、make、make test、make install、cd /var/www/html/bugzilla、perl checksetup.pl |more

这里就不放截图了哦，过程步骤同上面是类似的。

(十三)、安装Chart-2.3
进入/tmp/perl,解压Chart-2.3.tar.gz，然后进入Chart-2.3,perl Makefile.PL、make、make test、make install、cd /var/www/html/bugzilla、perl checksetup.pl |more

这里就不放截图了哦，过程步骤同上面是类似的。

(十四)、验证检查一下目前的安装情况

再次执行perl checksetup.pl |more 命令，看看是否以上组件是否都安装好了


#### 修改配置文件

(一)、修改localconfig文件

> localconfig文件包含安装时需要设定的很多重要信息，比如比如，`$webservergroup='daemon'` 表示apache使用的group；`$db_driver = 'mysql';`表示使用的数据库；`$db_host = 'localhost';`表示数据库服务器ip；`$db_name = 'bugs';`表示数据库名称；`$db_user = 'bugs';`表示连接数据库的用户名；`$db_pass = 'bugs';`表示连接数据库的用户密码。

我们需要根据实际情况来手动修改这些配制项。特别的数据库账户，需要我们事先在数据库中创建出这个账户并赋予其相应权限，以便下一步安装时通过通过该用户执行建库脚本！

`cd /var/www/html/bugzilla`，进入修改bugzilla目录下，`vi localconfig`，修改localconfig文件

![](https://i.loli.net/2019/02/24/5c72978779fc1.png)

(二)、修改Apache的配置

> 我们需要通过配置来告诉Apache新安装的bugzilla的位置，并且特别告知它是一个cgi程序，具体配制方法就是在apache的conf/httpd.conf文件中加入以下代码：

![](https://i.loli.net/2019/02/24/5c729786e6cf7.png)

说明：
`AddHandler cgi-script .cgi` 指明这个目录是cgi应用；`Options Indexes ExecCGI` 赋予执行 cgi应用的权力 
Apache的配置文件 httpd.conf 在`/etc/httpd/conf`目录下，注意，修改后需重启Apache服务！ 


#### 最后一步

![](https://i.loli.net/2019/02/24/5c7297879f355.png)

按照上面的操作走完之后，将会提示输入管理员邮件地址、真实用户名、密码，至于，bugzilla的安装已经完成。

访问服务器IP/bugzilla（这里的服务器IP就是之前配置的临时IP地址，这个依照你主机的IP地址来定就好，比如我的就是http://192.168.1.103/bugzilla/index.cgi）

![](https://i.loli.net/2019/02/24/5c729787e537c.png)