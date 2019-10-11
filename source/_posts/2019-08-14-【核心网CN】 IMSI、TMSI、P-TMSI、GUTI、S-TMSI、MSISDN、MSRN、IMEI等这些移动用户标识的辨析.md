---
title: 【核心网CN】 IMSI、TMSI、P-TMSI、GUTI、S-TMSI、MSISDN、MSRN、IMEI等这些移动用户标识的辨析
categories:
  - 核心网CN
tags:
  - IMSI
  - IMSI
  - TMSI
  - P-TMSI
  - GUTI
  - S-TMSI
  - MSISDN
  - MSRN
  - IMEI
abbrlink: 201908141
date: 2019-08-14 17:21:33
---


### 写在前面

3G下，接入网同时连接电路域CS（走语音的）和分组域PS（走得是IP，用于手机上网），即核心网分割为CS、PS，打电话信号走CS，数据业务信号走PS，而在LTE/EPC下，取消了CS域。

* 电路域使用TMSI和位置区标识LAI来表示用户，TMSI由VLR（拜访位置寄存器，Visitor Location Register）分配；
* 分组域使用P-TMSI和路由域标识RAI来表示用户，P-TMSI由SGSN分配。

其中，临时身份TMSI/P-TMSI只有在用户登记的位置区LA和路由区RA中才有意义。所以，它应该与LAI或RAI一起使用。IMSI和TMSI的关联保存在用户登记的拜访位置寄存器VLR/SGSN中。


### IMSI

* International Mobile Subscriber Identification Number，即国际移动用户识别码。

* 国际上为唯一识别一个移动用户所分配的号码。此码在所有位置，包括在漫
游区都是有效的。

* 结构组成：IMSI=MCC+MNC+ MSIN，其中：
MCC（ Mobile Country Code）指移动国家代码 ，三个数字，如中国为460；
MNC（Mobile Network Code）指移动网络代码，两个数字，如中国邮电的MNC为00；
MSIN（Mobile Subscriber Identification Number ）指移动用户标识，在某一PLMN内MS唯一的识别码。编码格式为：H1 H2 H3 S XXXXXX（不超过10位），不同运营商有不同的与MSISDN间的对应关系。

*  采取E.212编码格式。

* 典型的IMSI举例：460-00-4777 770001

### TMSI

* Temporary Mobile Subscriber Identity，即（电路域）用户临时标识符。

* 为了加强系统的保密性而**在 VLR 内分配的临时用户识别**，在某一 VLR 区域内与 IMSI 唯一对应。

* 由4个十进制数表示，使用BCD编码。注意： TMSI不可能设置全32位均为1（这是由于TMSI必须存储在SIM里，SIM用4个十进制全为1来标识没有可用的TMSI)。

* 当MS在位置区LA第一次注册时，就会将一个 TMSI 分配给MS，当MS离开该位置区LA时将释放该 TMSI 。当 MS 每次改变位置区LA时，执行 TMSI 再分配程序。 

### P-TMSI

* （分组域）用户临时标识符

* 为了加强系统的保密性而**在SGSN内分配的临时用户识别**，在某一SGSN区域内与IMSI唯一对应。

* P-TMSI由3个十进制数组成。注意： 不会配置P-TMSI全24位都为1（这是由于P-TMSI有效位必须存储在SIM里，SIM用3个十进制数均设置为1，表示没有有效的P-TMSI值）。

### GUTI

* **在 MME 内分配的临时用户识别**。

* 用于EPS系统中在不暴露UE永久性标识的同时为UE提供一个明确的身份标识。在网络与UE进行信令交互时，网络与UE可以使用它建立用户标识。

* 在MME所控制的域内，用户使用M-TMSI进行标识。

* 结构组成：GUTI = GUMMEI+M-TMSI，其中：
<GUMMEI> = <MCC><MNC><MME Identifier>
 <MME Identifier> = <MME Group ID><MME Code>

* MCC和MNC的格式及长度如前所述，M-TMSI长度为32bit，MME Group ID长度为16bit，MME Code长度为8bit。


### S-TMSI

* S-TMSI是**GUTI的一种缩短格式**，以保证能够对无线信令进行更有效的处理（如寻呼及服务请求）。S-TMSI由MMEC和M-TMSI组成，**用于对用户进行寻呼**。

* 结构组成：S-TMSI> = MMEC+M-TMSI，其中：M-TMSI长度为32bit，MMEC长度为8bit。

* M-TMSI由MME分配，在任一MME中不可重复。

### MSISDN

* Mobile Subscriber International ISDN/PSTN number，即国际移动设备身份码，它是**主叫用户为呼叫移动通信网中用户所需拨号的号码**。

*  结构组成：MSISDN=CC+NDC+SN，也可以表示为：CC+ N1N2N3 + H0H1H2H3 ＋ ABCD，其中：
CC：Country Code，国家码（CC），如中国的国家码为86。
NDC：National Destination Code，表示国内目的码（N1N2N3 + H0H1H2H3）。
SN：Subscriber Number，客户号码（ABCD）。

* 国内目的码（NDC）包括：移动接入码（MAC，Mobile Access Code）N1N2N3 和 HLR识别号H1H2H3H4，移动接入码（MAC）用于识别网络，我们目前采用：139、138……。HLR识别号表示用户归属的HLR，也表示移动业务本地网号。

* 若在以上号码中将国家码CC去除，就成了MS的国内身份号码，也就是我们日常所说的**"手机号码"**。

* 采取E.164编码格式（设备GT码）。

* 存储在HLR（归属位置寄存器，Home Location Register）和VLR中，在MAP接口上传送。

* 典型的MSISDN举例：86 152 7111 6868

### MSRN

* Mobile Station Roaming Number，即漫游号码。

* 在移动被叫过程中，由所在业务区的MSC/VLR临时分配，用于寻址VMSC.当移动台漫游到另一个移动交换中心(MSC)业务区时，该移动交换中心将分 配给移动台一个临时漫游号码，用于路由选择。

* 当移动台离开该区后，被访位置寄存器(VLR)和原归属位置寄存器(HLR)都将这个临时漫游号码删除，以便再分配给其他移动台使用在HLR中每一个SIM卡有唯一识别码IMSI，每个IMSI对应一个MSISDN，你拨对方号码的时候在MSC中会自动把MSISDN转换成对应的 IMSI再来处理；

* MSRN是在主叫和被叫在不同的MSC下时，MSC之间建立连接的一个漫游号码，作为身份识别。

* 结构组成：CC + NDC + SN，在此情况下，SN是MSC交换机的地址。

* **MSC是如何通过MSRN寻找到被叫用户并建立通话的呢？** ![图源百度百科](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy85OTM0NTU4LWY0ZGI5ZjczY2U1YTUyMGMucG5n)当每次呼叫发生时，主叫侧MSC会向被叫归属的HLR（Home Location Register，归属位置寄存器）请求路由信息。HLR知道被叫用户处在哪一个MSC/VLR服务区内。为了向主叫侧的MSC提供一个本次路由的信息，HLR请求被叫用户当前所处的MSC/VLR分配一个MSRN给被叫用户，并将此号码传递给HLR。HLR再将此号码转发给主叫侧MSC。此时主叫侧MSC就能根据MSRN将主叫用户的呼叫接续至被叫用户所在的MSC/VLR了。


### IMEI

* International Mobile Equipment Identity，即国际移动设备身份码。

*  结构组成：![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy85OTM0NTU4LTU4MWEwYzIzNWRmOTdjZGEucG5n)

* 其中：TAC ： Type Approval Code , 型号批准码，由欧洲型号批准中心分配。 
FAC ： Final Assembly Code , 最后装配码，表示生产厂家或最后装配所在地，由厂家进行编码。 
SNR ： Serial Number , 这 个数字的独立序号码唯一地识别每个 TAC 和 FAC 的每个移动设备。 
spare ：备用比特，当手机发送时，此位要置 0 。 

* 典型的IMEI举例：490547 40 376733 5