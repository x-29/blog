---
title: "计算机网络(Computer Networking)笔记" # Title of the blog post.
date: 2022-05-23T23:56:18+08:00 # Date of post creation.
description: "计算机网络(Computer Networking: A Top-Down Approach)学习笔记." # Description used for search engine.
featured: true # Sets if post is a featured post, making appear on the home page side bar.
draft: false # Sets whether to render this page. Draft of true will not be rendered.
toc: false # Controls if a table of contents should be generated for first-level links automatically.
# menu: main
usePageBundles: false # Set to true to group assets like images in the same folder as this post.
featureImage: "https://img3.doubanio.com/view/subject/s/public/s27997920.jpg" # Sets featured image on blog post.
featureImageCap: '图片来自: douban.' # Caption (optional).
# thumbnail: "https://img3.doubanio.com/view/subject/s/public/s27997920.jpg" # Sets thumbnail image appearing inside card on homepage.
# shareImage: "/images/path/share.png" # Designate a separate image for social media sharing.
codeMaxLines: 10 # Override global value for how many lines within a code block before auto-collapsing.
codeLineNumbers: false # Override global value for showing of line numbers within code block.
figurePositionShow: true # Override global value for showing the figure label.
categories:
  - 读书笔记
tags:
  - networking
# comment: false # Disable comment if false.
---

这是[《计算机网络自顶向下方法 第6版》](https://book.douban.com/subject/26176870/)一书的读书笔记。

## 一 计算机网络和因特网

*从整体上粗线条地勾画出计算机网络地概貌。*

### 因特网

因特网是一个世界范围地计算机网络，即它是一个互联了遍及全世界地数以亿计的计算机设备的网络。这些设备包括传统的桌面 PC、Linux 工作站已经现在的智能手机、平板电脑、游戏机、家用电器等，我们称之为**主机**（host）或**端系统**（end system）

* 端系统 (end system) 通过通信链路 (communication link) 和分组交换机（packet switch）连接到一起。通信链路根据物理媒体组成可以分为铜轴电缆、铜线、光纤和无线电频谱。不同的链路以不同的速率传输数据，链路的传输速率以 `bit/s` 度量（或者 bps）。
* 一台端系统要向另一台端系统发送数据时，发送端系统将数据分段，并为每段加上首部字节。由此形成的信息包称为**分组** (packet）。这些分组通过网络发送到目的端系统。
* 分组交换机从它的一条*入*通信链路接收到达的分组，并从它的一条*出*通信链路转发该分组。交换机主要有两类：`路由器（router）`和`链路层交换机（link-layer switch）`。
	* 从发送端系统到接收端系统，一个分组（packet）所经历的一系列通信链路和分组交换机称为该网络的**路径**(route 或 path)。
* 端系统、分组交换机和其他因特网部件都要运行一系列**协议**（protocol），这些协议控制因特网中信息的接收和发送。
	- e.g，TCP，IP，HTTP，Skype，802.11
* 因特网的标准由 **IETF**（Internet Engineering Task Force）研发。
	- IETF 的标准文档称为 **RFC**（Request For Comment）。

因特网也可以被看作为应用程序提供服务的基层设施，应用程序可以是 Web，Email，电子商务等。应用程序只需调用端系统提供的 API，就可通过因特网进行数据传输。

### 协议

**协议**（protocol）定义了在两个或多个通信实体之间交换的报文`格式`和`次序`，以及报文发送和/或接收一条报文或其他事件`所采取的动作`。

因特网广泛地使用了协议，不同的协议用于完成不同的通信任务。

### 因特网的部件：网络边缘、接入网和网络核心

端系统也称为主机，位于因特网的边缘。
- 主机被进一步划分为客户端（client）和服务器（server）。客服端通常是桌面 PC、移动 PC和智能手机等；而服务器是更为强大的机器，用于存储和发布 Web 页面、流视频等，如今，通常用于大型数据中心（data center）。

接入网是指端系统连接到其边缘路由器（edge router）的物理链路。
- 边缘路由器是端系统到任何其他远程端系统的路径上的第一台路由器。
- 如今，宽带住宅接入技术包括数字用户线（DSL)、电缆、光纤到户（FTTH）；企业（公司、学校）接入最流行的是以太网和 WiFi 两种接入技术，利用局域网（LAN）将端用户连接到边缘路由器；广域无线因特网接入技术采用了 3G/4G。

网络核心是由连接因特网端系统的分组交换机和链路构成的网状网络。
- 为了从源端系统向目的端系统发送一个报文，源将长报文划分成较小的数据块，称之为**分组**（packet)。在源和目的之间，每个分组都通过通信链路和分组交换机（packet switch）传送。
- 分组以等于链路最大传输速率的速度传输通过通信链路。
- 如果某源端系统或分组交换机经过一条链路发送一个 *L* bit 的分组，链路的传输速率为 *R* bit/s，那么传输该分组的时间为 *L/R* /s。

大多数分组交换机在链路的输入端使用`存储转发传输`（store-and-forward transmission）机制。这意味着交换机必须接收到整个分组，才向输出链路传输。（之前接收到的比特先缓存（即 “存储”）起来，仅当已经接收完了该分组的所有比特后，才开始向出链路传输（即 “转发”）该分组）

> 一个端到另一个端的时延 = 2*L/R*（假设传播时延为 0）。

每个分组交换机都有多条链路与之相连。对于每条相连的链路，该分组交换机具有一个输出缓存（output buffer）（也称为输出队列，用于存储路由器准备发往那条链路的分组。如果在一段时间内，链路的到达速率（以比特为单位）超过链路的传输速率：
- 分组将会在输出缓存中排队，等待在链路上传输。(排队等待就会出现排队时延）
- 如果输出缓存填满，分组可能被丢弃（丢失）。

每台路由器都有一个`转发表`，将目的地址（或目的地址的一部分）映射成输出链路。当分组到达路由器时，路由器以目的地址搜索其转发表，找到对应的输出链路，将分组导向该输出链路。端到端选路过程类似于一个不使用地图而喜欢问路的汽车司机，每到一个地点，就打听下一个地点的路线。

### 时延

当分组从一个结点（主机或路由器）沿着一条路径到达后继结点（主机或路由器），该分组在沿途的每个结点将会经受几种不同类型的时延：

- 结点处理时延（nodal processing delay）
	- 检查比特级别的差错
	- 决定将分组导向何处
	- 高速路由器的处理时延通常 < msec (微秒或更低)。
- 排队时延（queuing delay）
	- 分组在链路上等待传输的时间
	- 依赖于路由器的拥塞程度
	- 时延在毫秒到微秒级别
- 传输时延（transmission delay）
	- 路由器将分组推出所需要的时间
	- *L*：表示分组长度（bit)
	- *R*: 表示链路带宽，即从路由器 A 到路由器 B 的链路传输速率（bps)。对于一条 10Mbps 的以太网链路，速率 *R*=10Mbps；对于 100Mbps 的以太网链路，速率 *R*=100Mbps。
	- 传输时延 = *L/R*，通常在毫秒到微秒级别。
- 传播时延（propagation delay）
	- 一个比特从一台路由器向另一台路由器传播所需要的时间
	- *d*: 表示物理链路的长度，即两台路由器之间的距离
	- *s*: 表示链路的传播速率，取决于链路的物理媒体（即光纤、双绞铜线等）
	- 传播时延=*d/s*，毫秒级别。

> 结点总时延 = 结点处理时延 + 排队时延 + 传输时延 + 传播时延。

### 因特网的协议栈

因特网的协议栈由 5 个层次组成：物理层、链路层、网络层、运输层和应用层。

- 应用层包括许多协议，例如 HTTP, IMAP, SMTP, DNS，它们分布在多个端系统上。一个端系统中的应用程序使用应用层协议与另一个端系统中的应用程序交换信息的分组。这种位于应用层的信息分组称为报文 (message)。
- 传输层负责在端系统中的应用程序之间传输应用层的报文 (message)，传输层的协议有两个，即 TCP 和 UDP。传输层的分组称为报文段 (segment)。
- 网络层负责将称为数据报（datagram）的网络层分组从一台主机路由到另一台主机。网络层最著名的协议是 IP 协议，所以通常也被间称为 IP 层。
- 链路层的任务是在相邻的网络元素之间传输链路层分组。包括的协议有：Ethernet, 802.11 (WiFi), PPP。链路层分组称为帧。
- 物理层的任务是将帧中的一个一个比特（bit）从一个结点移动到下一个结点。
