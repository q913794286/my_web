---
---
**移植好的源码下载：http://download.csdn.net/detail/lxj_com2006/3746433**

**Keil3 C51 8.18注册版下载：http://download.csdn.net/detail/lxj_com2006/3746501**

**网络抓包工具下载：http://download.csdn.net/detail/lxj_com2006/3746594**

 

**1      概述：**

网络上关于uIP协议栈的文章不少，大多是讲解自带的http服务器为例子，没有过多的说明作为CS客户端在实际中的应用。

 

本文主要讲述ENC28J60和uIP协议栈作为CS模式在客户端的应用，即采用主动连接与服务器进行用户数据交互，保持长连接，支持自动重连。

​        

编译器：Keil3 C51 8.18

uIP版本：0.9

ENC28J60：ENC28J60-I/SO  28-Lead SOIC

单片机：SST89E516RD(1K RAM,64K program ROM 支持在线仿真，兼容51单片机)

​                   STC89C58RD+ （512 RAM  32K program ROM）烧录测试

 

特点：查询方式收包，定时更新ARP缓存表，协议栈、收、发共用缓存（内存开销少）

事件回调函数uip_appcall()

支持ICMP/TCP/UDP，端口监听，主/被动连接

**2      代码文件结构图：**

**2.1    文件列表：**

**![http://hi.csdn.net/attachment/201111/2/0_1320212459u3up.gif](/media/clip_image001.jpg)**

**2.2    代码流程图：**

**![img](/media/clip_image003.png)**

**3      系统开销：**

对于一个完成的TCP /IP协议栈来说，uIP算占用资源比较少的，根据实际应用，本例将去除了demo程序中自带的http服务器，fs部分，将连接数、监听端口表、ARP缓存表大小都设置为1，关掉日志，统计信息，重组包，把系统开销降到更低。

 

下表描述uIP系统主要开销情况（估算）：

| **RAM（内存空间）：**334字节左右       其它全局变量60（左右）   连接状态 28*1，ARP缓存表11*1，   协议栈收，发共用缓存233+2（**可设置，由单片机内存而定**）       对应代码位置：    ![http://hi.csdn.net/attachment/201111/2/0_1320212669zZyo.gif](/media/clip_image004.jpg)   注：实际用户数据空间需要减去40（等其它扩展）字节的TCP/IP头，剩下安全字节179左右，而TCP包用户数据可以达到1460字节，uIP设置了分割包大小为安全字节（见iris抓包），服务端（如：windows）会自动将大包拆分成小包发送<=179每包，所以服务端发送大于179字节的用户数据时，客户端需要重新组包。       **uIP用户数据单包大小：**    ![http://hi.csdn.net/attachment/201111/2/0_1320212741B1iY.gif](/media/clip_image005.jpg)   IRIS抓包（设置用户数据分割片大小）：   ![http://hi.csdn.net/attachment/201111/2/0_1320212785SsoP.gif](/media/clip_image006.jpg) |
| ------------------------------------------------------------ |
|                                                              |
|                                                              |
| **SP（栈空间）：**40字节左右（最大时）       uIP充分考虑到节约内存，大部分接口函数用宏实现，即加大程序的存储空间换取更小的栈空间，协议栈核心函数基本由uip_process()一个函数完成，几乎只有几个局部变量，函数调用参数也很少，除uip_appcall外（由用户决定），其它几个子函数无嵌套调用。 |
|                                                              |
|                                                              |
| **ROM（程序空间）：**12K左右       uIP自带http演示程序编译后bin文件大小在20K左右，本例去掉了http部分，已经压缩到12K左右，所以选型单片机时ROM至少在16K以上 |
|                                                              |
| **网卡IO脚：**最少占用才4个，最多7个（包括RST）              |
|                                                              |

 

如果自己新建工程使用本例代码，请将工程属性“Target->Memocy Mode”设置成：Large: variables in XDATA，即使用最大外部内存，否则将产生编译错误，提示内存不足，因为uIP的RAM开销超过了单片机内部内存128字节(超出mov寻址范围)，所以需要movx来完成更多内存访问，有些单片机都内置了外存，打开此选项，Keil C51  C编译器会自动完成外部内存访问。

![http://hi.csdn.net/attachment/201111/2/0_1320212843rRRL.gif](/media/clip_image007.jpg)

 

**4      网卡硬件原理图：**

下图为ENC28J60网卡的参考设计图，SCK,CS,SO,SI直接PIN TO PIN接到单片机（SI和SO不需要反接，不同于串口的是SPI的SO,SI都是相对于slave而言的），有些单片自带SPI接口，例如本例中使用的SST89E516RD，但我们程序中仍然采用IO口模拟SPI方式，通用性更好。

![http://hi.csdn.net/attachment/201111/2/0_1320212890DDyF.gif](/media/clip_image009.jpg)

**5      SPI****接口驱动：**

本例SPI接口采用单片机IO口模拟，只需根据实际的硬件电路设计（IO口需要上/下拉电阻），在spi.h文件中修改IO脚定义即可，需要注意的是ENC28J60采用SPI0模式，即时钟信号上升沿接收数据，下降沿发送数据，本例中SPI通讯时序已经调通，可以直接使用，至少要接SCK,CS,SI,SO即可，INT没有使用，可以不接，如果接上，ENC28J60驱动代码已经开启了接收中断，程序中可以接收到，但在使用中断模式时，请考虑收，发包的同步情况，比如：网卡支持全双工模式，正在发送包时，又收到一个包产生中断，而uIP协议栈是共用缓存的（为节约内存），如果再次去调用协议栈，会将协议栈缓存出错。

**spi.h**

**![http://hi.csdn.net/attachment/201111/2/0_1320212928ZRRC.gif](/media/clip_image010.jpg)**

SPI相关讯息请参考其它资料，本例略。

 

**6      ENC28J60****驱动：**

ENC28J60除初始化enc28j60_init()外，还需要提供两个主要原生数据收发接口函数给uIP协议栈：enc28j60PacketReceive（）网卡收数据，enc28j60PacketSend（）网卡发数据。

 

本例中ENC28J60驱动已经调试成功，可以直接使用，在此只做简单说明，更详细请参考相关手册。

![http://hi.csdn.net/attachment/201111/2/0_1320212976eWvc.gif](/media/clip_image011.jpg)

 

注：ENC28J60初始化会等待网卡应答，错误无法进入系统。

**7      uIP****协议栈TCP应用demo：**

**7.1    uIP****代码结构：**

 ![http://hi.csdn.net/attachment/201111/2/0_1320213014q0mD.gif](/media/clip_image012.jpg)

**7.2    main.c****代码说明：**

![http://hi.csdn.net/attachment/201111/2/0_1320213051036W.gif](/media/clip_image013.jpg)

![http://hi.csdn.net/attachment/201111/2/0_1320213072Bxb2.gif](/media/clip_image014.jpg)

**8      应用程序接口uip_appcall****（）：**

对于处理应用数据的用户，对uIP整个流程做一个了解即可，uIP将处理后的结果全部都回调到uip_appcall()函数统一处理，所以重点需要完成的工作全部在uip_appcall()函数中，以下介绍一个demo代码：

 

已开启主动连接功能uip.h 行300  #define UIP_ACTIVE_OPEN 1

 ![http://hi.csdn.net/attachment/201111/2/0_13202131168eYy.gif](/media/clip_image015.jpg)

![http://hi.csdn.net/attachment/201111/2/0_1320213137035g.gif](/media/clip_image016.jpg)

**8.1    uip_send** **使用举例：**

uip_send(uip_appdata,sprintf((char*)uip_appdata,"%s","Hello,Iconnected to you! thanks"));

 

uip_send("idle",4);

 

注：uip_send并没有真正将数据发送到物理网卡，也不保证数据正确到达，仅将数据存储到uIP协议栈中，由uIP来决定发送到物理网卡（空闲时），结果将产生回调，根据事件代码进行相应处理，如：收到ACK，可继续下一包，超时重连，重发等。

**9      配置参数：**

实际应用中MAC地址，IP地址，网关地址，服务器地址，端口号，应该是可以动态设置的，而MAC，IP地址（除VLAN外）在同一网络中必须是唯一的，否则导致网络不可用。

 

MAC地址可以在烧录程序时让烧录程序（或烧录器）自动加一，写到ROM固定位置，出厂时就设定好，但是MAC地址是有标准的，去IEEE申请还没有这个必要，可以借用其它厂家的，或是用**01:02:34:56:78:90:AB**这样累加的地址，但MAC地址冲突或不可用这种情况是会有的，比如有些交换机是会拒接一些MAC地址的，还有就是与部分网卡MAC冲突等。

 

本机IP,服务器IP，网关，端口号这些地址必须是可供用户修改的，出厂时无法确定用户的网络环境，需要提供UI供用户修改并保存到E2PROM或其它Flash中。

 

以下为各自参数配置代码位置：

**9.1    用户设定：**

初始化时将从E2PROM或flash中读取的设置参数替换以下处即可：

 ![http://hi.csdn.net/attachment/201111/2/0_132021320158Uv.gif](/media/clip_image017.jpg)

**9.2    固定（仅供测试）：**

以下是固定地址，由编译时决定，出厂后无法修改，不建议使用：

![http://hi.csdn.net/attachment/201111/2/0_1320213234j7Ye.gif](/media/clip_image018.jpg)

**10            uIP****协议栈事件列表：**

见uip.h 行：493-600，大部分事件已在demo代码中描述。

uipotp.h uIP协议栈的配置参数

**11            常见问题：**

测试中发现windows操作系统，会出现TCP checksum错误，导致丢包现象，是由于网卡硬件校验原因：

 

解决办法：

网卡配置->高级->Rx Checksum Offload/Tx Checksum Offload，很可能你的这两处
 设置是Enable，将之调整成Disable即可，代价是网络性能降低。
 一般由操作系统的TCP/IP协议栈完成TCP/UDP/IP校验和的计算工作，这两处设置成
 Enable之后，协议栈不再进行校验和的计算，而是由网卡自己完成。如果在前述位置
 没有发现Rx Checksum Offload/Tx Checksum Offload项，有两种可能，一种是网卡
 本身不支持这种功能，另一种是网卡驱动未提供配置项，后一种情形居多。

 ![http://hi.csdn.net/attachment/201111/2/0_1320213673NUYe.gif](/media/clip_image019.jpg)