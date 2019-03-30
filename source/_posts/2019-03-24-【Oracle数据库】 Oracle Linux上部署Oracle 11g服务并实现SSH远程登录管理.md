---
title: 【Oracle数据库】 Oracle Linux上部署Oracle 11g服务并实现SSH远程登录管理
categories:
  - Oracle数据库
tags:
  - Oracle数据库
  - Oracle 11g
  - SSH
abbrlink: 201903241
date: 2019-03-24 00:03:09
---

### 写在前面
贴上与这次安装相关文件、安装包的链接：
* 实体机（客户端）
链接：https://pan.baidu.com/s/13lpEv04jb1AL41yFRkU6kQ 
提取码：`dqa0`
* 虚拟机（服务器端）
链接：https://pan.baidu.com/s/1AYuwHc2ECMawgc8sEf-H7Q 
提取码：`jel2`

> 如遇链接失效，Mail To `nanzhouieATqq.com`!

### 实验内容
1. 安装 `virtualbox` 虚拟机（我更喜欢的是`VMware`）；
2. 在虚拟机上安装`Oracle Linux`；
3. 在`Oracle Linux`上安装`Oracle 11g`；
4. 配置虚拟机的网络，数据库服务器的监听，`TNS`，使得可以远程访问数据库服务器。
5. 采用远程登录方式，使用`ssh`登录数据库服务器，进行数据库实例管理。
6. 采用远程访问方式，使用 `i*sqlplus`或者第三方管理软件登录服务器，进行数据库实例管理。
7. 建立`HR`的模式（建议使用官方脚本）。

### 实验前期准备
#### 软件目录
| 名称 | 版本号  |
| :--:  |  :--: |
| Vmware虚拟机 | 15.0.0-full-10134415-64bit  |
| linux_11gR2_database_1of2 | 11.2.0.1.0 - 64bit  |
| Putty.exe | Release 0.70-64bit  |
| Toad for oracle | 12.8.0.49 -32bit  |
| Oracle_11gR2_client | 11g Release2 (11.2)-32bit  |
| Oracle Linux文件(DOC-1002902.ova) | Oracle Linux x64  |

#### 准备配置文件、脚本文件
* 在Oracle服务器端建立`HR(Human Resource)`模式所需的脚本执行文件；
* Oracle客户端配置`TNS`服务所需要的`tnsname.ora`、`listener.ora`文件。

> ps：以上这些安装、配置、脚本文件在上面我分享的百度云链接文件中都能找到。

### 实验方案（具体步骤）
#### 在虚拟机上安装Oracle Linux

1. 在虚拟机中导入老师提供的 `DOC-1002902.ova`文件，由于实验室32位PC机的原因，导入过程总是以失败结束；因此改选`64位PC机(配载VM虚拟机)`来操作。

![](http://upload-images.jianshu.io/upload_images/9934558-1dc8a0a2960422f5?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![](http://upload-images.jianshu.io/upload_images/9934558-4b75778372c442ba.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2. 在VM虚拟机成功安装`Oracle linux`（我的账户登录密码 ~~oracle~~ ），可以去设置语言、时区、用户名及密码等。
![](http://upload-images.jianshu.io/upload_images/9934558-acced08f8525796b?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 在Linux上安装Oracle 11g R2(服务器端)
##### 前期准备
###### 硬件环境监测
| Content |Instruction |
| :--:  |  :--: |
| 检查物理内存 | `grep MemTotal /proc/meminfo` |
| 查看交换分区 | `grep SwapTotal /proc/meminfo` |
| 查看当前内存使用情况 | `free` |

> PS：这一类指令可以有很多，但其实只要在之前导入虚拟机文件过程没有什么异常，不是很有必要去了解这些硬件环境信息，那么这一步可以跳过。

###### 检查各种补丁包、函数依赖包
* 方法：通过上网查找到需要的函数依赖包有以下这些：
	```
	binutils-2.17.50.0.6
	compat-libstdc++-33-3.2.3
	compat-libstdc++-33-3.2.3 (32 bit)
	elfutils-libelf-0.125
	elfutils-libelf-devel-0.125
	gcc-4.1.2
	gcc-c++-4.1.2
	glibc-2.5-24
	glibc-2.5-24 (32 bit)
	glibc-common-2.5
	glibc-devel-2.5
	glibc-devel-2.5 (32 bit)
	glibc-headers-2.5
	ksh-20060214
	libaio-0.3.106
	libaio-0.3.106 (32 bit)
	libaio-devel-0.3.106
	libaio-devel-0.3.106 (32 bit)
	libgcc-4.1.2
	libgcc-4.1.2 (32 bit)
	libstdc++-4.1.2
	libstdc++-4.1.2 (32 bit)
	libstdc++-devel 4.1.2
	make-3.81
	sysstat-7.0.2
	```
* 通过类似重复以下的操作，借助root用户终端 `yum search/install` 命令将这些未安装的依赖包全部安装到服务端。

![](http://upload-images.jianshu.io/upload_images/9934558-8fb3715030425bd8?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![image](http://upload-images.jianshu.io/upload_images/9934558-2bb2d34a1a21ff84?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 然后，再使用`yum`快速安装oracle预装环境——`yum install oracle-rdbms-server-11gR2-preinstall`

###### 修改用户环境配置等参数文件
* 通过`vi .bash_profile`命令进入编辑界面，点击`i`进入`insert模式`，就可以写入以下数据了，写完数据后，点击`ESC`退出`insert模式`敲入`:wq` 保存本次修改并回到终端。
	```bash
	#!/bin/bash
	ORACLE_BASE=/u01/app/oracle
	ORACLE_HOME=$ORACLE_BASE/product/11.2.0/db_1
	ORACLE_SID=prod 			#这个位置填写的是Service_name
	```
* 修改 `/etc/hosts` ,增加 `ip地址` 和 `主机名` 的对应关系，按照和上面同样的方法，添加下面这条：
	```conf
	192.168.216.128  docker.oracleworld.com  #ip地址 hostname
	```
> 注意：如果出现 `其他IP地址` 与 `hostname` 的对应关系，应当在前面加上`#`注释掉或者直接删掉。

* 同样的方法，通过终端`vi mk_dir.sh` 添入以下数据
	```cmd
	mkdir -p /u01/app/oraInventory
	chmod -R 775 /u01/app/oraInventory
	mkdir -p /u01/app/oracle
	mkdir /u01/app/oracle/cfgtoollogs
	chown -R oracle:oinstall  /u01
	chmod -R 775 /u01/app/oracle
	mkdir -p /u01/app/oracle/product/11.2.0/db_1
	chown -Roracle:oinstall/u01/app/oracle/product/11.2.0/db_1
	chmod -R 775 /u01/app/oracle/product/11.2.0/db_1
	```

###### 切换到Oracle用户
去设置里面找到`users`，解锁并修改已经生成的`oracle`用户密码，再切换用户登录到`oracle`下。
![](http://upload-images.jianshu.io/upload_images/9934558-52b8825356126603?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##### 安装Oracle 11g
在终端输入命令：`unzip oracle11g 11.2.0.4` 两个安装文件（也可以手动合并到同一个文件夹下），进入安装文件中的目录`database`，运行命令: `./runInstaller`
![](http://upload-images.jianshu.io/upload_images/9934558-0bfa487e1e68f80d?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
同时，如果由于环境配置的原因，发现随后`安装界面出现乱码`，那么最好在执行安装之前添上这样一条语句：`export LANG=en_US_UTF-8`。
![](http://upload-images.jianshu.io/upload_images/9934558-413813bdd5b19593?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
在随后出现的安装界面中，依次选择如下：
* 注意:过程中可能省去了一些安装界面（默认`Next`即可） 
![](http://upload-images.jianshu.io/upload_images/9934558-0e99996534117d4f?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
* 这里不填email等直接`next`，出现弹窗点击`Yes`；
![](http://upload-images.jianshu.io/upload_images/9934558-8eedb77173b43bf1?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
* 在接下来的界面中，依次选 `install software only`。
![](http://upload-images.jianshu.io/upload_images/9934558-46394392a2474745?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
* 选择单实例类型的数据库
![](http://upload-images.jianshu.io/upload_images/9934558-07bf8f7f631a4741?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
* 选择oracle的语言，选择英文
![](http://upload-images.jianshu.io/upload_images/9934558-0603fef4e3569852?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
* 数据库类型选择企业版
![](http://upload-images.jianshu.io/upload_images/9934558-ed49201f938c9643?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
* 这里注意查看`Oracle_Base`路径和`Oracle_home`路径，必须保证和用户的初始化参数文件一致
![](http://upload-images.jianshu.io/upload_images/9934558-2ad8b0f3de4628b0?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
* 设置 `INVENTORY DIRECTORY`
![](http://upload-images.jianshu.io/upload_images/9934558-dee547f1b175a02f?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
* 检查`oracle`用户所属的组
![](http://upload-images.jianshu.io/upload_images/9934558-cbc27bf4b8494221?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
* `preinstall`检查阶段，监测`oracle`所依赖的软件包，重新安装缺失的软件包，如果仍然有错误，那么全部忽略即可
![](http://upload-images.jianshu.io/upload_images/9934558-7f9ad57bb86e1110?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
* 最后一步监测`oracle`的情况
![](http://upload-images.jianshu.io/upload_images/9934558-de9aafeda5b4bb13?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
* 久违的安装界面
安装到这个步骤，基本上问题不多了, `copying files` 阶段时，`root`模式下开一个终端，进入安装目录下的`sysman/lib`，编辑`ins_emagent.mk`文件。把 `$(MK_EMAGENT_NMECTL)`替换成`$(MK_EMAGENT_NMECTL) -lnnz11` 
![](http://upload-images.jianshu.io/upload_images/9934558-28d397aece469d02?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
同时这个过程中有可能会有一个提示对话框，然后按照指定路径及说明方法运行两个脚本。
打开终端，通过`su – root`切换到`root`用户，再通过`bash`命令依次执行对应目录的两个脚本，然后再来这个弹窗界面点击`OK`即可。
![](http://upload-images.jianshu.io/upload_images/9934558-78b360ac2f6ac152?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![](http://upload-images.jianshu.io/upload_images/9934558-ff2980bb88e24cd1?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
* 随后，就可以点击`next`，出现如下界面，表示在虚拟机这边安装服务端的`Oracle 11g`就已经成功了
![](http://upload-images.jianshu.io/upload_images/9934558-e2f14184bd102626?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##### 服务端NETCA创建配置监听程序
###### 执行netca指令
![](http://upload-images.jianshu.io/upload_images/9934558-20dfc120af86a0f6?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![](http://upload-images.jianshu.io/upload_images/9934558-ec32de2ea25dc77c?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

###### 接下来的界面弹窗中直接默认Next
在选择TCP/IP端口号时，可能会出现一个弹窗提示：默认的1521端口可能被占用。通过查找并没有发现被占用问题，因此选择Yes继续。
![](http://upload-images.jianshu.io/upload_images/9934558-35b6376c10ba97db?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![](http://upload-images.jianshu.io/upload_images/9934558-bbd5d043cd714ddd?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##### 建立一个数据库实例DBCA建库
![](http://upload-images.jianshu.io/upload_images/9934558-65d8235b39eabd98?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![](http://upload-images.jianshu.io/upload_images/9934558-451dd8eaeb25a446?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![](http://upload-images.jianshu.io/upload_images/9934558-62e9c091087ac224?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![](http://upload-images.jianshu.io/upload_images/9934558-46af35de1b2f0b0b?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

###### 设置数据库名和SID
![](http://upload-images.jianshu.io/upload_images/9934558-8ee997ffa3c0c518?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

###### 不启用OEM
![](http://upload-images.jianshu.io/upload_images/9934558-0f5989ece6c27129?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

###### 设置密码
![](http://upload-images.jianshu.io/upload_images/9934558-9f56bcb83fdc0eba?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

###### 不需修改、直接next
![](http://upload-images.jianshu.io/upload_images/9934558-5256678d831edec4?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

###### 不启用快速恢复区、不启用归档
![](http://upload-images.jianshu.io/upload_images/9934558-f1fcbd9f66f93d70?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

###### 不需修改、直接next
![](http://upload-images.jianshu.io/upload_images/9934558-01ef04431cfb57ed?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![](http://upload-images.jianshu.io/upload_images/9934558-6421cad657428e32?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![](http://upload-images.jianshu.io/upload_images/9934558-98231589a401ccfa?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![](http://upload-images.jianshu.io/upload_images/9934558-4b6746a3369b4809?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
至此，Oracle服务端实例数据库创建完毕。

##### 服务端启用ssh服务
![](http://upload-images.jianshu.io/upload_images/9934558-97739298d1c072f1?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 在主机上安装Oracle 11g Client(客户端)
##### 检查ssh服务是否正常
下载`putty.exe`，打开以后，安装下图填入相对应信息（虚拟机IP地址可以在网络设置中找到，同时，建议虚拟机端网络连接方式选择仅主机模式）进行测试（确保虚拟机处于开启状态）。
![](http://upload-images.jianshu.io/upload_images/9934558-140897e6f9719600?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![](http://upload-images.jianshu.io/upload_images/9934558-5bbe51db491c1bde?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 安装Oracle客户端并配置本地网络服务监听
##### 点击setup.exe启动安装
* 安装过程中，除修改安装路径外，其余直接默认Next即可。
![](http://upload-images.jianshu.io/upload_images/9934558-24d4f4fc62da0ba4?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
* 选择管理员选项
![](http://upload-images.jianshu.io/upload_images/9934558-96b3632e4be7814e?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##### 配置前的一些准备
* 将之前准备好的 `listener.ora`、`tnsnames.ora`文件（两个文件里面的内容如下所示）放到如下所示目录下。
![](http://upload-images.jianshu.io/upload_images/9934558-60a9d80d7e8b3e3e?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![](http://upload-images.jianshu.io/upload_images/9934558-25e33f069563f53d?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![](http://upload-images.jianshu.io/upload_images/9934558-8b75d23f71bb816c?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
* 同时，在环境变量中添加以下系统变量。
![](http://upload-images.jianshu.io/upload_images/9934558-46beba552589c9a9?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
* 在ssh服务控制终端执行如下（开启数据库、开启监听）
![](http://upload-images.jianshu.io/upload_images/9934558-a019ff1e3459cf01?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![](http://upload-images.jianshu.io/upload_images/9934558-cc266ba67ec12f0d?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##### 本地网络服务名配置
在所有程序中找到`Net Configuration Assistant`，进入配置界面操作如下。在测试连接过程中更改登录如果忘了数据库服务端密码（以`sys`用户为例），则需要重新给`sys`设置密码（详见后面的实验问题及解决办法）。
![](http://upload-images.jianshu.io/upload_images/9934558-f0cfdb00504ac935?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![](http://upload-images.jianshu.io/upload_images/9934558-c9ff6c61433c79cb?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![](http://upload-images.jianshu.io/upload_images/9934558-927087915ef45538?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![](http://upload-images.jianshu.io/upload_images/9934558-6e86aa7a4da4062b?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![](http://upload-images.jianshu.io/upload_images/9934558-fd459eee5572500b?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 第三方软件Toad for Oracle连接数据库
##### TNS检测
进入`cmd`测试如下，说明数据库监听配置无误
![](http://upload-images.jianshu.io/upload_images/9934558-b09217e5837d98d3?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##### 使用Toad连接
打开`Toad for Oracle`中的`toad.exe`启动，输入数据库服务端用户名以及密码后成功连接会出现如下的工作界面。
![](http://upload-images.jianshu.io/upload_images/9934558-24b1520095a04c3f?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![](http://upload-images.jianshu.io/upload_images/9934558-d9044bea068e60db?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240) 

#### 建立HR的模式（服务器端进行）
##### 准备脚本文件
在如下所示目录中，将已准备好的脚本文件粘贴过来
![](http://upload-images.jianshu.io/upload_images/9934558-d483da5e7162ba69?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##### 执行脚本文件
进入数据库层面，按照下面流程执行` hr_main.sql `这个脚本文件设置参数，就可以解锁HR用户并构建`HR模式`，且数据表中都有初始样例数据。
![](http://upload-images.jianshu.io/upload_images/9934558-ffffb128c20a817f?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 实验出现的问题及解决办法
1. 第一次尝试安装时，总是在`Linking Text File`出失败，且界面卡死在那里。
![](http://upload-images.jianshu.io/upload_images/9934558-0826e3f4a60e55ee?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
* 解决办法：
虚拟机重新启动并通过恢复快照这一特性重新进行安装，主要在对那些依赖包的安装过程要尤为注意，一个一个一次检查确保已安装，虽然方法很笨拙，但至少不会出错。
同时，在`Prerequisite Checks`阶段忽略所有错误继续下一步，随后完成安装。

2. 整个过程中经常会出现各种类型的错误,下面列出一些我遇到的这类问题

| 错误类型 | 错误原因  |  解决方案  |
| :-----: | :-----: | :-----: |
|  `ORA-12162`  |  `TNS:net service name is incorrectly specified` | 给出ORACLE_SID（可赋临时值或是去host文件添加参数），重新尝试登录数据库服务端 |
| `TNS-12545`  |  `Connect failed because target host or object does not exist`  | 服务端root模式下修改host和listener.ora文件（即host文件里添上一条“IP地址 主机名”，listener文件里将host赋为修改后的主机名）|
| `ORA-01078、LRM-00109`  |  `Failure in processing system parameters`  | 将`$ORACLE_BASE/admin/数据库名称/pfile`目录下的`init.ora.\********`形式的文件copy到`$ORACLE_HOME/dbs`目录下 `initoracle.ora`即可  | 
| `ORA-12514`  |  `TNS 监听程序当前无法识别连接描述符中请求服务` | listener.ora中添加：(SID_DESC =   (GLOBAL_DBNAME=全局数据库名)    (ORACLE_HOME =依照相对应的路径来填)    (SID_NAME = 数据库服务名)    ) |
| `ORA-12154`  |  `TNS无法解析指定的连接标识符` | 重新进行配置网络，在服务端查询此数据库服务下的服务名，并重新检查配置，填写相对应虚拟机的IP地址，对于账号、口令登陆的问题，需要去sysdba用户下去设置。|

3. 用户忘记了数据库服务端的口令，导致在本地客户端进行配置网络服务时总是出现口令错误的提示
* 解决办法：按照下面的操作步骤进行，这样使用修改后的用户名和口令就可以测试成功了。
![](http://upload-images.jianshu.io/upload_images/9934558-bb8f17a7ed7beb34?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
只能选择用户状态是`open`的用户进行重新设置口令的处理
![](http://upload-images.jianshu.io/upload_images/9934558-ce0d8da4aa41d616.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 参考文章
* [⭐Oracle linux安装Oracle 11G](http://www.cnblogs.com/iyoume2008/p/6986374.html)
* [HR Schema](https://docs.oracle.com/cd/B19306_01/server.102/b14198/scripts.htm#Cihgfecd)
* [Oracle 11g中执行Execute的时候报异常ORA-01031的解决办法](https://blog.csdn.net/jave_f/article/details/78231905)
* [Oracle PL/SQL DBA 编程实践基础](https://blog.csdn.net/jave_f/article/details/78539082)
* [Oracle数据库任何用户密码都能以sysdba角色登入](https://blog.csdn.net/jave_f/article/details/77921442)
* [浅析Oracle PL/SQL 学习（部分）](https://blog.csdn.net/jave_f/article/details/77921554)
* [Oracle 11gR2安装乱码解决方案](https://jingyan.baidu.com/article/48a42057e29fb9a92425048d.html)

### 最后一点说明
这是我有史以来写过的最长最长的一篇安装配置教程了，大部分的内容步骤都还是大三选修所记录的实验报告内容。这段时间需要准备数据库知识的复习，这才重新拾起来，大体按照上面的步骤依次给重现了。让我意外的是，当时竟然没有po出一份安装配置教程放网上。所以，这次赶紧加了把劲，整理了这篇。

这里面有些地方可能比较混乱，一是因为图片量太大了，可能会有一些偏差，还有就是我得承认文中有用了几张别的地方的图片；二是因为这次的整个安装配置过程相当繁琐，很多地方可能会出现报错、失败等等，我还记得当年选修这门课第一个实验就是弄这个，当时Linux指令等一些知识还不够娴熟，好像前后花了三天时间才完事。

最后，我想说的就是，教程写的再完美，实际去操作的时候，每个人可能都难免会出现这样那样的问题，放在这篇来讲，出现最多的就是形如 “`ORA-××××××`” 的类似问题，出了问题并不可怕，去某度和谷歌搜一下“`ORA-××××××`”，只要不是特别棘手，基本上都能解决。


贴张图！！！
![提神醒脑](http://upload-images.jianshu.io/upload_images/9934558-c987f4c00df89814?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)