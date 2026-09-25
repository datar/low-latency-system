---
title: "参考摘要 — 软件工程与编程语言"
description: "以通俗语言介绍本站收录的编程语言、无锁编程与社区参考资料。"
permalink: /zh/summaries/software/
translation: /summaries/software/
lang: zh
nav: home
---

# 参考资料摘要：软件工程与编程语言

[← 返回资源列表](/zh/#programming-languages--code) · 以下每一条摘要对应本站收录的一篇参考资料，帮助你判断它是否值得花时间阅读。

---

## When Nanoseconds Matter — David Gross（CppCon 2024 主题演讲，"当纳秒决定成败"） {#gross-2024}

**来源：** [YouTube](https://www.youtube.com/watch?v=sX2nF1fW7kI) · <span class="lvl lvl-a">高级</span> · 2024 年

当前关于高频交易 C++ 的权威会议演讲，由 Optiver 期权技术负责人在 CppCon 2024 作为主题演讲发表。Gross 完整讲解了如何端到端构建一个极速交易系统：订单簿数据结构（为什么基于节点的容器会输给连续存储的有序 vector，以及查找策略应如何顺应更新集中的位置）、并发队列设计，以及如何用硬件计数器测量真正重要的指标。它以生产环境中实际使用的纳秒级技巧，更新了 2017 年那批演讲流传下来的经验之谈。作为 Carl Cook 经典演讲的续篇，值得必看。

## Trading at Light Speed — David Gross（Meeting C++ 2022，"以光速交易"） {#gross-2022}

**来源：** [YouTube](https://www.youtube.com/watch?v=8uAW5FQtcvE) · <span class="lvl lvl-a">高级</span> · 2022 年

Gross 较早的一场全系统视角演讲，讲述如何从零设计低延迟交易系统：把 CPU/硬件知识转化为 C++ 代码、面向性能的数据建模、在管理并发的同时利用多核、C-state/P-state 调优，以及共享末级缓存（LLC）的影响。它与 2024 年主题演讲互为补充，更侧重系统调优与测量方法论——是连接架构级优化与代码级优化的一座好桥梁。

## When a Microsecond Is an Eternity — Carl Cook（CppCon 2017，"当一微秒漫长如永恒"） {#carl-cook}

**来源：** [YouTube](https://www.youtube.com/watch?v=NH1Tta7purM) · <span class="lvl lvl-i">进阶</span> · 2017 年

经典之作，至今仍是 CppCon 观看量最高的演讲之一，主讲人是 Optiver 的工程师。Cook 演示了在热路径上抠时间的具体编码技巧——消除分支、缓存预热、避免系统调用和内存分配——目标是约 2.5 µs 的线到线延迟。部分细节已经过时，但其思维方式和大多数技巧仍是高频交易 C++ 的经典入门。建议与 David Gross 的 2024 年主题演讲搭配观看，以了解当前的技术水准。

## What is Low Latency C++? — Timur Doumler（C++Now 2023，共 2 部分，"什么是低延迟 C++"） {#doumler}

**来源：** [第 1 部分](https://www.youtube.com/watch?v=EzmNeAhWqVs) · [第 2 部分](https://www.youtube.com/watch?v=5uIsadq-nyk) · <span class="lvl lvl-i">进阶</span> · 2023 年

一场分两部分的综述，讲述"低延迟"在交易、音频和游戏领域各意味着什么，以及 C++ 为何在这些领域占据主导。第 1 部分讲效率基础——在热路径上避免内存分配、异常和系统调用；第 2 部分深入硬件效应：缓存、分支预测、低延迟多线程与无等待（wait-free）技术。这是对整个主题最系统、最现代（C++20 时代）的教学式讲解，主讲人是 ISO C++ 委员会成员。在观看各公司的高频交易专题演讲之前，它是最理想的第一次深入学习。

## SPSC Lock-free FIFO From the Ground Up — Charles Frasch（CppCon 2023，"从零构建 SPSC 无锁队列"） {#frasch-spsc}

**来源：** [YouTube](https://www.youtube.com/watch?v=K3P_Lmq6pw0) · <span class="lvl lvl-i">进阶</span> · 2023 年

一步步构建交易系统的主力数据结构：单生产者单消费者（SPSC）无锁（lock-free）队列。Frasch 从最朴素的互斥锁版本出发，逐步应用内存序（memory ordering）、对抗伪共享（false sharing）的缓存行对齐、索引缓存——每一步都做基准测试，精确展示纳秒都花在了哪里。实际上可视为 rigtorp 的 `SPSCQueue` 的视频伴读。这是学习用真实性能数据推理 C++ 原子操作（atomics）的最佳实战入门之一。

## C++ Design Patterns for Low-latency Applications（arXiv 论文） {#hft-paper}

**来源：** [arxiv.org](https://arxiv.org/abs/2309.04259) · <span class="lvl lvl-a">高级</span> · 2023 年

帝国理工学院的论文（Bilokon 与 Gunduz），以统计学的严谨性对低延迟 C++ 的各种经验之谈做了基准测试——缓存预热、`constexpr`、减少分支、循环展开、预取、C++ 版 Disruptor。实测中缓存预热和 `constexpr` 带来的延迟收益最大。论文附带配套代码仓库（[github.com/0burak/imperial_hft](https://github.com/0burak/imperial_hft)），包含全部基准测试代码。它的价值在于把会议演讲里的建议变成了可测量、可复现的数字——也是少数可正式引用的高频交易软件工程学术文献之一。

## Google Benchmark {#google-benchmark}

**来源：** [GitHub](https://github.com/google/benchmark) · <span class="lvl lvl-b">入门</span> · 积极维护中（v1.9.5，2026 年 1 月）

事实上的标准 C++ 微基准测试库，在低延迟社区被普遍用来验证各种优化说法。提供统计上稳健的计时、自动选择迭代次数、用于对抗编译器优化的 `DoNotOptimize`/`ClobberMemory` 屏障，以及性能计数器集成。你在演讲或博客里读到的任何延迟数据，都应该能用它加上 Linux `perf` 复现出来。

## What Every Programmer Should Know about How CPUs Work — Matt Godbolt（GOTO 2024，"每个程序员都该了解的 CPU 工作原理"） {#godbolt-cpu}

**来源：** [YouTube](https://www.youtube.com/watch?v=-HNpim5x-IE) · <span class="lvl lvl-b">入门</span> · 2024 年

Godbolt——Compiler Explorer 的作者、前高频交易工程师——用 43 分钟讲清现代 CPU 流水线、分支预测、乱序执行和缓存，以及编译器如何利用它们。这是当前最好的"机械同情"（mechanical sympathy）入门：Martin Thompson 经典博客教过的一切，按 2024 年的硬件更新，并用现场工具演示。观看各类高频交易专题演讲之前的理想热身。

## C++ Atomics, From Basic to Advanced — Fedor Pikus（CppCon 2017，"C++ 原子操作：从入门到精通"） {#pikus-atomics}

**来源：** [YouTube](https://www.youtube.com/watch?v=ZQFzMfHIxng) · <span class="lvl lvl-a">高级</span> · 2017 年（至今仍是权威）

Pikus 揭开 `std::atomic` 的面纱：原子性和内存序究竟保证了什么、硬件底层做了什么、顺序一致性相对 acquire/release 的真实代价。演讲里满是反直觉的基准测试结果——什么时候互斥锁反而比原子操作快，以及为什么"无锁"不等于"更快"。至今仍是在队列和 seqlock 实现中正确使用原子操作的参考演讲；Pikus 仍在他的 CppCon 课程和著作 *The Art of Writing Efficient Programs* 中讲授这些内容。

## Lock-Free Programming, Parts I & II — Herb Sutter（CppCon 2014，"无锁编程（上、下）"） {#sutter-lockfree}

**来源：** [第 I 部分](https://www.youtube.com/watch?v=c1gO9aB9nbs) · [第 II 部分](https://www.youtube.com/watch?v=CmxkPChOcvw) · <span class="lvl lvl-a">高级</span> · 2014 年（至今仍是权威）

Sutter 关于无锁（lock-free）编程基础的两部曲大师课。第 I 部分建立心智模型并讲清什么时候*不该*用无锁（双重检查锁定、`call_once`、无锁信箱）；第 II 部分讲解无锁链表、ABA 问题，以及借助原子智能指针进行内存回收。虽已十年，但作为概念基础仍无可替代——现代演讲都默认你已消化了它。（旧页面上流传的第 II 部分链接，包括本站之前的版本，视频 ID 被截断了；上面的链接是正确的。）

## Erik Rigtorp — 低延迟 C++ 文章与库 {#rigtorp}

**来源：** [rigtorp.se](https://rigtorp.se/) · <span class="lvl lvl-a">高级</span> · 2020–2021 年，至今仍是权威

被引用最多的现代低延迟工程文章合集："Low latency tuning guide"、"Correctly implementing a spinlock in C++"、"Latency implications of virtual memory"、"Optimizing a ring buffer for throughput"。配套的 MIT 许可库——[github.com/rigtorp](https://github.com/rigtorp) 上的 `SPSCQueue`、`MPMCQueue`、`Seqlock`——是生产级的参考实现，带有精心设计的伪共享填充和索引缓存。这里也是精选清单 [awesome-lockfree](https://github.com/rigtorp/awesome-lockfree) 的所在地。

## 1024cores — Dmitry Vyukov 谈无锁算法 {#vyukov-1024cores}

**来源：** [sites.google.com/site/1024cores](https://sites.google.com/site/1024cores/) · <span class="lvl lvl-a">高级</span> · 约 2010–2013 年（已迁址）

Vyukov 关于无锁/无等待算法、内存模型、生产者-消费者队列分类学的经典参考，还有他那著名的侵入式 MPSC 队列设计——被 JCTools、Akka 和无数交易系统沿用（他还设计了 Go 的可扩展 goroutine 调度器）。**警告：**旧的自定义域名 `1024cores.net` 已失效且被劫持（现在是垃圾站）——上面的 Google Sites 地址才是幸存的官方位置。内容属于 2010 年代，但其中的算法和"要事优先"（first things first）的方法论至今仍是基石。

## Aeron — 低延迟消息传输与集群 {#aeron}

**来源：** [GitHub（aeron-io）](https://github.com/aeron-io/aeron) · [文档](https://aeron.io/docs/) · <span class="lvl lvl-a">高级</span> · 持续开发至 2026 年

Martin Thompson 打造的高吞吐、低延迟消息传输层（UDP 单播/组播及共享内存 IPC），外加 Aeron Archive 和 Aeron Cluster，用于构建交易所级的容错复制状态机。Real Logic 于 2022 年 2 月被 Adaptive Financial Consulting 收购，代码仓库也从 `real-logic` 迁移到了 `aeron-io` 这个 GitHub 组织。它是用 Java 构建撮合引擎的开源参考架构，同时提供 C/C++ 和 .NET 客户端。

## Evolution of Financial Exchange Architectures — Martin Thompson（QCon 2020，"金融交易所架构的演进"） {#thompson-exchange}

**来源：** [YouTube](https://www.youtube.com/watch?v=qDhTjE0XmkE) · <span class="lvl lvl-i">进阶</span> · 2020 年

Disruptor/Aeron 的作者回顾 25 年的交易所架构演进：从大型机，到 LMAX 式内存事件溯源，再到基于 Raft 共识的复制状态机（Aeron Cluster）。演讲解释了为什么"确定性的单线程业务逻辑加顺序化日志"在延迟和正确性上都胜过分布式锁。要理解软件设计模式如何对接容错、回放等交易所级需求，这是最好的一场演讲。

## LMAX Disruptor 与 Chronicle Queue — 免垃圾回收的 Java 工具箱 {#disruptor-chronicle}

**来源：** [Disruptor](https://github.com/LMAX-Exchange/disruptor) · [Chronicle Queue](https://github.com/OpenHFT/Chronicle-Queue) · <span class="lvl lvl-i">进阶</span> · 持续维护至 2026 年

Disruptor 是开创"机械同情"（mechanical sympathy）运动的环形缓冲区线程间消息模式；项目已趋于稳定、更新不多（最新版本 4.0.0，2023 年 9 月），其[用户指南](https://lmax-exchange.github.io/disruptor/)依然是最权威的读物。Chronicle Queue（Peter Lawrey 的 OpenHFT 出品）通过内存映射文件提供微秒级持久化消息——无 broker、一切落盘——被多家大型投资银行采用；当前版本采用日历版本号（2026.x），并已全面支持 Java 25（2026 年 7 月）。两者共同定义了免垃圾回收（GC-free）的 Java 设计范式：预分配、享元（flyweight）、堆外内存。

## Azul Platform Prime（Zing）与 C4 无停顿垃圾收集器 {#azul-zing}

**来源：** [docs.azul.com](https://docs.azul.com/prime/c4-garbage-collection.html) · <span class="lvl lvl-i">进阶</span> · 现行文档

Azul 的 Zing JVM——现在以"Azul Zing Builds of OpenJDK"的形式打包在 Azul Platform Prime 中——凭借 C4（Continuously Concurrent Compacting Collector）垃圾收集器和 Falcon JIT，仍是无停顿 Java 的商业选项。本页文档讲解 C4 如何在没有 stop-the-world 停顿的情况下实现分代、完全并发的内存整理。可将其与 OpenJDK 免费的**分代 ZGC**（JDK 21 引入，JDK 23 起成为 ZGC 的默认模式，JDK 24 起成为唯一模式；用 `-XX:+UseZGC` 启用——JVM 的默认收集器仍是 G1）对比权衡——后者已大幅缩小了在延迟敏感负载上的差距。

## Rust vs C++ for Trading Systems — Databento（交易系统中的 Rust 与 C++ 之争） {#rust-vs-cpp}

**来源：** [databento.com](https://databento.com/blog/rust-vs-cpp) · <span class="lvl lvl-i">进阶</span> · 2025 年 8 月

关于这两门语言在交易领域取舍的最接地气的公开比较，作者是一家在生产环境中同时运行两者的行情数据供应商。C++ 在生态成熟度、交易所连接库和极致调优的热路径上仍占上风；Rust 可以做到与 C++ 相当的延迟，同时在编译期消除内存缺陷，并可通过 C FFI 顺利地渐进式引入。建议与同一家公司的反方观点对照阅读：[Why we didn't rewrite our feed handler in Rust](https://databento.com/blog/why-we-didnt-rewrite-our-feed-handler-in-rust)（2025 年 10 月），解释了他们最热的路径为何仍留在 C++（工具链，以及对缓存、NUMA 和内核旁路的底层控制）。两篇合起来反映的是行业的实际共识，而非语言站队。

## hftbacktest — Rust 高频交易回测框架 {#hftbacktest}

**来源：** [GitHub](https://github.com/nkaz001/hftbacktest) · <span class="lvl lvl-a">高级</span> · 活跃开发中（2025 年 12 月发布版本）

高频交易领域的旗舰开源 Rust 项目：一个 tick 级回测框架兼实盘交易机器人，建模了限价单队列位置、订单簿重建以及行情/报单延迟——这些正是朴素回测器忽略的细节。主体用 Rust 编写并提供 Python 绑定（4k+ star）。它既示范了地道的低延迟 Rust 写法（零拷贝事件处理、预分配数据结构），也可作为研究市场微观结构模拟的学习代码库。

## Mechanical Sympathy Google 讨论组 {#mech-sympathy}

**来源：** [groups.google.com](https://groups.google.com/g/mechanical-sympathy) · <span class="lvl lvl-i">进阶</span> · 2012 年至今

Martin Thompson 于 2012 年创立的讨论组，主旨是"体察底层平台、与之共情地编程"。如今的活跃度远不及 2012–2018 年的巅峰期，但仍然存续（最近的帖子可至 2025 年 5 月），其存档是一座无可比拟的问答宝库，有 Thompson、Gil Tene 和 Peter Lawrey 的亲自解答。Thompson 的配套博客（[mechanical-sympathy.blogspot.com](https://mechanical-sympathy.blogspot.com/)）已停更，但它仍是这场运动几篇核心文章的发源地。

## Databento 市场微观结构指南（ITCH、OUCH、MoldUDP64） {#databento-micro}

**来源：** [databento.com](https://databento.com/microstructure/itch) · <span class="lvl lvl-b">入门</span> · 2023–2026 年，积极维护中

一套免费、现代的参考系列，讲解每个高频交易工程师都必须掌握的协议：基于 MoldUDP64 的 Nasdaq TotalView-ITCH 逐笔委托行情、基于 SoupBinTCP 的 OUCH 报单，以及 tick-to-trade 延迟等概念。比交易所的原始 PDF 平易近人得多，同时附有原文链接。Databento 的工程博客可以说是 2024–2026 年间维护最勤的公开高频交易工程刊物。

## Ultra Low-Latency Trading Systems 博客（SubMicroTrading） {#submicro}

**来源：** [submicro.blogspot.com](https://submicro.blogspot.com) · [GitHub 上的代码](https://github.com/gsitgithub/SubMicroTrading) · <span class="lvl lvl-a">高级</span> · 2015 年（历史资料）

Richard Rose 的博客，记录了构建 SubMicroTrading——一个亚微秒级 Java 交易框架——所用的技术。博客自 2015 年起停更，但整个框架后来已开源，所以你可以直接阅读博客文章背后的真实代码。作为极致免垃圾回收（GC-free）Java 工程的完整实例保留于此。

## Mark Price — epickrram（LMAX）博客 {#epickrram}

**来源：** [epickrram.blogspot.com](https://epickrram.blogspot.com/) · 抖动系列：[第 1 部分](https://epickrram.blogspot.com/2015/09/reducing-system-jitter.html)、[第 2 部分](https://epickrram.blogspot.com/2015/11/reducing-system-jitter-part-2.html) · <span class="lvl lvl-a">高级</span> · 2015–2020 年（已停更）

LMAX Exchange 一位工程师的博客，最著名的是两篇"Reducing system jitter"（降低系统抖动）系列文章：在真实交易负载上，用隔离、追踪和测量的手段系统性地追捕操作系统引入的延迟尖峰。博客自 2020 年起停更，但这两篇抖动文章依然是"先诊断、后调优"闭环的绝佳实例——这正是真正的工程与照抄清单的区别所在。
