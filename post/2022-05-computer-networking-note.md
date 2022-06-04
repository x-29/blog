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

![结点时延](/images/img_202.jpg)

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

> 结点时延 = 结点处理时延 + 排队时延 + 传输时延 + 传播时延。

### 因特网的协议栈

因特网的协议栈由 5 个层次组成：物理层(Physical)、链路层(Link)、网络层(Network)、运输层(Transport)和应用层(Application)。

- 应用层包括许多协议，例如 HTTP, IMAP, SMTP, DNS，它们分布在多个端系统上。一个端系统中的应用程序使用应用层协议与另一个端系统中的应用程序交换信息的分组。这种位于应用层的信息分组称为报文 (message)。
- 传输层负责在端系统中的应用程序之间传输应用层的报文 (message)，传输层的协议有两个，即 TCP 和 UDP。传输层的分组称为报文段 (segment)。
- 网络层负责将称为数据报（datagram）的网络层分组从一台主机路由到另一台主机。网络层最著名的协议是 IP 协议，所以通常也被间称为 IP 层。
- 链路层的任务是在相邻的网络元素之间传输链路层分组。包括的协议有：Ethernet, 802.11 (WiFi), PPP。链路层分组称为帧。
- 物理层的任务是将帧中的一个一个比特（bit）从一个结点移动到下一个结点。

![主机、路由器和链路层交换机，每个包含了不同的层，反映了不同的功能](/images/img_201.jpg)

如图，数据从发送端系统（source）的协议栈一路向下，然后向上和向下经过中间的链路层交换机和路由器的协议栈，进而向上到达接收端系统（distination）的协议栈。位于下层的都会对上一层的报文进行封装，附加自己的首部信息。例如，运输层收到应用层的报文，附加上自己的首部信息构成运输层报文段。运输层报文段因此封装了应用层报文。

## 二 应用层

*学习应用层协议的概念和实现方面的知识。*

### 应用层协议原理

现代网络应用程序使用两种主流的体系结构：客户/服务器体系结构（Client/Server）和对等（P2P）体系结构。

在客户/服务器体系结构中，有一台总是打开的主机称为`服务器`，它服务于称为`客户`的主机的请求。一个经典的例子是 Web 应用程序，其中总是打开的专用 Web 服务器服务于浏览器（运行在客户主机上）的请求。值得注意的是利用客户/服务器体系结构，客户相互之间不直接通信，如 Web 应用程序中的两个浏览器并不之间通信。使用客户/服务器体系结构的著名的应用程序包括 Web，FTP、Telnet 和电子邮件。

在 P2P 体系机构中，应用程序对专用服务器的依赖很少，它在间断连接的主机对之间直接通信，这些主机对被称为对等方。因为这种对等方通信不必通过专门的服务器，该体系结构被称为对等方到对等方的。使用 P2P 体系机构的流行应用包括 BT，迅雷，Skype 和 IPTV。

***

在操作系统的术语中，进行通信的实际上是进程（process）而不是程序。一个进程k以被认为是运行在端系统中的一个程序，网络应用程序由成对进程组成，这些进程通过跨越计算机网络交换报文而相互通信。例如，在 Web 应用程序中，一个客户浏览器进程与另一台 Web 服务器进程交换报文。在一个 P2P 文件共享系统中，文件从一个对等方中的进程传输到另一个对等方中的进程。

进程通过一个称为套接字（socket）的软件接口向网络发送报文和从网络接收报文。看一个类比，把进程类比于一座房子，而它的套接字可以类比于它的门。当一个进程想向位于另外一台主机上的另一个进程发送报文是，它把报文推出该门（套接字）。该发送进程假定该门到另外一侧之间有运输的基础设施，该设施将把报文传送到目的进程的门口。一旦该报文抵达目的主机，它通过接收进程的门（套接字）传递，然后接受进程对该报文进行处理。

![进程与socket](/images/processes-communicating.jpg)

如图，套接字是同一台主机内应用程序层与传输层之间的接口。由于该套接字是建立网络应用程序的可编程接口，因此套接字也称为应用程序和网络之间的 API。

***

在一台主机上运行的进程为了向另一台主机上运行的进程发送分组，它需要知道两个信息：一是接收主机的地址，二是主机上的接收进程。在因特网中，主机由一个 32 位的 IP 地址标识，主机上运行的进程用端口号标识。例如，向一台 Web 服务器（gaia.cs.umass.edu）发送 HTTP 请求，那么发送端需要知道这台 Web 服务器的 IP 地址是 128.119.245.12，接收进程的端口号是 80。

***

因特网为应用程序提供了两个运输层协议，即 TCP 和 UDP。它们为应用程序提供了不同的服务。

TCP服务包括了面向连接服务和可靠数据传输服务。当某个应用程序选择 TCP 作为运输协议时，这个应用程序就获得了 TCP 的这两种服务。

- 面向连接的服务：在应用层数据报文开始流动之前，TCP 让客户和服务器先握手，相互交换运输层控制信息，握手之后，就在两个进程的套接字之间建立起一条 TCP 连接，这条连接是全双工的，连接双方的进程可以在此连接上同时发送和接收报文。当结束报文发送时，这条连接必须要断开。
- 可靠的数据传输服务：通信进程能够依靠 TCP，无差错、按适当顺序交付所有发送的数据。当应用程序的一端将字节流传进套接字时，它能够依靠 TCP 将相同的字节流交付给接收方的套接字，而没有字节的丢失和冗余。

TCP 协议还具有拥塞控制机制，当发送方和接收方之间的网络出现拥塞时，TCP 的拥塞控制机制会抑制发送进程。

UDP 是一种不提供不必要服务的轻量级运输协议，它仅提供最小的服务。

- UDP 是无连接的，在两个进程通信前没有握手过程。
- UDP 提供一种不可靠数据传输服务，当一个进程将报文发送进 UDP 套接字时，UDP 协议不会保证报文能到达接收进程。不仅如此，到达接收进程的报文也可能是乱序到达的。

UDP 没有拥塞控制机制，发送端可以用任意速率向下层（网络层）发送数据（实际情况可能会因文中间链路的带宽限制或拥塞，端到端的吞吐量可能小于这种速率）。

***

应用层协议 (application-layer protocol) 定义了运行在不同主机上的应用程序进程如何相互传递报文。特别是定义了：

- 交换的报文类型，例如请求报文和响应报文。
- 各种报文类型的语法，如报文中的各个字段及这些字段是如何描述的。
- 字段的语义，即这些字段中包含的信息的含义。
- 一个进程何时以及如何发送报文，对报文进行响应的规则。

网络应用和应用层协议是有区别的。应用层协议只是网络应用的一部分，例如，Web 是一种客户/服务器应用，它允许客户按照需求从 Web 服务器获取文档。该 Web 应用有很多组成部分，包括文档格式的标准（即 HTML）、Web 浏览器（如 Firefox，Chrome）、Web server (如 Apache、Nginx），以及一个应用层协议。Web 的应用层协议是 HTTP，它定义了在浏览器和 Web 服务器之间传输的报文格式和序列。因此 `HTTP 协议只是 Web 应用的一个部分`（尽管是重要的部分）。

### HTTP 协议

Web 的应用层协议是**超文本传输协议**（HyperText Transfer Protocol, HTTP），它是 Web 的核心，在 [RFC 1945] 和 [RFC 2616] 中定义。

HTTP 由两个程序实现：一个是客户程序（称为 **Web 客户**，如 Firefox、Chrome）和一个服务器程序（称为 **Web 服务器**， 如 Apache、Nginx）。客户程序和服务器程序运行在不同的主机上，通过交换 HTTP 报文进行会话。HTTP 定义了这些报文的结构以及客户和服务器进行报文交换的方式。

一些 Web 术语：

> Web 页面，也叫文档，是由对象组成的。

> 一个对象（object) 只是一个文件，如一个 HTML 文件、一个 JPEG 图形文件，它们可以通过一个 URL 地址定位到。

> 每个 URL 地址有两部分组成：存放对象的服务器主机名和对象的路径名。例如：URL 地址 http://www.someSchoool.edu/someDepartment/picture.gif，其中的 www.someSchool.edu 是主机名，/someDepartment/picture.gif 是路径名。

![HTTP 的请求-响应行为](/images/http-request-response.jpg)

客户和服务器的交换过程，如图所示。当用户请求一个 Web 页面（如点击一个超链接）时，浏览器向服务器发出对该页面中所包含对象的 HTTP请求报文，服务器接收到请求并用包含这些对象的 HTTP 响应报文进行响应。

HTTP 使用 TCP 作为它的支撑运输协议。HTTP 客户首先发起一个与服务器的 TCP 连接。一旦连接建立，该浏览器和服务器进程就可以通过套接字接口访问 TCP。客户端的套接字是客户进程与 TCP 连接之间的门，在服务器端的套接字接口则是服务器进程与 TCP 连接之间的门。客户向它的套接字接口发送 HTTP 请求报文并从它的套接字接口接收 HTTP 响应报文。同样地，服务器从它的套接字接口接收 HTTP 请求报文和向它的套接字接口发送 HTTP 响应报文。**一旦客户向它的套接字接口发送了一个请求报文，该报文就脱离了客户控制并进入 TCP 的控制**。

HTTP 服务器不保存关于客户的任何信息，所以说 HTTP 是一个无状态协议（stateless protocol）。

**往返时间**（Round-Trip Time, RTT）是指一个短分组从客户到服务，然后再返回到客户所花费的时间。RTT 包括分组传播时延、分组在中间路由器和交换机上的排队时延以及分组处理时延。

### HTTP 报文格式

HTTP 规范 [RFC 1945；RFC 2616] 包含了对 HTTP 报文格式的定义。HTTP 报文有两种：

- 请求报文
- 响应报文

一个典型的 HTTP 请求报文，如下：

```
GET /somedir/page.html HTTP/1.1
Host: www.someschool.edu
Connection: close
User-agen: Mozilla/5.0
Accept-language: fr
```

HTTP 请求报文是 ASCII 文本，第一行叫做请求行（request line），其后的行叫做首部行（header line）。

请求行有 3 个字段：
1. 方法字段。方法字段的取值包括 GET、POST、HEAD、PUT 和 DELETE。绝大部分的 HTTP 请求报文时延 GET 方法。
2. URL 字段。指定请求对象的标识。
3. HTTP 版本字段。自解释的。

请求报文的每一行都以回车+换行（\r\n）结束，行内的字段之间以空格隔开。首部行可以有多行，最后必须以一个空行（\r\n）表明首部行的结束。

{{< figure src="/images/http-request-general-format.jpg" caption="一个HTTP请求报文的通用格式" style="text-align:center">}}

从 HTTP 请求报文的通用格式上，可以看到在首部行后面还有一个实体体（entity body）。在方法为 POST 时，就会用到这个实体体字段，它包含了用户在表单字段中输入的值。**注意：实体体和首部行之间有一个空行（回车+换行）**。

一个典型的 HTTP 响应报文，如下：

```
HTTP/1.1 200 OK
Connection: close
Date: Tue, 09 Aug 2011 15:44 GMT
Server: Apache/2.2.3 (CentOS)
Last-Modified: Tue, 09 Aug 2011 15:11:03 GMT
Content-Length: 6821
Content-Type: text/html

(data data data data ....)
```