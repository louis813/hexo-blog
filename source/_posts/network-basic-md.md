---
title: 网络 - 基础
date: 2017-12-26 22:45:09
tags: network
---

# 概述

长篇系列，从网络协议，负载均衡，后端服务.

# 网络协议

网络技术发展到现在，形成了八个协议簇（TCP/IP、ISO、MICROSOFT、VOIP、VPN/Security、IBM、APPLE、Industrial Control）。下面主要针对 ISO 和 TCP/IP  进行介绍。

## OSI 参考模型

20世纪80年代，网络数量和规模均呈现出巨大的增长，使用不同规范和实现方法的网络之间的通信变得越来越困难。为了解决这个问题，国际标准化组织 [ISO](https://www.iso.org) 研究了不同的网络方案，创建了 [OSI](https://www.iso.org/standard/20269.html) 参考模型，以确保全世界各公司开发的不同类型的网络技术之间具有更好的兼容性和互操作性。虽然还有许多其他模型，但如今大部分网络供应商选择将自身产品与 OSI 参考模型关联（在第一二三层）。

根据分而治之的原则，OSI 分为七个层次，划分原则，具体分层如下：

| 层 | 名称 |  单元 | 描述 | 
| --- | ----- | --- | -----------------|
| 1 | 物理层 | 比特| 定义网络的物理结构，传输的电磁标准，Bit流的编码及网络的时间原则，如分时复用及分频复用，负责实际的信号传输。主要设备：交换机，调制解调器，光纤，电缆，双绞线。 |
| 2 | 数据链路层 | 数据帧 | 在两个主机上建立数据链路连接，向物理层传输数据信号，并对信号进行处理使之无差错并合理的传输。 主要设备：二层交换机、网桥。主要协议：WiFi，GRPS，PPPoE。|
| 3 | 网络层 | 数据包 | 主要负责路由，选择合适的路径，进行阻塞控制等功能。主要设备：路由器。主要协议：IP。 |
| 4 | 传输层 | 数据段 | 最关键的一层，向用户提供可靠不可靠的/端到端服务，它屏蔽了下层的数据通信细节，让用户及应用程序不需要考虑实际的通信方法。主要协议：TCP、UDP。 |
| 5 | 会话层 | 数据 | 主要负责两个会话进程之间的通信，即两个会话层实体之间的信息交换，管理数据的交换。代表：HTTP。|
| 6 | 表示层 | 数据 | 处理通信信号的表示方法，进行不同的格式之间的翻译，并负责数据的加密解密，数据的压缩与恢复。代表：HTTP。 |
| 7 | 应用层 | 数据 | 保持应用程序之间建立连接所需要的数据记录，为用户服务。代表：HTTP。 |

第一层到第三层，负责创建网络通信连接的链路(网络硬件厂商关注)；第四层到第七层，具体负责端到端的数据通信（网络应用服务商关注）。

每层完成一定的功能，每层都直接为其上层提供服务，并且所有层次都互相支持，而网络通信则可以自上而下（在发送端）或者自下而上（在接收端）双向进行。

当然并不是每一通信都需要经过 OSI 的全部七层，有的甚至只需要双方对应的某一层即可。例如物理接口之间的转接，以及中继器与中继器之间的连接，就只需在物理层中进行；而路由器与路由器之间的连接则只需经过网络层以下的三层即可。总的来说，双方的通信是在对等层次上进行的，不能在不对称层次上进行通信。

OSI 模型的作用:

* 人们可以很容易的讨论和学习协议的规范细节。
* 层间的标准接口方便了工程模块化。
* 创建了一个更好的互连环境。
* 降低了复杂度，使程序更容易修改，产品开发的速度更快。
* 每层利用紧邻的下层服务，更容易记住个层的功能。
* 一个定义良好的协议规范集，并有许多可选部分完成类似的任务。
* 定义了开放系统的层次结构、层次之间的相互关系以及各层所包括的可能的任务。
　　
OSI 参考模型并没有提供一个可以实现的方法，而是描述了一些概念，用来协调进程间通信标准的制定。即 OSI 参考模型并不是一个标准，而是一个在制定标准时所使用的概念性框架。

OSI 传输数据的封装和解封流程，如下图：

![osi detail](https://i-technet.sec.s-msft.com/dynimg/IC213395.gif)

参考：

* [OSI 模型 - 维基本科](https://zh.wikipedia.org/wiki/OSI%E6%A8%A1%E5%9E%8B)
* [OSI 模型 - 智库](http://wiki.mbalib.com/wiki/OSI%E6%A8%A1%E5%9E%8B)
* [OSI 和 TCP/IP 模型 - 思科学院](http://icon.clnchina.com.cn/pdf/icnd1_3.pdf)

## TCP/IP 协议栈

[TCP/IP](https://tools.ietf.org/html/rfc1180)（ Transmission Control Protocol/Internet Protocol）协议栈，是一组实现支持因特网和大多数商业网络运行的协议栈的网络传输协议，这个名称来源于其中两个最重要的协议：传输控制协议（TCP）和因特网协议（IP）。

发源于美国国防部的 ARPANET 项目，由文顿·瑟夫和罗伯特·卡恩两位开发，1984年，美国国防部将 TCP/IP 作为所有计算机网络的标准，1985年，250家厂商代表参加因特网架构理事会举行的关于计算产业使用TCP/IP的工作会议，推广 TCP/IP 的商业应用，慢慢地 TCP/IP 战胜其他网络协议的方案，比如 OSI 模型，后面 TCP/IP 由互联网工程任务组 [IETF](https://zh.wikipedia.org/wiki/%E4%BA%92%E8%81%94%E7%BD%91%E5%B7%A5%E7%A8%8B%E4%BB%BB%E5%8A%A1%E7%BB%84) 负责维护。

现在，网络服务商通常不会基于 OSI 参考模型构建网络，OSI 参考模型仍然仅作为指导，互联网领域的开放标准是 TCP/IP 协议栈，

TCP/IP 将软件通信过程抽象化为四个抽象层，采取协议堆栈的方式，分别实现出不同通信协议。协议族下的各种协议，依其功能不同，被分别归属到这四个层次结构之中。

TCP/IP 四层结构：

* 应用层
* 传输层
* 互联网层
* 网络接口层

| 层 | 名称 |  单元 | 描述 | 
| :-: | :-----| :---: | -------|
| 1 | 网络接口层 | 以太网帧 | 负责数据在主机和网络之间的交换。主要协议：PPTP，ARP|
| 2 | 网络互联层 | IP 数据报 | 负责数据的包装、寻址和路由。主要协议：IP。|
| 3 | 传输层 | TCP 报文段/ UDP 数据报 | 专注于端到端通信，负责可靠性、避免拥塞，顺序。主要协议：TCP，UDP。 |
| 4 | 应用层 | 应用层数据报 | 负责应用级消息传递。主要协议：HTTP。|

TCP/IP 与 OSI 功能相比：

![tcp/ip vs osi](http://fiberbit.com.tw/wp-content/uploads/2013/12/TCP-IP-model-vs-OSI-model.png)

分层:

连结层

* [ARP](https://tools.ietf.org/html/rfc826) 地址解析协议。在IPv4 场景下，解析网路层地址(IPv4 地址)来找寻数据链路层地址（MAC 地址）。
* [NDP](https://zh.wikipedia.org/wiki/%E9%82%BB%E5%B1%85%E5%8F%91%E7%8E%B0%E5%8D%8F%E8%AE%AE) 邻居发现协议。在IPv6 场景下，负责在链路上发现其他节点和相应的地址（与ARP 类似）。
* [Tunnels]()  
* [L2TP](https://zh.wikipedia.org/wiki/%E7%AC%AC%E4%BA%8C%E5%B1%82%E9%9A%A7%E9%81%93%E5%8D%8F%E8%AE%AE) 第二层隧道协议。一种虚拟隧道协议，通常用于虚拟专用网。
* [PPP]()点对点协议。工作在数据链路层（以OSI参考模型的观点）。它通常用在两节点间创建直接的连接，并可以提供连接认证、传输加密以及压缩。

寻址，传输以太网帧；
	
网络互联层

* IP
* IPv6

传输 IP 数据包

传输层：

* TCP 
* UDP 

传输 TCP 数据包 或 UDP 数据包

应用层：

* DNS 
* DHCP 
* HTTP 
* RPC 
* TLS／SSL

使用 TCP 协议的应用层服务：

HTTP
TELNET

同时使用 TCP 或者 UDP 协议的应用层服务：

* QQ 协议
* 微信 协议
* Socks 安全套接字协议
	
独立的 RPC 远程过程调用协议

TBC: https://www.one-tab.com/page/iESqck1jTkqYJKLEO9H5_Q
	

MAC 报文

IP 报文

TCP 报文

UDP 报文

HTTP 报文

参考：

![]()

 * [ 网络通讯协议图 - 科来](http://www.colasoft.com.cn/download/network-protocol-map-2017.pdf)

## 典型协议实现

RabbitMQ 的 AMQP 协议。


网络整体描述

    七层五层

    实际开发结构图（tcp http socket nginx servlet，tomcat netty SpringMVC Java IO HttpClient gRPC）

网络协议

    tcp http  http2 https socket
    


代理

    lvs

    nginx

    tengine

    openresty

后端容器

    tomcat

    netty

框架

    spring mvc

    java io 类型

    grpc
    
    