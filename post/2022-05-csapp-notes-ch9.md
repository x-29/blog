---
title: "深入理解计算机系统(csapp)笔记-虚拟内存"
description: "这是深入理解计算机系统(Computer Systems A Programmer's Perspective 3rd)第六章的学习笔记."
date: 2022-05-19T21:02:10+08:10
draft: false
math: true
categories:
  - 读书笔记
tags:
  - cs
---

现代系统提供了一种对主存的抽象概念，叫做「虚拟内存（VM）」。虚拟内存提供了三个重要的能力：
- 它将主存看成是一个存储在磁盘上的地址空间的高速缓存，这相当于在主存中的是活动区域，然后根据需要从磁盘读取数据到主存或将主存数据写回到磁盘。
- 它为每个进程提供了一致的地址空间。
- 它保护了每个进程的地址空间不被其他进程破坏。

## 物理和虚拟寻址

计算机系统的主存被组织成一个 $M$ 个连续的字节大小的单元组成的数组。每个字节都有一个唯一的「物理地址（Physical Address，PA）。第一个字节的地址为 0，接下来的字节地址为 1，再下一个为 2，依次类推。


CPU 使用物理地址访问内存的方式称为物理寻址（physical addressing），这种寻址方式通常在单进程的简单系统中使用。比如，嵌入式微控制器、数字信号处理器。

{{<figure width="500" src="/images/physical-addressing.jpg" caption="物理寻址">}}

然而，**现代处理器使用「虚拟寻址（virtual addressing）」方式**。

{{<figure width="600" src="/images/virtual-addressing.jpg" caption="虚拟寻址">}}

使用虚拟寻址，CPU 通过生成一个「虚拟地址（Virtual Address，VA）」来访问主存。这个虚拟地址在被送到主存之前先要经过「地址翻译（address translation）」转换成物理地址。地址翻译需要 CPU 硬件和操作系统相互配合，CPU 芯片上叫做「内存管理单元（Memory Management Unit，MMU）」的专用硬件，利用存放在主存中的查询表（页表）来动态翻译虚拟地址，查询表的内容由操作系统维护。

## 地址空间

地址空间（address space）是一个非负整数地址的有序集合。

在一个带有虚拟内存的系统中，CPU 从一个有 $N=2^n$ 个地址的地址空间中生成虚拟地址，这个地址空间称为「虚拟地址空间（virtual address space)」。一个地址空间的大小是由表示最大地址所需要的「位」数来描述的。例如，一个包含 $N=2^n$ 个地址的虚拟地址空间就叫做一个 $n$ 位地址空间。现代系统通常支持 32 位或者 64 位虚拟地址空间。

## 虚拟内存

概念上而言，将虚拟内存视为存储在磁盘上的 $N=2^n$ 个连续字节的数组。每个字节都有一个唯一的虚拟地址，作为到数组的索引。VM 系统将虚拟内存分割为虚拟页面（Virtual Page, VP），以虚拟页面作为传输单元，每个虚拟页面的大小为 $P=2^p$ 字节。同样地，物理内存被分割为多个物理页面（Physical Page, PP），物理页面的大小也为 $P$ 字节。
- 虚拟页面往往很大，通常是 4KB～2MB。
- 虚拟内存页面通常缓存在物理内存中。
  - 每个虚拟页面都可以放置在任何的物理页面中，不会有内存碎片

{{<figure width="600" src="/images/vm-as-a-tool-for-caching.jpg" caption="一个 VM 系统如何使用主存作为缓存的">}}

---

MMU（内存管理单元）中的地址翻译硬件在将一个虚拟地址转换为物理地址时，都会读取「页表（page table）」，页表是一种数据结构，它存放在物理内存中，包含了虚拟页面到物理页面的映射关系。页表由操作系统负责维护。

{{<figure width="600" src="/images/page-table.jpg" caption="页表">}}

页表是一个页表条目（Page Table Entry，PTE）的数组。虚拟地址空间中的每个页在页表中一个固定偏移量处都有一个 PTE。
- PTE 由一个「有效位」和一个 $n$ 位地址字段组成。
- 有效位表明虚拟页面当前是否被缓存在主存中。
  - 如果设置了有效位（为 1），那么地址字段就表示物理内存中的物理页面的起始位置，这个物理页面中缓存了该虚拟页面。
  - 如果没有设置有效位（为 0），地址字段为空就表示这个虚拟页面还未被分配（比如，PTE 5)，否则，这个地址指向该虚拟页面在磁盘上的起始位置（比如，PTE 3、PTE 6）。

假设，每个虚拟页面的大小是 4K（$2^{12}$），虚拟地址的位数为 16 位，那么需要多少个页表条目呢？

计算：$2^n/2^p=2^{n-p}=2^{16-12}=16$，系统中总共有 16 个页面，其中每个页面都需要一个页表条目，所以需要 16 个 PTE。

---

在虚拟内存的习惯说法中，DRAM 缓存不命中称为「缺页（page fault）」。地址翻译硬件从页表中的页表条目的有效位推断出虚拟页面未被缓存时，就会触发一个缺页异常。缺页异常调用内核中的缺页异常处理程序，该程序会选择一个牺牲页面，如果被选中的牺牲页面已经被修改了，那么内核就会将它复制到磁盘，然后内核从磁盘复制请求的页面到内存中，并更新 PTE，随后返回。当异常处理程序返回时，它会重新执行导致缺页的指令，该指令会把导致缺页的虚拟地址重新发送到地址翻译硬件。

---

操作系统为每个进程提供了一个独立的页表，因而也就是每个进程都有一个独立的虚拟地址空间。

{{<figure width="600" src="/images/virtual-address-space-for-process.jpg" caption="VM 如何为进程提供独立的地址空间">}}

从图中还可以看到，多个虚拟页面可以映射到同一个共享到物理页面上。

VM 简化了链接和加载、代码和数据共享，以及应用程序的内存分配。
- 简化链接
  - 每个程序都有相似的虚拟地址空间，这使得代码、数据，以及堆始终从相同的地址开始。比如，对于 64 位地址空间，代码段总是从虚拟地址 0x400000 开始。数据段跟在代码段之后，中间是满足对齐要求的空白。栈在用户进程地址空间最高的部分，并向下生长。
- 简化加载
  - 要把目标文件中 .text 和 .data 段加载到一个新创建的进程中，Linux 加载器为代码和数据段分配虚拟页面，并创建标记为无效的 PTE。
  - 根据虚拟内存系统的要求，逐页复制 .text 和 .data 部分。
- 简化共享
  - 将不同地址空间中的虚拟页面映射到同一个物理页面（例如，图中的 PP 6）
- 简化内存分配
  - 在一个运行在用户进程中的程序需要额外的堆空间时（如调用 malloc），操作系统分配 $k$ 个连续的虚拟内存页面，并将它们映射到物理内存中任意位置的 $k$ 个任意的物理页面（不需要连续，可以随机分散）。

---

VM 实现可读、可写和可运行的内存访问控制。
- 扩展页表条目，增加一个权限位。
- MMU 在每次内存访问时检查这些权限位。
  - 如果一个指令违反了权限控制，那么 CPU 就触发一个异常。

{{<figure width="600" src="/images/page-level-memory-protection.jpg" caption="用虚拟内存来提供页面级的内存保护">}}

例如，进程 i 有读 VP 0，读和执行 VP1，读写 VP2 的权限。

## 地址翻译

形式上来说，地址翻译是一个 $N$ 元素的虚拟地址空间（VAS）中的元素和一个 $M$ 元素的物理地址空间（PAS）中元素之间的映射。

MMU 利用页表来实现这种映射。在 CPU 中有一个控制寄存器叫「页表基地址寄存器（Page Table Base Register，PTBR），它指向当前页表。$n$ 位的虚拟地址分成两个部分：一个 $p$ 位的虚拟页面偏移（Virtual Page Offset，VPO）和一个 ($n-p$) 位的虚拟页号（Virtual Page Number，VPN）。

{{<figure width="600" src="/images/address-translation.jpg" caption="使用页表的地址翻译">}}

MMU 利用 VPN 来选择适当的 PTE。例如，VPN 0 选择 PTE 0，VPN 1 选择 PTE 1，以此类推。将页表条目中「物理页号（Physical Page Number，PPN）和虚拟地址中的 VPO 串联起来，就得到相应的物理地址。由于物理和虚拟页面都是 $P$ 字节的，所以「物理页面偏移（Physical Page Offset，PPO）和 VPO 是相同的。


下图展示了页面命中时，CPU 硬件执行的步骤。

{{<figure width="600" src="/images/page-hit.jpg" caption="页命中">}}

1）CPU 生成一个虚拟地址，并把它发送给 MMU。2-3）MMU 生成 PTE 地址，并从高速缓存/主存中得到 PTE。4）MMU 构造物理地址，并把它传送给高速缓存/主存。5）高速缓存/主存将请求的数据传送给 CPU。

页面命中完全是由硬件来处理的，与之不同的是，处理缺页需要硬件和操作系统内核协作完成，如图：

{{<figure width="600" src="/images/page-fault.jpg" caption="缺页">}}

1）CPU 生成一个虚拟地址，并把它发送给 MMU。2-3）MMU 生成 PTE 地址，并从高速缓存/主存中得到 PTE。4）PTE 中的有效位为 0，因此 MMU 触发缺页异常。5）缺页处理程序确定出物理内存中的牺牲页，如果这个页面已经被修改，那么就把它换出（Page out）到磁盘。6）缺页处理程序调入（Page in）新的页面，并更新内存中的 PTE。7）缺页处理程序返回到原来的进程，再次指向导致缺页的指令。

### 利用 TLB 加速地址翻译

每次 CPU 产生一个虚拟地址，MMU 就要访问内存两次，一次是获取用于地址翻译的 PTE，再一次是实际的内存请求。即便 PTE 像其他存储器字一样缓存在高速缓存 L1 中，它们也可能会被其他数据引用逐出缓存，并且 L1 缓存中的命中仍然需要 1-3 个时钟周期。许多系统为消除这种开销，它们在 MMU 中包括了一个「翻译后备缓冲器（Translation Lookaside Buffer，TLB）」。

TLB 是一个小的、虚拟寻址的缓存，其中每一「行」都保存着一个由单个 PTE 组成的「块」。TLB 有高度的相联度。

{{<figure width="600" src="/images/TLB.jpg" caption="虚拟地址中用以访问 TLB 的组成部分">}}

虚拟地址中的 VPN 被拆分为两个部分：TLB 标记（TLBT）和 TLB 索引（TLBI）。分别用于缓存的组选择和行匹配。如果 TLB 有 $T=2^t$ 个组，那么 TLB 索引是由 VPN 的 $t$ 个最低位组成的，而 TLB 标记是由 VPN 中剩余的位组成的。

{{<figure src="/images/tlb-hit-and-miss.jpg" caption="TLB 命中和不命中的操作图">}}

从上图可以看到，在 TLB 命中时，它消除了一次内存访问（步骤2和步骤3，MMU 从 TLB中取出相应的 PTE）。当 TLB 不命中时，会导致额外的内存访问（MMU 必须从 L1 缓存中取出相应的 PTE，并存放在 TLB 中）。幸运的是，TLB 不命中很少发生。

## 内存映射

Linux 将虚拟内存组织成一些「区域（也叫做段）」的集合。一个区域（area）就是已经存在着的（已分配的）虚拟内存的连续片（chunk）。例如，代码段、数据段、堆、共享夸段，以及用户栈都是不同的区域。每个存在的虚拟页面都保存在某个区域中，不属于某个区域的虚拟页面是不存在的，并且不能被进程引用。

Linux 通过将一个虚拟内存区域与一个磁盘上的对象（object）关联起来，以初始化这个虚拟内存区域的内容，这个过程称为「内存映射（memory mapping）」。虚拟内存区域可以映射到两种类型的对象中的一种：
- Linux 文件系统中的普通文件。一个区域可以映射到一个普通磁盘文件的连续部分，例如一个可执行目标文件。
- 匿名文件。一个区域页可以映射到一个匿名文件，匿名文件是由内核创建的，包含的全是二进制零。

无论是哪一种，一旦一个虚拟页面被初始化了，它就在一个由内核维护的专门的「交换文件（swap file）」之间换来换去。

---

内存映射提供了一种清晰的机制，用来控制多个进程如何共享对象。例如，C 程序都需要的标准 C 库。

一个对象可以被映射到虚拟内存的一个区域，要么作为「共享对象」，要么作为「私有对象」。
- 如果一个进程将一个共享对象映射到它的虚拟地址空间的一个区域内，那么，这个进程对这个区域的任何写操作，对于那些也把这个共享对象映射到它们虚拟内存的其他进程而言，也是可见的。而且，这些变化也会反映在磁盘上的原始对象中。
- 对于一个映射到私有对象的区域做的改变，其他进程是不可见的，也不会反映在磁盘上的对象中。
- 一个映射到共享对象的虚拟内存区域叫做「共享区域」。类似地，也有「私有区域」。

{{<figure width="600" src="/images/memory-mapping-example.jpg">}}

对于共享对象，即使是被映射到多个共享区域，物理内存中也只是存放共享对象的一个副本。私有对象使用「写时复制」的技术被映射到虚拟内存中，最开始私有对象同共享对象一样，在物理内存中只保存私有对象的一个副本。如果有一个进程试图写私有区域内的某个页面，那么就会触发一个保护故障（异常），故障处理程序会在物理内存中创建这个「页面」的一个新副本，并更新页表条指向这个新的页面。

---

Linux 进程可以使用 mmap 函数来创建新的虚拟内存区域，并将对象映射到这个区域中。

```
#include <unistd.h> 
#include <sys/mman.h>

void* mmap(void *start, size_t length, int prot, int flags, int fd, off_t offset);

                  Returns: pointer to mapped area if OK, MAP_FAILED (−1) on error
```

## 动态分配内存

C 程序员使用「动态内存分配器（dynamic memory allocator）在运行时获取虚拟内存。
- 用于只有在运行时才能知道其大小(或生存期)的数据结构。
- 动态内存分配器管理进程的称为「堆」的虚拟内存区域。

分配器有两种类型：
- 显式分配器。程序员分配和释放内存空间。比如， C 中的 malloc 和 free 函数。C++ 中的 new 和 delete 操作符。
- 隐式分配器。程序员只分配内存空间，由垃圾回收器负责释放。比如，java、Lisp 中的垃圾回收器。

---

分配器将「堆」组织为不同大小的「块」的集合。每个块就是一个连续的虚拟内存片（chunk），这些块要么是已分配的，要么是空闲的。
- 分配器请求堆区域中的页面;虚拟内存硬件和操作系统内核将这些页面分配给进程。
- 应用程序对象通常比页面小，因此分配器在页面内管理块。

---

C 标准库提供了一个称为 malloc 程序包的显式分配器。程序通过调用 malloca 函数来从堆中分配块：

```
#include <stdlib.h>

void* malloc(size_t size);
```

malloc 函数会分配一个连续的 size 字节的未初始化的内存块，并返回一个指向这个块开始位置的指针，如果返回 NULL，则表明分配失败。
- 这个块会为可能包含在这个块内的任何数据对象类型做对齐。通常与 8 字节（32 位模式）或 16 字节（64位模式）边界对齐。
- 如果 malloc 遇到问题，比如，程序要求的内存块比可用的虚拟内存要大，那么它就返回 NULL，并设置 errno。

一个好的实践：
```
ptr = (int*) malloc(n * sizeof(int));
```
sizeof 使代码更具有可移植性。void * 能被隐式转换为任何指针类型；在指针类型不匹配时，显示类型转换有助于捕获编码错误。

程序调用 free 函数来释放已分配的堆块：

```
#include <stdlib.h>

void free(void *ptr);
```

free 函数将 ptr 指向的这个块释放到可以内存池。
- 指针 ptr 必须是 malloc 最初返回到地址（即块的开头位置)，否则将引发系统异常。
- 不要在已释放的块或 NULL 上调用 free。

一个动态内存分配的例子，图中的每个方框表示一个字，每个字为 64 位，即 8 字节，分配的大小会是框的倍数（即 8 字节的倍数)。

{{<figure width="600" src="/images/memory-allocation-example.jpg" caption="用 malloc 和 free 分配和释放块。每个方框对应于一个字。">}}

### 分配器的要求和目标

对于应用程序：
- 可以以任意的顺序调用 malloc 和 free。
- 但，决不能访问当前未分配的内存。
- 也决不能释放当前未分配的内存。
- 此外，必须仅对先前 malloc 的块使用 free。

对于分配器：
- 无法控制已分配块的数量或大小。
- 必须立即响应 malloc。
- 必须从可用内存中分配块。
- 必须对齐块，使其满足所有对齐要求。
- 无法移动已分配的块。

在这些限制条件下，分配器的编写者试图在给定分配和释放请求的某种序列 $R0, R1, R_k...R_{n-1}$，实现吞吐率最大化和内存使用率最大化的两个性能目标通常是相互冲突的。
- 最大化吞吐率
  - 吞吐率是指每单位时间内完成的请求数。例如，如果一个分配器在 10 秒钟内完成 5000 个分配请求和 5000 个释放请求，那么它的吞吐率就是 1000/每秒个操作。
- 最大化内存利用率
  - malloc(p) 得到的已分配块的「有效载荷（payload）」是 $p$ 字节。
  - 在请求 $R_k$ 完成之后，「聚集有效载荷（aggregate payload）」 为当前已分配的块的有效载荷的和，表示为 $P_k$，$H_k$ 表示堆当前的大小。假如 $H_k$ 是单调非递减的。
  - 那么，在 $k+1$ 个请求之后的峰值利用率 $U_k=max_{i\le k}P_i/H_k$

在最大化吞吐率和最大化利用率之间是相互牵制的。

### 碎片

造成堆利用率很低的主要原因是存在「碎片（fragmentation）」现象。有两种形式的碎片：内部碎片（internal fragmentation）和外部碎片（external fragmentation）。

内部碎片是在一个已分配块比有效载荷大时发生的。

{{<figure width="600" src="/images/internal-fragmentation.jpg" caption="内部碎片">}}

引发的原因有很多，例如：
- 对齐留下的间隙

外部碎片是在空闲内存合计起来满足一个分配请求，但是又没有一个单独的空闲块能满足分配请求时发生的。

## 垃圾收集

垃圾收集器（garbage collector）是一种动态内存分配器，它自动释放程序不再需要的已分配块。这些块被称为「垃圾（garbage）」。自动回收堆存储的过程叫做「垃圾收集（garbage collection）」。在一个支持垃圾收集的系统中，应用显式的分配堆，但从不显式的释放它们。

垃圾收集器将内存视为一个张有向可达图（reachability graph）。

{{<figure width="600" src="/images/a-directed-graph.jpg">}}

图的节点被分成一组「根节点（root node）」和一组「堆节点（heap node）」。
- 每个堆节点就是一个已分配块。
- 有向边代表一个指针，意味着一个块中的某个位置指向另一个块中的某个位置。
- 根节点对应于一种不在堆中的位置（比如，寄存器、栈里的变量或者是虚拟内存中读写数据区域内的全局变量），它们中包含指向堆中的指针。

如果存在任意一条从根节点到某节点的路径，那么这个节点（块）就是可达的。在任何时刻，不可达节点就是垃圾，是不能被应用再次使用的。