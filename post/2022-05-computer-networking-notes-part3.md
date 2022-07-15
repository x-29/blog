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

用一台笔记本电脑浏览一个 Web 页面（例如，www.google.com），似乎很简单，但实际会是怎样的一个过程呢？

假设这台笔记本电脑通过网线连接到一台以太网交换机，交换机又与一台路由器相连。这台路由器与一个 ISP 连接，并且运行在着 DHCP 服务器。
1. **获得 IP 地址**。在这台笔记本电脑与网络连接时，它是没有 IP 地址的，也就不能访问 Web 页面。所以，首先它要运行 DHCP 协议从本地 DHCP 服务器获得一个 IP 地址以及一些其他信息。
    - 操作系统生成一个 DHCP 请求报文，并将这个报文封装在目的端口为 `67`（DHCP 服务器）和源端口为 `68`（DHCP 客户）的一个 UDP 报文段中，该 UDP 报文段则被放置在一个具有广播 IP 目的地址（255.255.255.255）和源 IP 地址 0.0.0.0（因为这台电脑还没有 IP 地址）的 IP 数据报中。该 IP 数据报则被放置在以太网帧中，帧的目的 MAC 地址是广播 MAC 地址`FF:FF:FF:FF:FF:FF`。
    - 包含 DHCP 请求的广播以太网帧是第一个由这台电脑发送到以太网交换机的帧。该交换机在所有的出端口广播入帧，包括连接到路由器的端口。
    - 路由器接收到该广播以太网帧，从帧抽取出 IP 数据报，该数据报的广播 IP 目的地址指示了这个 IP 数据报应当由在该节点的高层协议处理，因此该数据报的载荷（ 一个 UDP 报文段）被分解（demuxed）向上到达 UDP，DHCP 请求报文从此 UDP 报文段中抽取出来。此时 DHCP 服务器有了 DHCP 请求报文。
    - DHCP 服务器生成 DHCP ACK，其中包含了分配给这台电脑的 IP 地址、默认网关路由器（第一跳路由器）的 IP 地址、DNS 服务器的名称 和 IP 地址。该 DHCP ACK 被放入一个 UDP 报文段中， UDP 报文段被放人一个 IP 数据报中， TP 数据报再被放入一个以太网帧中。该以太网帧的目的 MAC 地址，是这台笔记本电脑的 MAC 地址。
    - 包含 DHCP ACK 的以太网帧由路由器发送给交换机。因为交换机是自学习的，并且先前从 Bob 便携机收到（包含 DHCP 请求的）以太网帧，所以该交换机 知道寻址到 00: 16: 03: 23: 68: 8A 的帧仅从通向 Bob 便携机的输出端口转发 。


