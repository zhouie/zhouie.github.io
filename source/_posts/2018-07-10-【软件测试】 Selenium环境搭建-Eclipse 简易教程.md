---
title: 【软件测试】 Selenium环境搭建-Eclipse 简易教程
categories:
  - 软件测试
tags:
  - selenium
  - UI自动化
abbrlink: 201807101
date: 2018-07-10 23:30:11
---

### 写在前面

* selenium是可以支持多平台的，本身的环境安装也是很简单的
* 学习的测试脚本编写是Java语言，所以之后要将selenium与eclipse集联起来
* 将selenium与eclipse集联起来也是这次配置体验中可能会出现的（一些）难点
* 对软件测试还算不上熟练掌握，如果错误，欢迎指正

### 安装（配置）步骤说明

1. 安装配置好JAVA环境（JDK 1.8以上）以及Eclipse（建议使用高版本）
2. 安装配置好mvn（Maven）本地环境
3. Eclipse安装Maven插件
4. 准备好与浏览器对应的WebDriver
5. 在Eclipse中新建`Maven Projects -> pom.xml文件中安装selenium-java dependency（依赖） -> 编写一个简易的测试脚本代码演示`

其中，selenium与eclipse集联的关键步骤在于第5步，后面也会提到。


### 安装配置JAVA环境

#### 下载与安装
这里提供一下一直在用版本号为"1.8.0_92"的JDK安装包下载链接

[JDK-1.8.0_92](https://pan.baidu.com/s/1vKczTd4XobGMFLmiiVDRIQ)，提取密码：929a

特别注意：可以不用我这里所提供的JDK安装包，去[Oracle 官网](http://www.oracle.com/technetwork/java/javase/downloads/index.html)去找相应版本JDK，那么你所下载的JDK版本也要保证至少要1.8以上

相信不少朋友的电脑上都安装配置有JAVA环境（JDK），那么可以通过`Win + R` -> `cmd` -> `java -version` 来查看是否安装了JAVA环境以及相应JDK版本

![](http://p7n85i5tr.bkt.clouddn.com/zhouie/img/selenium/1.png)

至于安装JDK的步骤，一直默认Next即可，没有什么需要特别注意的。

#### 配置本机JAVA环境

* 安装完成后，右击"我的电脑"，点击"属性"，选择"高级系统设置"

![](http://p7n85i5tr.bkt.clouddn.com/zhouie/img/selenium/2.png)

* 选择"高级"选项卡，点击"环境变量"

![](http://p7n85i5tr.bkt.clouddn.com/zhouie/img/selenium/3.png)

* 然后就会出现如下图所示的画面：

![](http://p7n85i5tr.bkt.clouddn.com/zhouie/img/selenium/4.png)

* 在"**系统变量**"中设置3项属性，JAVA_HOME,PATH,CLASSPATH(大小写无所谓),若已存在则点击"编辑"，不存在则点击"新建"。变量设置参数如下：

| 变量名       | 变量值  |
| :-:          | :-: |
| JAVA_HOME    | C:\Program Files (x86)\Java\jdk1.8.0_91 //根据自己的实际路径配置  |
| CLASSPATH    | .;%JAVA_HOME%\lib\dt.jar;%JAVA_HOME%\lib\tools.jar;//记得前面有个"." |
| Path         | %JAVA_HOME%\bin;%JAVA_HOME%\jre\bin; |

>注意：在 Windows10 中，因为系统的限制，path 变量只可以使用 JDK 的绝对路径。%JAVA_HOME% 会无法识别，导致配置失败。如下所示：
>
> >C:\Program Files (x86)\Java\jdk1.8.0_91\bin;C:\Program Files (x86)\Java\jdk1.8.0_91\jre\bin;
> 
> 如果使用1.5以上版本的JDK，不用设置CLASSPATH环境变量，也可以正常编译和运行Java程序。

* 测试JDK是否安装成功
    - `Win + R` -> `cmd`
    - 键入命令: `java -version`、`java`、`javac` 几个命令，出现以下信息，说明环境变量配置成功

![](http://p7n85i5tr.bkt.clouddn.com/zhouie/img/selenium/5.png)

#### Eclipse IDE
至于Eclipse IDE，网上版本有很多，不过我个人还是建议到[Eclipse官网](http://www.eclipse.org/)下载安装，功能完善还很放心

考虑到高版本的Eclipse IDE自带了mvn插件，不用安装能免去一些步骤，建议去安装较高版本的

>在eclipse的4.4以上的版本加入了对maven的支持，即不需要安装maven插件，但对4.4以下的版本需要自己安装插件

这里提供一个Eclipse压缩文件（该Eclipse版本号为：Neon.3 Release-4.6.3，解压打开即可使用）

[eclipse-jee-neon-R-win32-x86_64](https://pan.baidu.com/s/1RiSIkljL1PGwVoE062Nq9w)，提取密码：g440。


### 安装配置mvn（Maven）本地环境

#### 下载与安装
这里提供一个Maven程序包：[apache-maven-3.3.9](https://pan.baidu.com/s/1vnn9JAZ57nKqfPjskUTAvw)，提取密码：3xwe。当然，也可前往[https://maven.apache.org/download.cgi](https://maven.apache.org/download.cgi)下载编译过的版本

![](http://p7n85i5tr.bkt.clouddn.com/zhouie/img/selenium/6.png)

* 将文件解压到`D:\Program Files\apache-maven-3.3.9`目录下，文件目录结构如下所示：
    - bin目录：该目录包含了mvn运行的脚本，这些脚本用来配置Java命令；
    - boot目录：只包含一个文件：plexus-classworlds-2.5.2.jar，是一个类加载器框架，相当于java类的默认加载器。
    - conf目录：包含了settings.xml，一个重要的配置文件，可以全局定制Maven的行为。
    - lib目录：该目录包含了所有Maven运行时需要的Java类库。
    - LICENSE.txt
    - NOTICE.txt：记录了Maven包含的第三方软件。
    - README.txt

![](http://p7n85i5tr.bkt.clouddn.com/zhouie/img/selenium/7.png)

* 同JDK环境配置一样，新建环境变量 `MAVEN_HOME`，赋值`D:\Program Files\apache-maven-3.3.9`

![](http://p7n85i5tr.bkt.clouddn.com/zhouie/img/selenium/8.png)

* 新建环境变量 `MAVEN` ，赋值 `%MAVEN_HOME%\bin`

![](http://p7n85i5tr.bkt.clouddn.com/zhouie/img/selenium/9.png)

* 新建环境变量 `MAVEN_OPTS` ，赋值 `-Xms256m -Xmx512m`

![](http://p7n85i5tr.bkt.clouddn.com/zhouie/img/selenium/10.png)

* 编辑环境变量`Path`，增加`%MAVEN%;`

![](http://p7n85i5tr.bkt.clouddn.com/zhouie/img/selenium/11.png)

* 测试mvn是否安装成功
    - `Win + R` -> `cmd`
    - 键入命令: `mvn -v`，出现以下信息，说明mvn环境配置成功

![](http://p7n85i5tr.bkt.clouddn.com/zhouie/img/selenium/12.png)

#### 配置Maven本地仓库

* 在E盘目录下新建`maven-repository`文件夹，该目录用作maven的本地仓库。

* 打开`D:\Program Files\apache-maven-3.3.9\conf\settings.xml`文件，查找下面这行代码：

> <localRepository>/path/to/local/repo</localRepository>

`localRepository`节点默认是被注释掉的，需要把它移到注释之外，然后将`localRepository`节点的值改为我们在上面创建的目录`E:\maven-repository`，这里记得写成绝对路径，你所下载的包就会放在你填写的目录下了。

说明：`localRepository`节点用于配置本地仓库，本地仓库其实起到了一个缓存的作用，它的默认地址是 `C:\Users\计算机用户名\.m2`，我们一般会自己定义一个文件夹，让maven的依赖包装在你所想要放在文件夹下，便于以后自己的维护和管理。
当我们从maven中获取jar包的时候，maven首先会在本地仓库中查找，如果本地仓库有则返回；如果没有则从远程仓库中获取包，并在本地仓库中保存。
此外，我们在maven项目中运行`mvn install`，项目将会自动打包并安装到本地仓库中。

* 仓库镜像的配置 

maven自带的仓库是国外的maven官方的一个仓库[http://repo1.maven.org/maven2/](http://repo1.maven.org/maven2/)，一般不翻墙的话速度非常慢

在setting.xml中找到`<mirrors>…………</mirrors>`的位置，在`<mirrors>…………</mirrors>`中间加入你想要的仓库的标签，这里推荐几个国内网速比较快，资源比较全的仓库

1、阿里云的maven仓库

```
<mirror>
      <id>alimaven</id>
      <name>aliyun maven</name>
      <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
      <mirrorOf>central</mirrorOf>        
</mirror>
```

2、开源中国的仓库

```
<mirror>  
    <id>nexus-osc</id>  
    <mirrorOf>central</mirrorOf>  
    <name>Nexus osc</name>  
    <url>http://maven.oschina.net/content/groups/public/</url>  
</mirror>  
<mirror>  
    <id>nexus-osc-thirdparty</id>  
    <mirrorOf>thirdparty</mirrorOf>  
    <name>Nexus osc thirdparty</name>  
    <url>http://maven.oschina.net/content/repositories/thirdparty/</url>  
</mirror>  
```

* 运行`mvn help:system`命令验证一下，运行情况应该如下图所示，如果前面的配置成功，那么`E:\maven-repository`会出现一些文件。

![](http://p7n85i5tr.bkt.clouddn.com/zhouie/img/selenium/13.png)

* 这里补充一下，要不要设置HTTP代理呢？

如果上网没使用代理，这里则不需要配置。但是之前帮别的同学在Eclipse中安装Maven插件时失败了好多次，我也不知道是不是之前没有设置HTTP代理的原因，所以保险起见，最好还是配置一下吧

首先在cmd中输入：`ping repo1.maven.org`，如果不能ping通，则一定要先设置一下代理，设置的方式为：

进入Maven目录下，找到settings.xml文件，然后在`<proxies>`标签中加入如下信息：

```
    <proxies> 
    <proxy>
      <id>my-proxy</id>
      <active>true</active>
      <protocol>http</protocol>
      <host>114.212.80.250</host>
      <port>80</port>
     
      <username>PARK</username>
      <password>****</password>     
      <nonProxyHosts>www.park.com|*.host.com</nonProxyHosts>
    </proxy>
```

| 标签名 | 含义 |
| :-: | :-: |
| id | 代理ID，标识代理 | 
| active | 设置代理是否启用 | 
| protocol | 代理使用的协议 | 
| username | 连接代理的用户名，如此代理不需要用户名，则可把此标签删掉 | 
| password | 连接代理的密码，同上 | 
| host | 代理的网址 | 
| port | 代理使用的端口 | 

其中，`<host>`, `<username>`, `<password>`标签中改为自己的`IP地址`，`主机名`和`密码`即可


###  配置Eclipse的Maven环境

对于一些低版本的或是功能不完整的Eclipse IDE，在做这一步之前，可能就需要手动去Eclipse安装Maven插件，主要有以下两种在线安装方式：

#### 方式一、在线安装
这可以分为两种方式来下载安装
（1）、Install New Software

打开eclipse，点击`help–>Install New Software`,然后输入要下载插件的链接地址：`http://m2eclipse.sonatype.org/sites/m2e`，如下图

![](http://p7n85i5tr.bkt.clouddn.com/zhouie/img/selenium/14.png)

勾选择要下载的项，点击下一步进行下载。下载完成会自动安装。安装速度跟你自身网速和服务器有关。

（2）、Eclipse Marketplace

打开eclipse，点击`help–>Eclipse Marketplace`,然后输入`maven`回车搜索，选择以下勾选的插件"`Maven Integration for Eclipse (Luna)`"，点击`Install`安装
参考网站：[http://marketplace.eclipse.org/content/maven-integration-eclipse-luna](http://marketplace.eclipse.org/content/maven-integration-eclipse-luna)

![](http://p7n85i5tr.bkt.clouddn.com/zhouie/img/selenium/15.png)

#### 方式二、离线安装
[eclipse-maven3-plugin.zip](https://pan.baidu.com/s/1UXkFZloKFgmWQhSQL1Hnjg)，提取密码：x5hq。

这也可以分为两种方式来下载安装

（1）、link方式（自定义方式）

1. 在你的 eclipse安装的根目录下创建两个文件夹：`links`，`mavenPlugins`（文件夹名称可自定义），把`eclipse-maven3-plugin.zip`解压后的`features`和`plugins`文件夹放到`mavenPlugins`文件夹下（必须如此，注意解压后文件夹的嵌套情况）。
2. 在 `links `目录下创建一个`maven.link`（文件名称可自定义）文件，打开并输入：`path=mavenPlugins文件夹绝对路径`（需要注意文件夹路径中是"/"或者"\\"  而不是"\" ）。
3. 重启 eclipse后，再次打开`Window ---> Preferences`会发现一个多了一个选项Maven，说明Maven插件安装成功了。

（2）、直接粗暴的方式

将解压后的子文件夹`features`和`plugins`的`jar包文件`分别导入Eclipse安装目录下的`features`和`plugins`文件夹内，然后重启Eclipse就可以在`Preferences`中看到Maven选项了，即Maven插件配置成功。

![](http://p7n85i5tr.bkt.clouddn.com/zhouie/img/selenium/16.png)

保证Eclipse有了Maven插件后，那么就按照下面的步骤来 **配置Maven**:

* 在`windows -> preferences`中找到maven选项，下图

![](http://p7n85i5tr.bkt.clouddn.com/zhouie/img/selenium/16-1.png)

* 找到`maven -> installations`，选择`add按钮`，找到的maven的对应路径，如下

![](http://p7n85i5tr.bkt.clouddn.com/zhouie/img/selenium/16-2.png)

* eclipse中更新配置文件

找到`User settings`，修改配置文件为之前已经修改好了的配置文件`settings.xml`

![](http://p7n85i5tr.bkt.clouddn.com/zhouie/img/selenium/16-3.png)

至此，Maven和eclipse的集成已经完成！


### 下载浏览器对应的Driver

这里需要说明的是，Driver对应`IE浏览器`、`Mozilla Firefox浏览器`、`Chrome Google浏览器`可分为3种

下面三个链接分别对应IE浏览器、Firefox浏览器、Google浏览器的driver下载：

[Selenium.WebDriver.IEDriver](https://www.nuget.org/packages/Selenium.WebDriver.IEDriver/)

[mozilla-geckodriver](https://github.com/mozilla/geckodriver/releases/)

[chromedriver](http://npm.taobao.org/mirrors/chromedriver)

**注意**：为了避免浏览器与Driver版本上的不匹配，所以建议大家下载相应的最新版driver，并将浏览器更新至最新版，这样就可以避免出现不必要的错误。

下载好对应浏览器的Driver之后，将其解压放在浏览器安装目录下。比如，我用的是google浏览器，所以对应下载的是最高版本的`chromedriver.exe`，解压后放在对应Google浏览器的安装目录下，如下图所示

![](http://p7n85i5tr.bkt.clouddn.com/zhouie/img/selenium/16-4.png)


### Eclipse中新建Maven项目并安装selenium依赖

* 打开eclipse，点击`File –> New -> Project`,输入`Maven`搜索，选择`Maven Project`

![](http://p7n85i5tr.bkt.clouddn.com/zhouie/img/selenium/17.png)

* 一直点击Next（3次），填入相关名称信息，点击Finish完成。

![](http://p7n85i5tr.bkt.clouddn.com/zhouie/img/selenium/18.gif)

* 如果没有什么问题的话，侧边栏`Project Explorer`中会出现一个正常的文件夹（这里的正常意味着未爆红，并且有`Maven Dependencies`这样一个目录）

![](http://p7n85i5tr.bkt.clouddn.com/zhouie/img/selenium/19.png)

* 打开新建Maven项目`xtu2018`下的`pom.xml`文件，搜索定位到`<dependencies>`标签处

![](http://p7n85i5tr.bkt.clouddn.com/zhouie/img/selenium/20.png)

* 去[http://mvnrepository.com/](http://mvnrepository.com/)**maven公共库网站**搜索`selenium`，选择第一项`Selenium Java`，点击进入，建议选择最新版本（也可以结合用户数参考）

![](http://p7n85i5tr.bkt.clouddn.com/zhouie/img/selenium/21.png)

![](http://p7n85i5tr.bkt.clouddn.com/zhouie/img/selenium/22.png)

* 比如我这里选择了`Selenium Java 3.13.0`，那么点击`3.13.0`进入后，复制页面下方Maven对应代码块，粘贴至`pom.xml`文件对应位置并保存修改。

![](http://p7n85i5tr.bkt.clouddn.com/zhouie/img/selenium/23.png)

pom.xml文件修改前：
```
  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>3.8.1</version>
      <scope>test</scope>
    </dependency>
   </dependencies>
```

pom.xml文件修改后：
```
  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>3.8.1</version>
      <scope>test</scope>
    </dependency>
    
    <!-- https://mvnrepository.com/artifact/org.seleniumhq.selenium/selenium-java -->
    <dependency>
        <groupId>org.seleniumhq.selenium</groupId>
        <artifactId>selenium-java</artifactId>
        <version>3.13.0</version>
    </dependency>
   </dependencies>
```

* 在对`pom.xml`修改后并保存，Maven项目文件下的`Maven Dependencies`文件目录下会多出很多这样的`*.jar`包，如果是第一次创建Maven项目，那么加入这些`*.jar`包的时间要求很长，需要耐心等待才能看到这样的变化，当然也不乏会出一些问题，当然，这都是后话...

![](http://p7n85i5tr.bkt.clouddn.com/zhouie/img/selenium/24.png)

* 如果这些`*.jar`包加入正常，没出什么问题，也就意味着selenium依赖安装完成（其实这在上面截图中的`*.jar`包中可以找到），那么就可以很愉快地直接进入代码编写阶段了...


### 编写一个简易的测试脚本代码演示

* 移除掉`src/test/java`下的`com.xtu2018`包

![](http://p7n85i5tr.bkt.clouddn.com/zhouie/img/selenium/25.png)

* 并在`src/test/java`下新建一个包package`com.xtu2018.one`，再在该包下新建一个类`demo01.java`

![](http://p7n85i5tr.bkt.clouddn.com/zhouie/img/selenium/26.png)

* 编写一段如下所示的Demo代码

```
package com.xtu2018.one;

import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;

public class demo01 {
    public static void main(String[] args) throws Exception {
        // TODO Auto-generated method stub
        //1、找到driver路径
        System.setProperty("webdriver.chrome.driver", "C:\\Program Files (x86)\\Google\\Chrome\\Application\\chromedriver.exe");
        //2、创建一个浏览器
        WebDriver driver = (WebDriver) new ChromeDriver();
        //3、打开URL
        driver.get("http://www.baidu.com");
        //4、浏览器窗口最大化
        driver.manage().window().maximize();
        //5、在搜索框输入内容
        driver.findElement(By.id("kw")).sendKeys("12306");
        WebElement btn =driver.findElement(By.id("su"));
        btn.click();
        //延时等待
        Thread.sleep(3000);
        //6、后退
        driver.navigate().back();
        //7、前进
        Thread.sleep(3000);
        driver.navigate().forward();
        Thread.sleep(3000);
        driver.close();
    }
}
```

![](http://p7n85i5tr.bkt.clouddn.com/zhouie/img/selenium/27.gif)