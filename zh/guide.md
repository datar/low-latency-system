---
title: "低延迟交易系统完全指南"
description: "用一个页面梳理低延迟交易系统的全部主题，从入门概念到专家级技术，并附 2026 年最前沿的数据。"
lang: zh
permalink: /zh/guide/
translation: /guide/
nav: guide
---

# 低延迟交易系统完全指南

*一个页面，从入门到专家。更新于 2026 年 7 月，2026 年 9 月重新核查。文中每项技术都链回[资源合集](/zh/)中的参考资料，供你深入研读。*

## 目录
{:.no_toc}

* TOC
{:toc}

---

## 1. "低延迟"是什么意思（从这里开始） {#what-low-latency-means}

**延迟（latency）** 是因与果之间的时间间隔：交易所上价格发生变化（*tick*），你的系统作为响应发出一笔订单（*trade*）。这段间隔叫做 **tick-to-trade 延迟**，它是整个领域的头号指标（[Databento 的定义](https://databento.com/microstructure/tick-to-trade)是目前最好的现代版本）。

为什么它重要？在电子化市场里，机会按到达顺序被认领。如果两家公司看到同一个错误定价，快的那家成交，慢的那家错失机会——或者更糟，基于过时信息成交。因此交易中的"低延迟"并不是"平均起来快"，而是**在尾部依然又快又可预测**。一个平均 1 µs 响应、但偶尔要花 1 ms 的系统是坏掉的：慢响应恰恰集中在市场剧烈波动、延迟最要命的时刻。这就是为什么这门学科对**抖动（jitter）**（方差）的执念不亚于速度本身，也是为什么所有严肃的测量都以百分位数（p99、p99.9、max）报告，从不用平均值。

**数量级的直觉** ——在学任何东西之前，先把这些数量级刻进脑子里：

| 时间 | 这段时间里会发生什么 |
|---|---|
| 1 ns | 光传播约 30 cm；约 4-5 个 CPU 时钟周期 |
| ~1–4 ns | L1 缓存命中；一台 layer-1 交换机转发一个数据包（Arista 7130：约 4 ns） |
| ~14 ns | 已公布的最快 FPGA tick-to-trade（STAC-T0 纪录，2024 年） |
| ~30–40 ns | L3 缓存命中；FPGA 行情复用器（MetaMux：约 39 ns） |
| ~100 ns | 一次 DRAM 访问；跨 CPU chiplet 的核间消息 |
| ~500 ns–2 µs | 一条完整的内核旁路软件 tick-to-trade 路径 |
| ~10–50 µs | 一次调优良好的内核网络往返；一次糟糕的 TLB shootdown |
| ~1 ms | 一次没能在设计上消除的垃圾回收停顿；约 200 km 的光纤（单程） |

从这张表能得出两个推论。第一，**物理定律是硬预算**：在纳秒尺度上，线缆长度都有影响，这就是为什么各家公司花钱租用距离交易所撮合引擎仅几米的托管（co-location）机架，也是芝加哥与纽约之间存在微波链路的原因（微波比光在玻璃中的传播速度更快）。第二，**每一层都算数**：应用调得再完美，只要操作系统打断它、网卡缓冲它、或者交换机把它排进队列，就前功尽弃。低延迟是一个*全栈*属性。

> **入门阅读路径：** [Tick-to-trade 延迟](/zh/summaries/networking/#tick-to-trade) → [内核旁路（Cloudflare）](/zh/summaries/networking/#cf-kernel-bypass) → [Matt Godbolt 讲 CPU 工作原理](/zh/summaries/software/#godbolt-cpu) → [Timur Doumler：What is Low Latency C++?](/zh/summaries/software/#doumler)

## 2. 交易系统的解剖图：纳秒都花在了哪里 {#anatomy}

一个最简的电子交易系统长这样：

```
Exchange ──market data (UDP multicast)──▶ [switch] ─▶ [NIC] ─▶ feed handler
                                                                    │
                                                              order book / strategy
                                                                    │
Exchange ◀──order entry (TCP)─────────── [switch] ◀─ [NIC] ◀─ execution gateway
```

tick-to-trade 预算大致按以下顺序被消耗：

1. **线缆 → 网卡（网络）**：光纤/铜缆中的传播、交换机转发、网卡 PHY/MAC。layer-1 交换机能把这段压到约 4 ns；注意在 25GbE 上，仅前向纠错（FEC）一项每跳就增加约 100–250 ns（RS-FEC 处于慢的一端），所以追求极致低延迟的公司在短链路上会关闭 FEC。
2. **网卡 → 应用（主机入向）**：DMA 之后，要么走内核网络栈（微秒级），要么走内核旁路（kernel bypass）栈直接从用户态读取（数百纳秒级）。
3. **解码与建簿**：解析交易所协议（如 MoldUDP64 上的 ITCH）并更新订单簿（order book）——纯粹的 CPU/缓存工作。
4. **决策**：策略逻辑本身。在有竞争力的系统里这一步是纳秒级——预先算好的决策、查表——而不是"跑一个模型"。
5. **订单发出（主机出向 + 网络）**：序列化订单（如 SoupBinTCP 上的 OUCH），经过网卡（CTPIO 之类的直通发送特性有帮助），再穿回交换机。

后面的每一节各攻克这份预算中的一层。专家级的洞见在于**各层是相互作用的**：例如，网卡放在错误的 NUMA 节点会毒害第 2–5 步；一个没驯服的内核定时器 tick 会给第 3–4 步注入抖动，代码写得再好也没用。

**三个技术层级**（2026 年数据）：

| 层级 | Tick-to-trade | 成本与灵活性 |
|---|---|---|
| 调优后的软件（内核旁路，C++/Rust） | ~1–2 µs | 最便宜、最灵活；所有人的起点 |
| FPGA（CPU+FPGA 混合） | ~50–500 ns；纪录 13.9 ns | 需要硬件团队；主流竞争层级 |
| ASIC / eFPGA | ~10–50 ns，确定性 | 仅限顶级公司；僵化且资本密集 |

## 3. 第 1 级 — 先测量（没有商量余地的纪律） {#measure-first}

所有专家级资料在操作顺序上达成一致：**测不了的东西就调不了**。在改动任何一个 BIOS 开关之前：

- **给代码做基准测试**：用 [Google Benchmark](/zh/summaries/software/#google-benchmark) 加上时钟周期级精确计时（[rdtsc 方法论](/zh/summaries/timing/#rdtsc-paper)）；用 `perf` 和 [Brendan Gregg 的工具集](/zh/summaries/linux-tuning/#gregg)做性能剖析——火焰图告诉你时间去了哪里，eBPF/bpftrace 告诉你为什么。
- **测量 OS 抖动**：用 [cns-sysjitter](/zh/summaries/tools/#sysjitter) 或 [cyclictest/rtla](/zh/summaries/tools/#cyclictest)：在每个核上跑一个自旋线程，记录每一次被打断。这是判断你的隔离调优是否真正生效的基准事实。
- **测量机器的拓扑**：用 [core-to-core-latency 或 c2clat](/zh/summaries/tools/#c2clat)：得到每对核心之间通信成本的热力图，它决定线程的摆放。
- **测量线路** ——生产系统使用带纳秒时间戳的被动分光/镜像（[Corvil/Pico](/zh/summaries/tools/#corvil)）；业界经过审计的横向对比是 [STAC 基准测试](/zh/summaries/tools/#stac)。
- **报告百分位数**，永远不要报平均值。p99.9 和 max 才是决定你赚钱还是亏钱的数字。

来自实践者指南的一个好习惯：一次只改一样东西，改的同时保持抖动测量持续运行。很多"优化"是用尾部换均值。

## 4. 第 2 级 — 机器：硬件选型与 BIOS {#hardware}

*本节完整参考资料：[服务器硬件与 CPU](/zh/#server-hardware--cpus)。*

**CPU 选型（2026 年）。** 延迟路径需要的是*少而快、缓存充裕的核心*，而不是又多又慢的核心：

- **超频精品服务器**（Blackcore、Hypertec CIARA）是软件层级的顶配：液冷、体质筛选（binned）、全核约 5+ GHz 的超频 Intel Core/Xeon W/Xeon 600 或 AMD Ryzen/Threadripper 处理器，出厂自带调好的 BIOS 配置和烤机测试。超频的收益不止于主频：它还能降低内存延迟（约 34%）、缓存延迟（约 30%）和 PCIe 延迟。（HPE 旧的"Trade and Match"超频产品线已停产——这个细分市场转向了精品厂商。）
- **AMD EPYC 9005 F 系列**（如 9575F：64 核、5.0 GHz 加速频率、256 MB L3；或 16 核、512 MB L3 的 9175F）是必须使用标准服务器时的指定高频 SKU；**EPYC 4004/4005 X3D** 型号把 3D V-Cache（总计 128 MB L3，但额外的 64 MB 只在两个 CCD 中的一个上）带入了廉价的单路平台——巨大的 L3 能让订单簿常驻缓存（L3 约 49 周期 vs DRAM 约 350 周期）。**下一代：**Zen 6 EPYC 9006 "Venice"（2026 年 7 月发布；高频型号 9686F 加速频率 5 GHz）2026 年第四季度起进入服务器。
- **Intel Xeon 6**（Granite Rapids P 核——仍是 Intel 的低延迟之选：Xeon 6+ Clearwater Forest 只有能效核，Xeon 7 Diamond Rapids 推迟到 2027 年）的应对是 Latency Optimized Mode（把 mesh/uncore 频率保持在高位——[实测约 17% 提升](/zh/summaries/hardware/#phoronix-lom)）、Efficiency Latency Control，以及 SST-BF（让一部分核心以有保障的更高基础频率运行，供你的热点线程使用）。
- **拓扑比品牌重要**：Intel 的 mesh 即使跨 die 也能做到 50–80 ns 的核间延迟；AMD 在 CCX 内部约 8–25 ns，但跨 CCD 约 85–180 ns（随代际和固件而异）。用 c2clat 实测，并据此把相互通信的线程绑在一起。

**已成定论的架构决策：**

- 延迟路径用**单路（single socket）**。跨插槽一跳的核间延迟代价是 2–3.6 倍；把网卡、它的内存和绑定的核心放在同一个插槽/NUMA 节点上。双路机器留给吞吐型工作（风控、回测）。
- **BIOS 铁律**（[AMD 指南](/zh/summaries/hardware/#amd-9005-tuning)、[Dell](/zh/summaries/hardware/#dell-16g)、[HPE](/zh/summaries/hardware/#hpe-low-latency)、[社区清单](/zh/summaries/hardware/#hft-server-settings)）：SMT/超线程**关**；C1 以深的 C-state **关**（或全关）；最大性能/确定性电源配置；turbo 开；uncore 频率固定在高位；功耗/占用率监控类 SMI **关**（SMI 是不可见的、数微秒级的停顿）；内存跑在最高标称频率、所有通道插满；禁用不用的设备。

## 5. 第 3 级 — 操作系统：Linux 调优 {#linux}

*本节完整参考资料：[Linux 操作系统调优](/zh/#linux-os-tuning)。*

目标：给你的热点线程**专用且不被打扰的核心**。现代 Linux 能把微秒级的抖动压到几乎为零，但前提是深思熟虑的配置。以下是分层配方，出自 [Red Hat](/zh/summaries/linux-tuning/#rhel-rt)、[Rigtorp](/zh/summaries/linux-tuning/#rigtorp-guide) 和 [SUSE](/zh/summaries/linux-tuning/#suse-cpu-isolation) 的参考资料：

1. **给机器分区。** 杂务（内核线程、IRQ、守护进程）放在少数几个核上；其余核心全部隔离出来给应用。`tuned` 的 **cpu-partitioning** 配置文件把这一步自动化了，是 Red Hat 推荐的起点。
2. **正确地隔离——单靠 `isolcpus` 不算隔离。** 它只是把核心从调度器中移除，但定时器 tick 和 RCU 回调仍会触发（[一个真实案例：在"已隔离"的核上每 4 ms 出现 200 µs 尖刺](/zh/summaries/linux-tuning/#isolcpus-hft)）。完整配方是 `isolcpus=`（或 cgroup-v2 isolated partition）**+ `nohz_full=`**（自适应无 tick——每秒仅残留一次 tick）**+ `rcu_nocbs=`**（卸载 RCU 回调）**+ `irqaffinity=`**（把中断引导到杂务核心上）。
3. **消灭频率与睡眠状态切换。** 使用 performance governor；通过 `cpu_dma_latency` PM-QoS 限制 C-state（tuned 的延迟类配置文件会保持它打开）——从深度 C-state 唤醒要花数十微秒。
4. **驯服内存。** 静态 hugepages（减少 TLB miss）、`mlockall()`（杜绝缺页）、关 swap、关透明大页、关自动 NUMA 均衡。当心 [TLB shootdown](/zh/summaries/linux-tuning/#tlb-shootdowns)：进程中任何线程的 `munmap`/`madvise` 都可能打断该进程的每一个核——预分配内存，热路径上永不释放。
5. **对投机执行缓解措施做出决策。** 在物理上受保护的单租户交易主机上，`mitigations=off` 是标准做法（开启时历史上有 15–25% 的性能损失；较新的 CPU 在硬件层面修复了更多问题）。逐台主机做风险评估。
6. **考虑 PREEMPT_RT。** 实时抢占**自 Linux 6.12（2024 年 11 月）起已进入主线**——不再需要树外补丁——`kernel-rt` 随标准版 RHEL 9/10 一同发布。注意 RT 优化的是*最坏情况*的调度延迟，代价是吞吐下降——Red Hat 给出的范围是 0–30%，视负载而定；很多交易负载更偏好"调优后的标准内核 + 忙轮询"。Red Hat 的经验法则：标准内核调优就能拿到约 90% 的收益。内核版本提示：自 **Linux 7.0（2026 年 4 月）**起，x86、arm64 等主流架构上已移除旧的 `PREEMPT_NONE`/`PREEMPT_VOLUNTARY` 模型——"标准"内核如今意味着完全抢占或惰性抢占（`PREEMPT_LAZY`，6.13+），升级到 7.x 时请重新做基准测试。
7. **验证。** `rtla timerlat`/`osnoise`（内核 5.17+）、sysjitter、cyclictest——每次改动之后都要跑。

## 6. 第 4 级 — 网络路径 {#network}

*本节完整参考资料：[网络、网卡与内核旁路](/zh/#networking-nics--kernel-bypass)。*

**先理解内核路径**（[CUBRID 心智模型](/zh/summaries/networking/#cubrid)、[packagecloud 深度剖析](/zh/summaries/networking/#packagecloud)、[整合版现代指南](/zh/summaries/networking/#ntk148v)）。内核栈每个包要花微秒级的成本（系统调用、拷贝、中断、softIRQ），单核上限约 100 万 pps——但正确的调优（多队列网卡、RSS/RFS、IRQ 亲和性、一切 NUMA 本地化、忙轮询）能换来很多：[不绕过内核也能 100 万+ pps](/zh/summaries/networking/#cf-million-packets)，[真实服务器上 5 倍提升、每个旋钮都有测量](/zh/summaries/networking/#talawah)。而且在最后一微秒不值钱的所有场景，[留在内核里的理由很充分](/zh/summaries/networking/#cf-kernel-tcp)：你保住了路由、防火墙、工具链和可运维性。

**内核旁路**是交易热路径上的标准做法。协议栈把网卡映射进用户态；应用以轮询（自旋）取代中断：

- **AMD Solarflare**：[Onload](/zh/summaries/networking/#openonload) 对未修改的 socket 应用做透明加速（2021 年起开源；通过 AF_XDP 可运行在其他网卡上）；**ef_vi** 是位于延迟下限的原始 layer-2 API；**TCPDirect** 介于两者之间。全部收录在 [UG1586](/zh/summaries/networking/#onload-guide) 中。
- **NVIDIA（Mellanox 血统）**：**XLIO** 取代了 VMA 做 socket 加速；[Rivermax + NEIO FastSockets](/zh/summaries/networking/#rivermax)（序列化约 0.25 µs）是 2025 年面向行情数据的方案；全自研数据通路则用 DPDK 的 mlx5 驱动。
- **DPDK** 在你掌控整条包路径（行情处理器、网关）时仍是金标准。**AF_XDP** 是"够快、运维更简单"的中间路线；**io_uring** 对一般异步 I/O 有帮助，但不是 tick-to-trade 的工具。

**网卡（2026 年）**：AMD Solarflare **X4**（2025 年 10 月，定制 ASIC、PCIe Gen5、CTPIO 直通发送）领跑软件交易细分市场；**X3522**/X2522 部署广泛；[Cisco Nexus SmartNIC（前 ExaNIC）](/zh/summaries/networking/#exanic)仍能做到 568 ns 的触发到响应，但已进入维护模式；NVIDIA ConnectX-7/8 常见于行情分发、抓包和 RoCE 后端。

**交换机**：自 Cisco 于 2023 年对前 Exablaze 的 Nexus 3550 产品线停售后，[Arista 7130](/zh/summaries/networking/#arista-7130) 独占超低延迟细分市场：layer-1 转发约 4 ns，MetaMux FPGA 复用约 39 ns，MetaWatch 分光镜像带亚纳秒时间戳。常规超低延迟交换机（约 150–250 ns）用于汇聚层。

**RDMA/InfiniBand**：角色自 2018 年以来发生了变化——面向交易所的路径是以太网 UDP 组播；RDMA 以 RoCEv2 fabric 的形式存活于公司*内部*（分发、存储、抓包，约 2–5 µs 级别）。如果你要构建这类系统，[RDMA 编程手册](/zh/summaries/networking/#rdma-manual)仍是参考标准。

## 7. 第 5 级 — 代码 {#code}

*本节完整参考资料：[编程语言与代码](/zh/#programming-languages--code)。*

语言是最小的选择；**机械同情（mechanical sympathy）**——写出 CPU 能够可预测地执行的代码——才是真正的功夫：

- **掌控缓存。** 数据布局决定一切：连续数组优于基于节点的容器，有序 vector 优于 map，每个写者独占一个缓存行（cache line）（避免伪共享——用 `std::hardware_destructive_interference_size` 做填充，或为了 ABI 稳定固定用 64/128 字节）。订单簿首先是个数据结构问题，其次才是算法问题（[Gross 2024](/zh/summaries/software/#gross-2024)）。
- **热路径上不做任何慢的事。** 不分配内存、不做系统调用、不抛异常、不加锁、不缺页。一切预分配；绑定线程；用忙轮询代替睡眠。让热路径保持*热*：如果它很少被执行，连分支预测器和指令缓存都会变冷——所以要用假流量做**缓存预热**，这是[帝国理工研究](/zh/summaries/software/#hft-paper)实测的两个最大收益之一（另一个：`constexpr`——把计算挪到编译期）。
- **并发：消息传递优于共享。** 经典设计是单线程业务逻辑，由无锁（lock-free）SPSC 队列供数（[从零构建一个](/zh/summaries/software/#frasch-spsc)，[rigtorp 的实现](/zh/summaries/software/#rigtorp)）。认真学好内存模型（[Pikus 讲原子操作](/zh/summaries/software/#pikus-atomics)、[Sutter 的上下两部](/zh/summaries/software/#sutter-lockfree)、[Vyukov 的 1024cores](/zh/summaries/software/#vyukov-1024cores)）——并且记住"lock-free"说的是进展保证（progress guarantee），不等于自动变快。[C++26](https://herbsutter.com/2026/03/29/c26-is-done-trip-report-march-2026-iso-c-standards-meeting-london-croydon-uk/)（2026 年 3 月定稿）把 `std::hazard_pointer` 和 `std::rcu` 纳入标准，用于安全的无锁内存回收。
- **C++** 仍是业界默认（上面那批 CppCon 经典，如今由 Optiver 的演讲领衔）。**Rust** 可以做到与 C++ 相当的延迟并快速增长——[Databento 的对比](/zh/summaries/software/#rust-vs-cpp)（请与其解释行情处理器为何仍用 C++ 的反方文章对照阅读）是最坦诚的共识；C++ 保有生态和底层工具链优势。**Java** 只要你坚决不产生垃圾，也能做到低微秒级：[LMAX Disruptor、Chronicle Queue](/zh/summaries/software/#disruptor-chronicle)，消息传递/集群用 [Aeron](/zh/summaries/software/#aeron)，无停顿 GC 用 [Azul C4](/zh/summaries/software/#azul-zing) 或 OpenJDK 的分代 ZGC（JDK 21 引入，JDK 24 起为 ZGC 唯一模式；当前 LTS 为 JDK 25）。
- **懂协议。** 真实系统说的是 MoldUDP64/SoupBinTCP 上的 ITCH/OUCH 或各交易所的等价物——[Databento 的市场微观结构指南](/zh/summaries/software/#databento-micro)是平易近人的参考。
- **来自交易所自身的架构课**（[Thompson：Evolution of Financial Exchange Architectures](/zh/summaries/software/#thompson-exchange)）：确定性的单线程逻辑 + 定序事件日志 + 复制状态机，在延迟和正确性上都胜过分布式锁。

## 8. 第 6 级 — 硬件加速：FPGA 与 ASIC {#fpga}

*本节完整参考资料：[FPGA 与硬件加速](/zh/#fpga--hardware-acceleration)。*

当软件走到尽头（约 1 µs），数据通路就要搬进硬件：

- **FPGA 买到的是什么**：确定性的流水线延迟——解析、建簿、决策、发单全部在门级硬件（gateware）中完成，不受缓存、调度器和中断影响。有竞争力的典型 tick-to-trade：几十到几百纳秒（[一条可落地实现的 ITCH/OUCH 流水线](/zh/summaries/fpga/#dgway-itch-ouch)）。
- **当前最高水平**：AMD 的 [Alveo UL3524](/zh/summaries/fpga/#ul3524)（2023 年）是第一款专为交易打造的 FPGA 卡（<3 ns 收发器）；Exegy + AMD 在它上面用异步（无时钟）关键路径创下 [13.9 ns 的 STAC-T0 纪录](/zh/summaries/fpga/#stac-t0-record)（2024 年），抖动约 200 ps。更便宜的 UL3422（2024 年）把这个层级推向了更低价位的市场。
- **常见架构是混合式的**：FPGA 承担线速热路径；软件（内核旁路 C++）承担策略、风控以及一切每天都在变的东西。厂商出售现成的构建模块——[Orthogone](/zh/summaries/fpga/#orthogone)（17.1 ns MAC/PCS、6.2 ns 发送侧 TCP offload）、Exegy（2022 年并入 Enyx，2026 年又收购 NovaSparks）、[Algo-Logic](/zh/summaries/fpga/#algo-logic)、Xelera（FPGA 上的 ML 推理）。
- **最前沿**：顶级公司把确定性功能烧进 **ASIC**（10–50 ns，僵化且资本密集）或 eFPGA 嵌入 ASIC 的混合方案；下一个战场是关键路径上的在线 ML 推理（[战略综述](/zh/summaries/fpga/#ateam-fpga)）。

## 9. 时间：时钟同步与时间戳 {#time}

*本节完整参考资料：[时钟同步与时间戳](/zh/#time-synchronization--timestamping)。*

没有严肃的时钟，你既无法测量交易系统，也无法合法运营它：

- **监管划定了底线**：欧盟规则要求 HFT 公司与 UTC 的偏差保持在 **100 µs 以内**，且有文档记录、可溯源。自 2026 年 3 月 2 日起，这些要求由 MiFIR 下的[授权条例 (EU) 2025/1155](https://eur-lex.europa.eu/eli/reg_del/2025/1155/oj/eng) 规定——它取代了 MiFID II RTS 25，并把最细的时间戳粒度从 1 µs 收紧到 0.1 µs；英国仍沿用其本土化的 RTS 25（1 µs）。参见[监管要求到 Linux 的映射](/zh/summaries/timing/#mifid-rts25)。
- **带硬件时间戳的 PTP（IEEE 1588-2019 及其 2022–2024 年的修正案；完整修订版正在制定中）** 是主力方案：在支持硬件时间戳的网卡上跑 [linuxptp](/zh/summaries/timing/#linuxptp)（`ptp4l` + `phc2sys`），保守可达约 3 µs，好硬件上可达约 10 ns——但[网卡选择能让精度相差 45 倍](/zh/summaries/timing/#nic-ptp-accuracy)，所以务必实测。[Meta 的部署](/zh/summaries/timing/#meta-ptp)（最坏约 436 ns，全套开源）是参考架构。
- **开放硬件吃掉了 grandmaster 市场**：[OCP Time Card](/zh/summaries/timing/#time-card)（PCIe 卡上集成 GNSS + 原子钟）能把任何服务器变成 stratum-1 时间设备。
- **亚纳秒级**：[White Rabbit](/zh/summaries/timing/#white-rabbit)——CERN 的技术，现已成为 PTP High Accuracy profile——由 [Deutsche Börse 作为托管服务提供](/zh/summaries/timing/#eurex-wr)：你直接同步到交易所自己的时钟。
- **机箱之内**：借助 invariant TSC 和 [rdtsc 方法论](/zh/summaries/timing/#rdtsc-paper)实现时钟周期级精确计时。

## 10. 成长路线图：入门 → 专家 {#roadmap}

**阶段 0 — 打基础（数周）。** 理解业务领域和机器。读 [tick-to-trade](/zh/summaries/networking/#tick-to-trade)；看 [Godbolt 讲 CPU](/zh/summaries/software/#godbolt-cpu) 和 [Doumler 的两部曲](/zh/summaries/software/#doumler)；学习[内核网络路径](/zh/summaries/networking/#cubrid)。交付物：你能讲清一微秒都花在了哪里。

**阶段 1 — 测量（数周）。** 在一台 Linux 机器上搭好 Google Benchmark、`perf` + 火焰图、sysjitter 和 c2clat。给一切建立基线。交付物：一份原厂硬件与内核的延迟/抖动报告。

**阶段 2 — 调优机器（一个月）。** 应用 [BIOS 铁律](/zh/summaries/hardware/#hft-server-settings)和 [Linux 配方](/zh/summaries/linux-tuning/#rigtorp-guide)：cpu-partitioning、`nohz_full`、IRQ 引导、hugepages、governor。每次改动后重新测量。交付物：sysjitter 证明确实安静的隔离核心。

**阶段 3 — 调优网络（一个月）。** 先做内核内的多队列/RSS/IRQ 亲和性/忙轮询（[Cloudflare](/zh/summaries/networking/#cf-million-packets)、[talawah](/zh/summaries/networking/#talawah)）；再用 [Onload](/zh/summaries/networking/#openonload) 或 XLIO 做内核旁路；用 [sockperf](/zh/summaries/tools/#sockperf) 测量。交付物：一条低于 2 µs 的回显路径，并有直方图为证。

**阶段 4 — 写热路径代码（数月）。** [从零构建](/zh/summaries/software/#frasch-spsc)一个 SPSC 队列；认真学好[原子操作](/zh/summaries/software/#pikus-atomics)；用缓存友好的布局写一个 ITCH 解析器和订单簿；对照[帝国理工研究的结论](/zh/summaries/software/#hft-paper)做基准测试。打开代码边看边学 [Optiver 的演讲](/zh/summaries/software/#gross-2024)。交付物：一个热路径上永不分配内存、永不系统调用的行情处理器 + 订单簿。

**阶段 5 — 生产级（持续进行）。** 带硬件时间戳的 PTP 时钟同步；被动抓包与百分位监控；基于确定性重放的架构（[Thompson](/zh/summaries/software/#thompson-exchange)）；路径内的风控检查。这个领域的大多数职业生涯都在这一层。

**阶段 6 — 硬件层级（专精方向）。** 面向真实协议的 FPGA 开发（[Design Gateway 实战讲解](/zh/summaries/fpga/#dgway-itch-ouch)）、厂商 IP、STAC-T0 式的测量。这是一套不同的技能——大多数公司先买后建。

---

*发现错误或遗漏的资源？[提交 issue](https://github.com/datar/low-latency-system/issues)。带逐条摘要的完整参考列表在[首页](/zh/)，本次 2026 年更新背后的调研在[研究笔记](/research/)（英文）中。*
