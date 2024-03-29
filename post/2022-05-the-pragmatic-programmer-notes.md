---
title: "The Pragmatic Programmer 笔记" # Title of the blog post.
date: 2022-05-26T13:38:20+08:00 # Date of post creation.
description: "这是《程序员修炼之道 第2版》一书的读书笔记"
draft: false # Sets whether to render this page. Draft of true will not be rendered.
categories:
  - 读书笔记
tags:
  - 程序员
# comment: false # Disable comment if false.
---

这是[《程序员修炼之道 第2版》](https://book.douban.com/subject/35006892/)一书的读书笔记。


> 编程是一项艰难的工作

你不应该拘泥于任何特定的技术，而应该拥有足够广泛的**背景**和**经验**基础，以便在特定的情况下选择合适的解决方案。你的**背景来自对计算机科学基本原理的理解**，而你的**经验来自广泛的实际项目**。

务实的程序员共有的特征：

- 喜欢尝试。接触到新事物，能很快地掌握并应用起来。
- 好奇。倾向于问问题。
- 批判性的思考者。没有得到证实前很少接受既定的现实。
- 现实主义。试图理解面临的每个问题的本质。
- 多面手。熟悉各种技术和环境。
- 关注技艺。技艺人，不关注自己的技艺怎么能做好技艺活呢。

***

### 一

> 人生是你的，是你在拥有、经营和创造。

工作环境很糟糕？工作很无聊？尝试纠正它。

想远程工作？要求过了吗？

你有选择权，人生是你的。

***

责任意味着你对某事积极认同。

承担责任，做出承诺之前，必须分析超出你控制范围的风险情况，如果责任不清，抑或风险过大，你有权不承担责任。

承担责任意味着你将承接相关义务。

- 犯了错误或是做出了错误的判断，**诚实的承认它，并尝试给出选择**
- 不要把问题归咎于别人或其他什么事情上，也不要寻找借口
- 不要说搞不定，解释一下要做什么才能挽回局面

意识到自己在说“我不知道”时，一定要接着说“---但是我会去搞清楚”。

不要搁置“破窗”（糟糕的设计、错误的决定、低劣的代码），每发现一个就**赶紧**修一个。

不要只是因为一些东西非常紧急，就去造成附带损害。

破窗一扇都嫌太多。

构建知识组合，并对知识组合做定期投资。

读非技术书。你做的事情是为了**满足人**的需要。

批判性地分析你读到和听到的东西。

- 问“五个为什么”。有了答案后，还要追问“为什么”
- 谁从中受益。追踪金钱的流动更容易理清脉络
- 有什么背景。每件事都有它发生的背景。
- 什么时候在哪里可以工作起来。是在什么情况下？太晚了嘛？不要停留在一阶思维下（接下来会发生什么），要进行二阶思考：它结束后还会发生什么？
- 为什么是这个问。是否存在一个基础模型？这个基础模型是怎么工作的？

把母语看成另一门编程语言，像写代码一样用自然语言写文章：尊重 DRY 原则、ETC、自动化。

想说什么之前问一下自己：“这是否用正确的方式向我的听众传达了我想表达的东西？”

记下你想要沟通的想法，并准备多个让对方理解的策略。

听他们说。你不听他们的，他们也不会听你的。

回应别人。让人感觉到你并没有忘记他们。


### 二

> ETC（Easier To Change，更容易变更），对代码而言，就是要顺应变化。

无论什么设计原则，都是 ETC 的一个特例。e.g，解耦，单一职责原则。

价值观是帮助你做决定的：应该做这个，还是做那个？在软件领域思考时，ETC 是一个向导，它能帮助你在不同的路线中选出一条。**ETC 是一种价值观念，不是一条规则**。

> DRY---不要重复自己

DRY 针对的是你对**知识**和**意图**的复制。它强调的是，在两个地方表达的东西其实是相同的，只是表达方式有可能完全不同。

被要求做一个估算时说什么？应该说“我等一下答复你。”

估算使用的时间单位会对结果的解释产生影响。如果你说某件事需要 130 个工作日完成，那么听的人往往觉得实际需要的时间会很接近这个数字。然而，如果你说的是“大约 6 个月吧”，他们就会认为是需要 5 到 7 个月不等。建议采用下面的时间尺度做估算：

| 持续时间 | 采用的估算单位 |
| --- | --- |
| 1-15 天 | 天 |
| 3-6 周 | 周 |
| 8-20 周 | 月 |
| 20+ 周 | 说出估算前再仔细想想 |

### 三

> 工具会放大你的才能

工具越好，同时你越知道怎么用更好，效率就越高。

要一直寻找更好的做事方法。

每次发现自己又在重复做某件事情的时候，要习惯性地想到“或许有更好的方法”，然后找到这个方法。

一旦发掘出一个新的特性，尽快把它内化成一种肌肉记忆。方法是不断重复，有意识地寻找机会使用，最好是一天多次。

学着用组合键，以字符、单词、行、块为单位，进行移动、选择和删除操作。

学习过程中，要将学到的按键组合记录下来（用上学时的老办法，**使用铅笔和纸**）。

> 永远使用版本控制

即使你只有一个人且项目周期一周就会结束，即使它是一个“用完即弃的”原型，即使是非项目的事物，永远都应如此 --- 确保所有内容都在版本控制之下。

如果你在看到 Bug 或 Bug 报告时的第一反应是“这不可能”，那你就大错特错了。不要在“但那不可能发生”的思路上浪费哪怕一个神经元，因为很明显它会发生，而且已经发生了。

调试时，把纸笔放在旁边会很有帮助，可以随时做笔记。如果之前没有记下从哪里开始的，可能会在找回源头上浪费很多时间。

跟踪消息应该采用规范一致的格式，有可能需要自动对其解析。例如，如果需要跟踪资源泄露（例如不平衡的文件open/close），可以在日志文件中跟踪每个文件打开和关闭操作。通过使用文本处理工具或 shell 命令来处理日志文件，就可以很容易地识别出，有问题的打开操作发生在哪里。

团队里的讨论，如果一个人误解了，那么很可能很多人都误解了。

记录工程日记吧。

### 四

> 崩溃，不要制造垃圾

一旦代码发现本来不可能发生的事情已经发生，程序就不再可靠。从这一刻开始，它所做的任何事情都是可疑的，所以要尽快终止它。

发现自己在想“当然这是不可能发生的”时，添加代码来检查这一点。最简单的方法是使用断言。
```
assert(result != null);
```

分配资源的函数或对象，应该负责释放资源。

对嵌套的分配的两个额外的建议：

- 释放资源的顺序与分配资源的顺序相反。
- 在代码的不同位置，如果都会分配同一组资源，就始终以相同的顺序分配它们。

在有异常的环境中，处理资源回收的正确模式是：

```
thing = allocate_resource()
begin
    process(thing)
finally
    deallocate(thing)
end
```

当你不得不做下面这些事情的时候，可能已经陷入了“占卜”的境地了：

- 估计未来几个月之后的完成日期
- 为将来的维护或可扩展性预设方案
- 猜测用户将来的需求
- 猜测将来有什么技术可用

与其浪费精力为不确定的未来做设计，还不如将代码设计成可替换的。使代码可替换，有助于提高内聚性、解耦和 DRY，从而实现更好的总体设计。

### 五

> 一切都是为了变更

耦合有传递性：如果 A 与 B、C 耦合，B 与 M、N 耦合，C 与 X、Y 耦合，那么 A 实际上与 B、C、M、N、X 及 Y 耦合。

解耦代码让改变更容易，三个方面：

- 一连串的方法调用。
- 全局数据
- 继承

***

**只管命令不要询问**。不应该根据对象的内部状态做出决策，然后更新该对象。例如：

```
public void applyDiscount(customer, order_id, discount) {
	totals = customer
				.orders
				.find(order_id)
				.getTotals();
	totals.grandTotal = totals.grandTotal - discount;
	totals.discount = discount;
}
```

这样做完全破坏了封装的优势，也会把实现相关的知识扩散到整个代码中。

把计算折扣的工作委托给 total 对象：

```
public void applyDiscout(customer, order_id, discount) {
	customer
		.orders
		.find(order_id)
		.getTotals()
		.applyDiscount(discount);
}
```

对于客户对象和对象的订单来说，不应该取出订单列表在从中搜索，而应该直接向客户要想要的订单：

```
public void applyDiscout(customer, order_id, discount) {
	customer
		.findOrder(order_id)
		.getTotals()
		.applyDiscount(discount);
}
```

订单对象和订单的总金额之间也有相同的问题。订单的实现使用了一个分离的对象 totals 来保存总金额，外部世界为什么要知道这么一个对象呢？

```
public void applyDiscout(customer, order_id, discount) {
	customer
		.findOrder(order_id)
		.applyDiscount(discount);
}
```
***

访问某样东西时，尽量不要超过一个 “.”，也就是不要链式调用方法。

```
# 这是很糟糕的风格
amount = customer.orders.last().totals().amout;
# 这也一样......
orders = customer.orders;
last = orders.last();
totals = last.totals();
```

“一个点” 规则的例外是：如果链式调用的东西真的不太可能改变。

***

任何可变的外部资源都是全局数据，始终确保将这些资源包装在你所控制的代码之后。

***

有限状态机（FSM）基本上就是怎么处理事件的一份规范。它由一组状态组成，其中一个是**当前状态**。对于每个状态，我们列出对该状态有意义的事件。对于每个事件，我们定义出系统新的当前状态。

例如，我们可能从 websocket 接收到多个部分的消息。第一消息是消息头，接下来是任意数量的数据体，之后是消息尾。用 FSM 表示为：

![](/images/IMG_200.jpg)

我们从 “初始状态” 开始。如果我们接收到消息头。就**转换**到 “读取消息” 状态。如果在初始状态接收到任何其他内容（带有星号的线），我们就转换到 “出错” 状态。

当我们处于 “读取消息” 状态是，可以接收数据消息（在这种情况下，我们将继续以相同的状态读取消息），也可以接收消息尾（它将让我们转换到 “完成” 状态）。其他任何操作都会导致转换到 “出错” 状态。

处理代码比较简单：

```
TRANSITONS = {
  initial: { header:  :reading },
  reading: { data:  :reading, trailer:	:done }
}

state = :initial

while state != :done && state ! :error
  msg = get_next_message()
  state = TRANSITONS[state][msg.msg_type] || :error #实现状态间转换
end
```

***

观察者模式因为每个观察者都必须与观察对象注册在一起，所以它引入了耦合。此外，回调是由观察对象以同步的方式内联处理的，因此可能会导致性能瓶颈。

```
module Terminator
  CALLBACKS = []
    
  def self.register(callback)
    CALLBACKS << callback
  end
  
  def self.exit(exit_status)
    CALLBACKS.each { |callback| callback.(exit_status) }
    exit!(exit_status)
  end
end

Terminator.register(-> (status) { puts "callback 1 sees #{status}" })
Terminator.register(-> (status) { puts "callback 2 sees #{status}" })

Terminator.exit(99)
```

**发布/订阅**（pubsub）推广了观察者模式，同时解决了耦合和性能问题。

在 pubsub 模式中，有`发布者`和`订阅者`。它们是通过信道连接在一起的。信道在单独的代码块中实现：有时是库，有时是进程，有时是分布式基础设施。所有这些实现细节对代码来说都是隐藏的。

每个信道都有一个名字。订阅者注册感兴趣的一个或多个具名信道，发布者向信道写入事件。与观察者模式不同，发布者和订阅者之间的通信是在代码之外处理的，并且可能是异步的。

***

编程讲的是代码，而程序谈的是数据。

所有程序其实都是对数据的一种变换---将输入转换成输出。

例如：列出目录树中行数最多的五个文件。

```
$ find . -type f | xargs wc -l | sort -n | tail -6 | head 5
```

从各个步骤之间数据流动的角度来考虑，就变成了一系列的转换：

```
目录名
->列出文件
->列出行数
->对列表排序
->最多的五项 + 总数
->最多的五项
```

在变换式模型中，要把数据看作是一条浩浩荡荡的`河流`。数据成为与功能对等的东西：`代码->数据->代码->数据......`。

***

> 不要付继承税

继承就是耦合。

用这三种技术，不要再使用继承：

- 接口与协议。尽量用接口来表达多态
- 委托。用委托提供服务：“有一个” 胜过 “是一个”
- mixin 与特征。利用 mixin 共享功能

***

如果代码依赖某些值，而这些值在应用程序发布后还有可能改变，那么就把这些值放在程序外部。

将配置信息包装在一个（瘦）API 后面。不直接放在文件中，也不放在数据库里，而是存储在一个服务 API 之后。

> 不要因为懒，就放弃做决策，而把分歧做成配置。

如果对某个特性应该这样工作还是那样工作真的存在争议，或者不知道是否应该让用户选择，那就去找个方法试试，通过反馈来决定哪一种更好。

### 六

**并发性**指的是两个或更多个代码段在执行过程中表现得像是在同时运行一样。**并行性**是指它们的确是在同一时刻一起运行。

> 通过分析工作流来寻找并发存在的地方。

在项目中，我们想知道哪些事情可以同时进行，哪些必须按照严格的顺序发生。这时可以利用**活动图**来理清工作流，通过分析工作流来找到并发存在的地方。

### 七

当编程举步维艰时，其实是潜意识在告诉你有什么地方不对劲。

在开始编程前，对这件事大概会花多长时间要有概念。可以用常识判断出许多基本算法的级别：

- 简单循环。算法是 *O*(n) 时间的---时间增加和 n 线性相关。例子有穷举查找、找到数组中的最大值。
- 嵌套循环。在循环中嵌套另一个循环，算法就变成了 *O*(m x n)，m 和 n 分别是两层循环的上限。例如冒泡排序。趋近于 *O*(n^2)。
- 二分法。在循环中每轮将集合对分，那么算法是 *O*(lg n) 时间的。对有序二分查找、遍历二叉树、找到机器字的最高位，都是 *O*(lg n) 的。
- 分治法。如果算法将输入分成两半来分开处理，最后再把结果合并起来，那么就是 *O*(n lg n) 的。经典的例子是快速排序。
- 组合问题。当算法开始关注事物的排列组合时，运行时间可能会失控。因为排列涉及阶乘（数字 1 到 5 点排列就有 5! = 5 x 4 x 3 x 2 x 1 = 120 种)。以计算 5 个元素的组合算法的时间为基准：运行 6 个元素需要 6 倍的时间，运行 7 个元素需要 42 倍的时间。例子包括了许多公认的*困难*问题的算法---旅行推销员问题、最优装箱问题。

> 测试与找 bug 无关。

测试是观察代码的一个视角，可以从中得到针对设计、接口和耦合度的反馈。

严格的密码策略实际上会**降低*完全性。NIST 的建议：

- 不要将密码长度限制在 64 个字符以内。NIST 推荐 256 为最佳长度。
- 不要阶段性用户选择的密码。
- 不要限制特殊字符，比如`;&%$#/`。
- 不要向未经身份认证的用户提供密码提示，或提示输入特定类型的信息（例如，“你的第一只宠物叫什么名字？”）。
- 不要禁用浏览器中的`粘贴`功能。破坏浏览器和密码管理器的功能，会促使用户创建更简单、更短、更容易破解的密码。
- 不要强加其他组合规则。例如，不要强制要求任何特定的大小写混合、数字或特殊字符，或禁止重复字符，等等。
- 不要蛮横地要求用户在一段时间后更改密码。只有在有正当理由的情况下才这样做（例如，系统遭到了破坏）。

涉及到加密时，永远不要自己做加密算法。

### 八

需求不是架构；需求无关设计，也非用户界面；需求就是需要的东西。

没有人能确切地知道自己想要什么。现实世界是混乱的、矛盾的、未知的，在现实世界中，得到任何事物的精确规范，即使不是完成不可能，也是非常罕见的。**我们程序员的工作是帮助人们理解他们想要什么**。

和用户一起工作以便从用户角度思考。你正在为帮助台编写一个系统，不妨花几天时间和有经验的客服一起接电话。你正在让手动库存控制系统自动化，试着去仓库工作一周。

创建并维护项目术语表，在上面记录项目中所有特定术语和词汇的定义。项目所有的参与者，包括最终用户和支持员工，都应该使用同一张术语表来确保一致性。

解决谜题的关键是，认识到你所受到的约束和你所拥有的自由度。

当面对一个棘手的问题时，把你面前所有可能的解决途径都列出来。不要忽略任何东西，无论听起来多么无用或愚蠢。然后逐条解释为什么不能选择它。

一种积累工程经验的方法：记录工程日记。

与所有合作性事物一样，你需要对人的方面如同技术方面一样有所把控。

- 打造代码，而非打造自我。
- 批评要针对代码，而不针对人。
- 观点不同不是错误。

敏捷是一个形容词，它指向你做事的方式。

无论什么时候有人说，“这么干，你就敏捷了”，显然都是错的。

敏捷的价值观不会告诉你该做什么，当你想要做点什么的时候，它们会告诉你要去追寻什么，那便是做事时该有的精神，也就是如何处理不确定性，即通过收集反馈，并采取行动。

### 九

在我们看来，团队是小而稳定的实体。50 个人就不算是团队，那是部落（随着团队规模的增长，沟通路径以 *O*(n^2) 的速度增长，其中 n 是团队成员的数量。)。如果团队的成员经常被分配到其他地方，相互之间缺乏了解，那么这也不是一个团队，他们只是暂时同在一个公交车站躲雨的陌生人。

同时标注已做工作和待做工作的燃尽图（burnup)，要比只标注待做工作的燃尽图（burndown）好，因为前者便于清楚地看到增加功能如何改变目标的。

“只有有空闲时间” 就去做，意味着这件事永远不会发生。

不要仅仅因为“大家都在这么做”，或是你在会议上听到、网上看到了什么，就去采用新的技术、框架或库。做一个原型来慎重考察候选技术。

当开始一个项目时，给它起一个名字，最好是一个稀奇古怪的名字。

与别人交谈时，大方地使用团队的名字。

测试状态的覆盖率，而非代码的覆盖率。

```
int test(int a, int b) {
  return a / (a + b);
}
```

这个函数接受两个整数，假设每个整数是 0 到 999 之间的数字。理论上，这个三行的函数有 1,000,000 个逻辑状态，其中 999,999 个可以正常工作，而另一个则不能（a + b 等于零时）。仅仅知道执行了这行代码并不能告诉你这些，你需要识别出程序的所有可能的状态。

用户真正要的不是代码，他们只是遇到某个业务问题，需要在目标和预算范围内解决。他们的信念是，通过与你的团队合作，能够做到这一点。

用户的期望与软件无关，他们的期望是对业务价值的期望，软件只是到达这一目标的一种手段。了解到他们的价值期望后，就要考虑如何实现这些期望：

- 确保团队中的每个人都清楚这些期望。
- 在做决定的时候，想想哪条路更接近这些期望。
- 根据期望严格分析用户需求。在许多项目中，一些“需求”是与期望相悖的，如果能证明有方法使项目更接近目标，就大胆提出修改需求的建议。
- 随着项目的进展，继续考虑这些期望。

> 在作品上签名

我们想看到你对所有权引以为豪---“这是我写的，我与我的作品同在”。你的签名应该被认为是质量的标志。人们应该在一段代码上看到你的名字，并对它是可靠的、编写良好的、经过测试的、文档化的从满期许。

*** 

### 后记

读完本书后，找到了译者 @云风 老师最近的一篇采访，摘录一些文字在这里。

**《新程序员》：你们现在忙吗？**

云风：还算有点忙，但至少我负责的团队，从不鼓励加班。我们在晚上六、七点钟就下班，下班后也不谈工作。在我看来，做开发这项工作，最主要的是把事情想清楚，再按照计划好的工作时间做事。比如一件事你做了两年，回头看有多少时间在做有效工作？可能你做了很多无谓的事情，占用了太多时间。你会发现，最终有效工作花掉的时间并不多。因此，要少犯错误、少走弯路，减少返工，这样就会有很多时间被节省下来。如果事情没有做好，我作为团队领导者责任最大，是我带着大家走了弯路。

**《新程序员》：很多伟大的项目往往由一个人或者一个极小数量的群体发起并成长起来的，但在企业里，作为开发者缺乏高层决策能力，项目的开展也讲求完美的计划，多人头脑风暴，你认为这两者之间是否存在“矛盾”？**

云风：我个人不太喜欢多人头脑风暴，大部分时候都是在浪费时间，只有极少数头脑风暴能带来一点灵感。我们启动新项目时，通常只在吃饭的时候，大家凑在一起聊一聊，很少在工作时间开大会。

首先，我个人比较反感开会这件事，我认为做好工作计划，大家各自执行就好了，然后团队负责人尽量了解其他人在做什么事，做好沟通工作。一堆人为了沟通专门坐在一起开会，是非常没有效率的。

其次，项目做得好不好，最为关键的往往只在一两个人。

**《新程序员》：您如何看待开源的发展及它对世界的影响？未来开源还会向哪些方向发展？**

云风：世界上要解决的问题是非常多的，尤其是一些根源问题，但每个问题都只能找到极少的几个人去解决。因为对于程序员而言，做得好的人和做得不那么好的人，会相差几个数量级。如果你需要解决的问题非常需要人去做的话，那么可能你在世界上只能找到那么几个人可以把这个事情做好，不是说其他人做不好，其他人可能不关心这个问题，或者不是他的领域。如果这个前提正确，你就会发现很多基础设施的问题和一些很专注的问题，若限定在一个公司里边，根本找不到合适的人。只有群体足够大，大到世界范围，才可以找到对的人把事情做好或把问题很好的解决，我觉得这是开源最大的意义：把特定的事情和擅长做这些事的人连接、聚集在一起。

正因此，不是所有的代码都应该开源，比如一项特别专注于某个领域或某项技术的业务代码，开源没有意义，因为不需要有很多人参与。反之，越是能被大众广泛使用的项目，越值得开源。游戏引擎同理，大家不需要把精力都浪费在同样的事情上。

当然，开源的发展与互联网的发展相互关联，如果互联网不那么发达，就没办法把世界各地、各种各样的人连接在一起。举个例子，为什么在早年间美国的大学之间会流行开源、黑客文化等？因为他们有自己的网络。当人和人不能顺畅沟通的时候，就谈不上做开源这件事，而开源又会极大地促进互联网行业的发展，因此我认为，至少对于通用的、底层的设施，必然会走向开源，不应该有第二条路。

**《新程序员》：你对中国的开发者在开源领域，有哪些期待或倡导？**

云风：当我们作为开源领域的参与者或贡献者时，最重要的能力不是代码写得比别人好多少，而当我们作为一个开源项目的主导者，最重要的能力是与人沟通的能力，听更多人的意见，求同存异。这和我们在公司当差不一样，在公司里，大家都拿工资，下级需要听从上级的安排，但在开源领域，大家可能都是“为爱发电”，只有信任才能让彼此更好地协作，把事情做好，因此，通过网络语言让别人相信你的想法，愿意跟你共事，是相当重要的。

另外，怎么让你的项目吸引更多人，一起把你的项目做好，也非常重要。拿到一个开源项目，私下做一个分支，我不喜欢这样的开发方式，我觉得对于开源项目，如果它是一个基础设施，所有参与贡献的开发者就应该抱紧了朝一个方向走下去，即使这个方向有人并不喜欢，但可以先参与进去，慢慢将其修正为自己喜欢的方向，或逐渐改变自己的想法，尽量减少分叉。对于这种情况，你需要花费很多时间与参与者或用户，也可以称其为潜在的开发者，保持良好的关系。

**《新程序员》：如今，你已经有三十多年码龄了，在你的编程生涯中，从前和现在的做事方式或观念有没有一些变化？**

云风：改变都是渐进的，从趋势上来看，我越来越趋向于写更简单的程序，而不追求性能更好。但我想说，把程序写简单，不论是空间性能还是时间性能都更容易变好。性能是一种自然而然的结果，不必刻意追求。

现在做事情也更有规律。二十几岁时，我经常通宵写代码，一旦进入状态能写到第二天早上七、八点，还不觉得累，觉得一天能做很多事，非常有满足感。而现在，我会在每天早上或头天晚上临睡前想好接下来要做什么事、几个小时完成、大概怎么做，以及在每天下午四、五点钟思考下一步的计划。即使这个时间段我有很好的工作状态，也会去想下一步该怎么做、应该在哪里停下来。在下班之前，我会把工作告一段落，并想好第二天怎么接着启动更好。如果是周五，就告一个大的段落，如果这件事比较重要，我在周末两天也会思考，但不会动手做。把事情分阶段完成，讲究节奏，不倾向于突击，是我在做事方面比较大的改变，三思而后行。

**《新程序员》：以你多年的从业经历来看，优秀的研发人员具有哪些品质和能力？**

云风：很多能力都重要，近两年我认为，比较重要的能力是抓住问题的本质。就是可以用足够简单的方法去解决根源问题，抛开中间的一些枝节干扰。

为什么我会强调用简单的方法？Keep it Simple and Stupid，即KISS原则。在20年前，我不完全理解这个道理，比如，我在大学期间以及刚毕业的时候，比较喜欢做程序的优化，让自己写的代码比别人写的代码跑得快。很多年轻程序员和我一样，都喜欢炫技。但我现在看来，这些事不是解决问题的本质。这与把事情做简单有什么关系？以前我认为写出复杂的程序并且不出错是一种出色的能力，可随着时间的推移，我的代码需要被别人维护，可能还要和其他人合作，这时我们需要在这群人之间找到一个共同的基点，让代码更容易理解。所以我们需要让代码足够简单，让别人一看就明白。什么样的代码是好代码？并不是看上去好像没有问题的代码，而是看上去所有东西都清清楚楚，断定它肯定不会出问题的代码。

最近几年，我写程序很少炫技。炫技在短时间内看，是用一个很巧妙的方法把问题解决掉了，但经不起时间的检验。把复杂的问题简单化，对程序员而言是一项非常重要的能力。

另一项重要能力是评估事物的能力，知道一件事情大概是怎么回事，需要多长时间完成，需要什么条件完成，这是靠经验堆出来的。也要对自身有清晰的认知，这样你才能规划好你做整件事情的流程和时间，减少返工。通常一件事谁都能做，比如，同样是2000行代码，有人只需要两天，有人却需要两周、两月，为什么？当一个程序员经验不足时，他评估一件事可以用两天搞定，但他可能又花费两周甚至两月来解决他做事过程中发现的问题，改错、返工，导致和预期完成时间出入很大。这就是能力的差异，否则程序员的能力差异体现在哪里？别人会的东西，你学习后也能掌握，所以真正在做事时，能力的差异就在于一个人的评估能力。

**《新程序员》：那么，有哪些人生感悟、研发感悟可以分享给大家吗?**

云风：第一，做事情要专注。想好一件事情就去做，只要花的时间和精力足够多，不三心二意，这件事终究会给你回报。每个人的生命都几乎一样长，你不可能比别人多出很多时间和精力，你也不会比别人差在哪或好多少，那么，一件事情做得好不好，就看你投入的时间多不多。但过程中是有方法的，不能蛮干，时常回顾自己怎么才能把这件事学好、做好，发现错误或更好的方向，及时修正。

我现在40多岁，每个人从十几岁开始明白这个世界的道理，到了40多岁，也就30年的时间，每个人都是30年。如果这30年间你都做一件事，那你会超越大部分人。对于任何事情，如果你处于金字塔上层的位置，就会觉得自己是个很有用的人。很多事情必需你来做才能做好的时候，自然而然，这个世界就会给你回报。

第二，信任和你共事的人，接受他们做得不好的方面。以把整件事情做好为主。每个模块都按照你的想法做，通常是行不通的。你可以找准你真正想要的那件事，围绕那件事情，把它做好，其他事情放心交给你的伙伴。我反复讲，人不会比其他人强多少，也不会差多少，你找对人一起做事，接纳其他人的不同想法，求同存异，把整件事做好就可以了。


(完)


