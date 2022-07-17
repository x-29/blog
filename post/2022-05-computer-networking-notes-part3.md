---
title: "计算机网络(Computer Networking)笔记(三)"
description: "计算机网络(Computer Networking: A Top-Down Approach)学习笔记第三部分，Web 页面请求的历程." # Description used for search engine.
date: 2022-05-25T09:58:10+08:00
draft: false
categories:
  - 读书笔记
tags:
  - networking
---

这是[《计算机网络自顶向下方法 第6版》](https://book.douban.com/subject/26176870/)一书的读书笔记的第三部分。

[第一部分](../2022-05-computer-networking-notes-part1)

[第二部分](../2022-05-computer-networking-notes-part2)

## Web 页面请求的历程

我们打开一台笔记本电脑浏览一个 Web 页面（例如，www.google.com），似乎很简单，但实际会是怎样的一个过程呢？

{{<figure width="600" src="/images/web-page-request.jpg" caption="Web 页请求的历程：网络环境和动作">}}

假设这台笔记本电脑通过网线连接到一台以太网交换机，交换机又与一台路由器相连。这台路由器与一个 ISP 连接，并且运行着 DHCP 服务器。

**1. 准备：DHCP、UDP、IP和以太网**。

在这台笔记本电脑与网络连接时，它还没有 IP 地址，所以访问不了 Web 页面。因此，它首先要运行 DHCP 协议从本地 DHCP 服务器获得一个 IP 地址以及一些其他信息。

  {{<figure width="300" src="/images/connecting-to-the-internet-step-1.jpg" caption="主机发送 DHCP 请求报文">}}

  - 操作系统生成一个 DHCP 请求报文，并将这个报文封装在目的端口为 `67`（DHCP 服务器）和源端口为 `68`（DHCP 客户）的一个 UDP 报文段中，该 UDP 报文段则被放置在一个具有广播 IP 目的地址（255.255.255.255）和源 IP 地址 0.0.0.0（因为这台电脑还没有 IP 地址）的 IP 数据报中。该 IP 数据报再被放入到一个以太网帧中，该帧以广播地址`FF:FF:FF:FF:FF:FF` 为目的 MAC 地址，发送到以太网交换机。这是第一个由这台电脑发送到以太网交换机的帧，交换机在它所有的出端口广播入帧，包括连接到路由器的端口。
  - 路由器接收到该广播以太网帧，从帧抽取出 IP 数据报，该数据报的广播 IP 目的地址指示了这个 IP 数据报应当由在该节点的高层协议处理，因此该数据报的载荷（一个 UDP 报文段）被分解（demuxed）向上到达 UDP，DHCP 请求报文从此 UDP 报文段中抽取出来。此时 DHCP 服务器（运行在路由器中）有了 DHCP 请求报文。

  {{<figure width="300" src="/images/connecting-to-the-internet-step-2.jpg" caption="路由器回复 DHCP ACK 报文">}}

  - DHCP 服务器生成 DHCP ACK，其中包含了分配给这台电脑的 IP 地址、默认网关路由器（第一跳路由器）的 IP 地址、DNS 服务器的名称 和 IP 地址。该 DHCP ACK 被放入一个 UDP 报文段中， UDP 报文段被放人一个 IP 数据报中， TP 数据报再被放入一个以太网帧中。该以太网帧的目的 MAC 地址，就是这台电脑的 MAC 地址。
  - 包含 DHCP ACK 的以太网帧由路由器发送给交换机。因为交换机是自学习的，并且先前已经收到过这台电脑发送的以太网帧，交换机表中已有对应表项，所以该交换机知道向通向这台电脑的输出端口转发该帧。
  - 这台电脑收到包含 DHCP ACK 的以太网帧，从该以太网帧中抽取 IP 数据报，从 IP 数据报中抽取 UDP 报文段，从 UDP 报文段抽取 DHCP ACK 报文。DHCP 客户则记录下它的 IP 地址和它的 DNS 服务器的 IP 地址。它还在其 IP 转发表中安装默认网关的地址。

至此，我们这台电脑已经有了一个 IP 地址，也知道了网关路由器的地址、 DNS 服务器的名称和地址。现在，准备开始处理 Web 页面的获取。

**2. 仍在准备：DNS 和 ARP**

在 Web 浏览器键入网址 www.google.com 时，Web 浏览器将会生成一个 TCP 套接字，这个套接字用于向 www.google.com 发送 HTTP 请求。为了能生成这个套接字，需要先通过 DNS 协议获得 www.google.com 的 IP 地址。
- 操作系统生成一个 DNS 查询报文，字符串 "www.googel.com" 被放入到 DNS 查询报文的问题段中。该 DNS 报文则放置在目的端口为 53 (DNS 服务器）的 UDP 报文段中。UDP 报文段则被放入目的地址为 DHCP ACK 返回的 DNS 服务器地址（例如，68.87.71.226），源地址为 68.85.2.101（DHCP ACK 返回的 IP 地址） 的 IP 数据报中。
- 包含 DNS 请求报文的 IP 数据报最终放入在一个以太网帧中，并发送到网关路由器。为了能把帧发送到网关路由器，需要知道路由器的 MAC 地址。虽然，DHCP ACK 告知了网关路由器的 IP 地址（例如，68.85.2.1），但还不知道路由器的 MAC 地址。因此，要使用 ARP 协议来获得。

{{<figure width="300" src="/images/arp-query.jpg" caption="发送 ARP 查询报文">}}

- ARP 模块生成一个 ARP 查询报文，报文的目的 IP 地址为 68.85.2.1（路由器的地址）。该 ARP 查询报文被放置在一个以太网帧中，帧的目的 MAC 地址为广播地址（FF:FF:FF:FF:FF:FF）。帧被发送到交换机后，交换机将它广播给所有连接的设备，包括网关路由器。
- 网关路由器接收到包含 ARP 查询报文的帧，发现 ARP 查询报文中目标 IP 地址（68.85.2.1）与其接口的 IP 地址匹配。于是，准备一个 ARP 回答报文，报文包含其 MAC 地址。该 ARP 回答报文放置在一个以太网帧中，帧的目的地址为我们的这台电脑的 MAC 地址。帧被发送到交换机，再由交换机将帧传递到这台电脑。
- 我们的这台电脑接收到包含 ARP 回答报文的帧，并从 ARP 回答报文中抽取网关路由器的 MAC 地址。
- 现在，有了网络路由器的 MAC 地址，就能够把包含 DNS 查询报文的以太网帧发送到网关路由器。

**3. 仍在准备：域内路由选择到 DNS 服务器**

- 网关路由器接收到包含 DNS 查询报文的以太网帧，抽取出 IP 数据报，查找数据报的目的地址（68.87.71.226)，并根据路由器转发表决定该数据报应该发送到 Comcast 域的网关路由器（图中最左边路由器，子网 68.80.0.0/13）。
- 在 Comcast 域中的网关路由器接收到帧，抽取出 IP 数据报，检查数据报中的目的地址（68.87.71.226），并根据路由器转发表确定出接口，经过该出接口转发数据报到 DNS 服务器。转发表是根据 Comcast 的域内协议（如 RIP、OSPF 或 IS-IS）以及因特网的域间协议 BGP 所产生。
- 在包含 DNS 查询的 IP 数据报到达 DNS 服务器后，DNS 服务器抽取出 DNS 查询报文，在它的 DNS 数据库中查找名字 www.google.com，找到包含对应 www.google.com 的 IP 地址（64.233.169.105）的 DNS 源记录（假设它当前缓存在 DNS 服务器中，这种缓存数据源于 google.com 的权威 DNS 服务器），并形成一个包含主机名到 IP 地址映射的 DNS 回答报文。该 DNS 回答报文在经过 UDP 报文段 -> IP 数据报 -> 以太网帧的封装后，发送到我们的这台电脑。
- 我们的这台电脑从 DNS 回答报文中抽取出服务器 www.google.com 的 IP 地址。

**4. Web 客户-服务器交互：TCP 和 HTTP**

在有了 www.google.com 的 IP 地址后，就能够生成 TCP 套接字，该套接字将用于向 www.google.com 发送 HTTP GET 报文。
- 在生成 TCP 套接字时，我们的这台电脑中的 TCP 必须先与 www.google.com 中的 TCP 完成三次握手。
  - 我们的这台电脑首先生成一个具有目的端口 80 的 TCP SYN 报文段，将该 TCP 报文段放置在目的 IP 地址为 64.233.169.105（www.google.com） 的 IP 数据报中，将该数据报放置在 MAC 地址为网关路由器的 MAC 地址的帧中，并发送该帧到交换机。
  - 包含 TCP SYN 的数据报到达 www.googel.com 后，www.google.com 产生一个 TCP SYNACK 报文段，发送给我们的这台电脑，从而进入连接状态。
  


