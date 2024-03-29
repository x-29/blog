---
title: "深入理解计算机系统(csapp)笔记-存储器层次结构"
description: "这是深入理解计算机系统(Computer Systems A Programmer's Perspective 3rd)第六章的学习笔记."
date: 2022-05-17T15:22:10+08:10
draft: false
math: true
categories:
  - 读书笔记
tags:
  - cs
---

存储器系统（memory system）是一个具有不同容量、成本和访问时间的存储设备的层次结构。

## 局部性

编写良好的计算机程序常常具有良好的局部性（locality）。也就是，程序倾向于使用地址接近或等于它们最近使用过的地址的数据和指令。有良好局部性的程序比局部性差的程序运行得更快。

局部性通常有两种不同的形式：
- 时间局部性（temporal locality）。最近引用过一次的内存位置很可能在不远的将来再被多次引用。
- 空间局部性（spatial locality）。一个内存位置被引用了一次，那么程序很可能在不远的将来引用它相邻的内存位置。

现代计算机系统的各个层次，从硬件到操作系统、再到应用程序，它们的设计都利用了局部性。在硬件层，局部性原理允许计算机设计者通过引入小而快速的「高速缓存存储器」来保存最近被引用的指令和数据项，从而提供对主存的访问速度。

一个有良好局部性的函数：

```C
int sumvec(int a[n]) {
    int sum = 0;
    for (i = 0; i < n; i++) 
        sum += a[i];
    return sum;
}
```

数据引用上，每次迭代都会重复引用 sum，所以有很好的时间局部性，同时以顺序引用模式（也称为步长为 1 的引用模式，stride-1 reference pattern）访问数组 a 的元素，因此有良好的空间局部性。

取指令上，for 循环体里的指令是按照连续的内存顺序执行的，因此循环有良好的空间局部性。又因为循环体会被执行多次，所以它也有很好的时间局部性。

一个空间局部性很差的函数：
```C
int sum_array_cols(int a[M][N]) {
    int i, j, sum = 0;

    for (j = 0; j < N; j++)
        for (i = 0; i < M; i++)
            sum += a[i][j];
    return sum;
}
```
C 数组在内存中是按照「行优先」顺序存放的，上面的函数是按照「列」顺序来扫描数组，而不是按照「行」顺序，结果就得到了步长为 N 的引用模式。步长的增大，会使得空间局部性下降。

只需要交换 i 和 j 的循环，使得嵌套循环按照「行优先顺序」读取数组元素

```
for (i = 0; i < M; i++)
    for (j = 0; j < N; j++)
        sum += a[i][j];
```

结果就得到一个很好的步长为 1 的引用模式，具有良好的空间局部性。

量化评价程序中局部性的一些简单原则：
- 重复引用相同变量的程序会有良好的时间局部性。
- 对于具有步长为 k 的引用模式的程序，步长越小，空间局部性越好。
    - 具有步长为 1 的引用模式的程序有很好的局部性。
    - 在内存中以大步长跳来跳去的程序空间局部性会很差。
- 对于取指令来说，循环有好的时间和空间局部性。循环体越小，循环迭代次数越多，局部性越好。

## 存储器层次结构

计算机硬件和软件系统有这样一些基本的和持久的特性：
- 速度较快的存储技术每个字节的成本总是更高，而且容量更小。
- 编写良好的程序往往具有良好的局部性。

这些属性完美的互相补充，使得可以用「存储器层次结构」的方式来组织内存和存储系统。

{{<figure width="600" src="/images/memory-hierarchy.jpg" caption="存储器层次结构">}}

存储器层次结构的基本思想是：**对于每个 k，位于 k 层的更快更小的存储设备作为位于 k+1 层更大更慢的存储设备的缓存**。也就是说，层次结构中的每一层都是缓存（caching）来自较低一层的数据对象。例如，本地磁盘作为通过网络从远程磁盘读取的文件的缓存，主存作为本地磁盘上数据的缓存，依次类推，直到最小的缓存 \-\- CPU 寄存器组。

### 缓存的机制

使用高速缓存（cache）的过程称为缓存（caching），缓存的机制：

{{<figure width="600" src="/images/basic-principle-of-caching.jpg" caption="存储器层次结构中缓存的基本原理">}}

- 第 k+1 层的存储器被划分成连续的数据对象组块（chunk），称为块（block）。
    - 每个块都有一个唯一的地址或名字。
    - 块可以是固定大小的，也可以是可变大小的。
- 第 k 层的存储器被划分成较少的块的集合，每个块的大小与 k+1 层的块的大小一样。
    - 在任何时刻，第 k 层的缓存包含第 k+1 层块的一个字集的副本。
- 数据总是以块大小为「传送单元」（transfer unit），在第 k 层和第 k+1 层之间来回复制。

### 缓存命中和不命中

当程序需要第 k+1 层的某个数据对象 d 时，它首先在第 k 层的块中查找，如果 d 刚好缓存在第 k 层中，那么就是「缓存命中」（cache hit），程序直接从第 k 层中读取 d。

如果第 k 层没有缓存数据对象 d，那么就是「缓存不命中」（cache miss），这时，第 k 层就要从第 k+1 层中取出包含 d 的那个块。
- 如果第 k 层已经满了，就要覆盖现存的一个块。
- 覆盖现存的块的过程称为替换（replacing）或驱逐（evicting）这个块。被驱逐的这个块称为牺牲块（victim block）。
- 决定该替换哪个块是由缓存的「替换策略」（replacement plocy）来控制的。
    - 比如，随机替换策略会随机选一个牺牲块。LRU 替换策略会选择最近最少被使用的那个块。

缓存不命中的种类：
- 冷不命中（cold miss)。发生在第一次访问块时。如果第 k 层的缓存是空的，那么对任何数据对象的访问都会不命中。
- 冲突不命中（conflict miss）。只要发生不命中，第 k 层就会执行放置策略（placement policy），放置策略可能会把下一次正好需要的数据块给替换（或驱逐）了，这样就会引起冲突不命中。
- 容量不命中（capacity miss）。当工作集的大小超过缓存的大小时，缓存就会经历容量不命中。

### 为什么存储器层次结构有效？

由于局部性，程序访问层级 k 的数据的频率往往高于访问层级 k+1 的数据的频率，因此，层级 k+1 的存储可以较慢，并且因此可以更大、更便宜。

> 存储器层次结构创建了一个大的存储池，其成本与底部附近的廉价存储器一样高，但它以顶部附近的快速存储器的速率向程序提供数据

## 高速缓存存储器（cache）

早期计算机系统的存储器层次结构只有三层：CPU 寄存器、DRAM 主存储器和磁盘存储。由于 CPU 和主存之间逐渐增大的差距，计算机系统设计者在 CPU 寄存器和主存之间插入了高速缓存存储器，最先是插入了一个小的 SRAM 高速缓存存储器，称为 L1 高速缓存（一级缓存），它的访问速度几乎和寄存器一样快（大约 4 个时钟周期）。随着 CPU 和主存之间的性能差距不断增大，系统设计者又在 L1 高速缓存和主存之间插入了一个更大的高速缓存，称为 L2 高速缓存（访问速度大约 10 个时钟周期）。像 Intel Core i7 这样的现代系统，在 L2 和 主存之间还插入了一个更大的 L3 高速缓存（访问速度大约 50 个时钟周期）。

{{<figure width="600" src="/images/intel-core-i7-cache-hierarchy.jpg" caption="Intel Core i7 的高速缓存层次结构">}}

从 Intel Core i7 处理器的高速缓存层次结构可以知道，现代处理器的高速缓存既保存数据（称为 d-cache），也保存指令（称为 i-cache），而且它们是独立的。即保存指令又保存数据的高速缓存称为统一的高速缓存（unified cache）。

### 通用的高速缓存存储器组织结构

前面有讲过，存储器之间的数据总是以「块大小」（block size）作为传送单元的：
- 块的大小以字节为单位，始终是 2 的幂。比如，64 Byte。$64B=2^6$=$log_2(64)$。
- 块由相邻字节组成（地址相差 1)。

一个计算机系统，它的每个存储器地址有 $m$ 位，也就有 $M=2^m$ 个不同的地址。这样一个机器的高速缓存被组织成一个有 $S=2^s$ 个「高速缓存组（cache set）」的数组。每个组包含 $E$ 个「高速缓存行（cache line）」。每个高速缓存行包含一个 $B=2^b$ 字节的数据「块（block）」、一个「有效位」，以及 $t=m-(b+s)$ 个「标记位」。

{{<figure width="600" src="/images/general-cache-organization.jpg" caption="高速缓存的通用组织">}}

高速缓存的结构一般用元组 $(S,E,B,m)$ 来描述。高速缓存的大小（或容量）$C$ 指的是所有数据块（block）的大小之和。标记位和有效位不包括在内。因此，$C=S \times E \times B$。即，容量=组 x 行 x 数据块大小。

### 缓存读取

高速缓存的结构（参数 $S$ 和 $B$）将 $m$ 个「地址位」划分成了三个字段： $t$ 个「标记位」，$s$ 个「组索引位」和 $b$ 个「块偏移位」。当一条加载指令指示 CPU 从主存地址 $A$ 中读取一个字时，高速缓存的这种结构使得它能通过简单的检查地址位，找到所要请求的字。
- 首先，通过 A 地址的「组索引位」定位到字存放在哪个高速缓存组中。
- 然后，检查组中是否有任何行的标记位与地址 $A$ 中的「标记位」相匹配。
- 如果有匹配的行，并且该行设置了有效位，那么就认为组中的这一行包含这个字，也就是命中了缓存。
- 一旦命中，也就是知道了是哪个行包含了所请求的字，就能通过 $A$ 地址的「块偏移量位」定位到数据的起始位置。
- 最后，取出字。

举个例子，假设有一个直接映射高速缓存（每个组只有一行的高速缓存，即 $E=1$），描述为 $(S,E,B,m)=(4,1,4,8)$，现在 CPU 要从内存地址 $[01110110]$ 读取数据。

{{<figure width="600" src="/images/example-direct-mapped-cache-0.jpg">}}

高速缓存的描述表明了这个高速缓存有 4 个组，每个组一行，每个块为 4 个字节，地址是 8 位的（共有 256 个内存地址）。高速缓存的这种结构，会把地址 $[01110110]$ 划分成这样的结构：

```
 0111    01    10
------  ----  ----
  t      s     b
```
其中，$s=log_2S$，即 $s=log_24=2$，也就是地址位中有 2 位划分为组索引位。$b=log_2B$，即 $b=log_24=2$，地址位中有 2 位划分为块偏移位。$t=m-(s+b)=8-(2+2)=4$，标记位占 4 位。

{{<figure width="600" src="/images/example-direct-mapped-cache.jpg">}}

高速缓存确定一个请求是否命中，然后抽取被请求的字的过程，分为三步：

1）组选择：$s=[01]_2=1$。确定字存放在组索引号为 1 的组中。

2）行匹配：由于 $E=1$，只有一行，就直接选择这一行。

3）字抽取：如果行的有效位为 1，并且标记位匹配 `0111`，那么就命中，从偏移量 $b=[10]_2=2$ 开始读取字。如果行的有效位为 0，那么高速缓存要从主存（或低一层的高速缓存）中取出字，并把这个字存储在组 1 中。然后，高速缓存返回新取的高速缓存行的块。

---

由于直接映射高速缓存中每个组只有一行，因此存在「冲突不命中」的问题。「组相联高速缓存」（set associative cache）的每个组保存有多个高速缓存行（即，$1<E<C/B$)，从而减少了这种不命中。
- 组相联高速缓存中的组选择与直接缓存映射的组选择一样，组索引位标识了组。
- 行匹配和字选择，需要检查多个行的标记位和有效位，以确定所请求的字是否在其中。
    - 高速缓存必需搜索组中的每一行，寻找一个有效的行，且标记与地址中的标记相匹配。
    - 如果找到，那么就命中，根据块偏移读取字。
    - 如果没有命中，高速缓存必需从内存中取出包含这个字的块。一旦高速缓存取出了这个块，就要考虑该怎么存放，如果有一个空行，那就直接放在这个空行，否则就要根据替换策略，替换现有的行。

---

「全相联高速缓存（fully associative cache）」只有一个组，包含了所有高速缓存行。即 $E=C/B$。
- 全相联高速缓存中没有组的选择。因为只有一个组，所以地址中没有组索引位，地址只被划分成了标记位和块偏移位。
- 行匹配和字选择与组相联高速缓存中的一样。

### 缓存写入

缓存写入比读取要复杂一些。写入有两种情况需要处理：写命中时该怎么办？写不命中时该怎办？

假如要写的字已经在高速缓存中缓存了，也就是写命中了。那么，在更新高速缓存之后，怎么更新低一层中的副本呢？
- 最简单的方法是「直写（write-through）」，就是立即将高速缓存块写入到低一层中。缺点是每次写都会引起总线流量。
- 另一种方法是「写回（write-back）」，延迟写入，直到替换算法要驱逐这个缓存行时，才写入到低一层中。缺点是必须为每个高速缓存行维护一个额外的脏位（dirty bit），以跟踪是否被修改。

假如要写的字不在高速缓存中，也就是写不命中，这时也有两种处理方法：
- 写分配（write-allocate）或者 fetch on write，加载相应的低一层中的块到高速缓存中，然后更新高速缓存中的行。缺点是每次不命中都会导致一个块从低一层传送到高速缓存。
- 非写分配（not-write-allocate）或 write around，直接写到低一层中。

> 直写通常是非写分配的，写回通常是写分配的。对于程序员来说，在心里采用一个使用写回和写分配的高速缓存的模型是有帮助的。

> 一行总是存储一个块，说到高速缓存的“行大小”时，实际上指的是块大小

## 编写高速缓存友好的代码

局部性好的程序有较高的命中率，命中率高的程序运行的就更快。编写高速缓存友好的代码，就要在程序中利用局部性。
- 关注函数内部循环代码，大部分计算和内存访问都发生在这里。
- 使工作集保持在合理的小范围内，一旦从存储器中读入了一个数据对象，就尽可能多地使用它（时间局部性）
- 使用小步长（空间局部性）

(本章完)
