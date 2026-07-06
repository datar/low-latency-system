---
title: "参考摘要 — Linux 操作系统调优"
description: "本站收录的所有 Linux 操作系统调优参考资料的通俗摘要。"
permalink: /zh/summaries/linux-tuning/
translation: /summaries/linux-tuning/
lang: zh
nav: home
---

# 参考资料摘要：Linux 操作系统调优

[← 返回资源列表](/zh/#linux-os-tuning) · 下面每一条摘要对应收录中的一份参考资料，帮助你判断它是否值得花时间阅读。

---

## 针对低延迟运行优化 RHEL 实时系统（Optimizing RHEL for Real Time for Low Latency Operation，RHEL 9/10） {#rhel-rt}

**来源：** [docs.redhat.com](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux_for_real_time/9/html-single/optimizing_rhel_9_for_real_time_for_low_latency_operation/index) · <span class="lvl lvl-i">进阶</span> · 持续更新（2022–2026）

Red Hat 当前关于将 RHEL 系统调校至稳定、可预测低延迟的权威指南——本站多年链接的旧版 RHEL 7 低延迟调优文章的直接继任者。内容涵盖 TuneD 实时配置文件、调度策略与优先级、CPU 隔离与中断（IRQ）绑定、内存锁定与大页（hugepages）、规避 TCP 协议栈和日志写入引入的延迟，以及使用 `hwlatdetect` 和 `rteval` 进行硬件/固件延迟测试。自 RHEL 9 起，实时内核（`kernel-rt`）已随标准 RHEL 一同提供——无需单独购买产品。对于在 RHEL 9/10 上部署交易系统的人来说，这是必读的基础资料。

## 监控与管理系统状态和性能（Monitoring and Managing System Status and Performance，RHEL 9） {#rhel-perf}

**来源：** [docs.redhat.com](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html-single/monitoring_and_managing_system_status_and_performance/index) · <span class="lvl lvl-b">入门</span> · 持续更新

已退役的 RHEL 7《Performance Tuning Guide》的现代替代品。它介绍了在当前 RHEL 上进行系统性能工作的概念与工具：TuneD 配置文件、PCP/perf 监控，以及网络、内存、磁盘和 CPU 调优各章节。这些概念同样适用于 CentOS Stream、Rocky、Alma，以及（工具略有不同的）其他发行版。如果你是 Linux 性能调优新手，请先读这一篇；之后再深入上面的实时系统指南。

## Erik Rigtorp — 低延迟调优指南（Low Latency Tuning Guide） {#rigtorp-guide}

**来源：** [rigtorp.se](https://rigtorp.se/low-latency-guide/) · <span class="lvl lvl-i">进阶</span> · 2020 年

针对 HFT 类工作负载调优 x86_64 Linux 时被引用最多的实践者清单，作者是一位交易系统工程师。它逐项讲解 BIOS 与电源设置、SMT、睿频、performance 调频策略、使用 `isolcpus`/`nohz_full` 进行核心隔离、中断（IRQ）亲和性、关闭 swap/THP/NUMA 自动均衡/安全缓解措施、缓存分区（Intel CAT）、`mlockall`、大页（hugepages），以及规避 TLB shootdown。每条建议都解释了它所消除的抖动（jitter）机制，配套的 `hiccups` 工具还能让你实测调优后的操作系统抖动。虽然写于 2020 年，但绝大部分内容至今依然适用。

## 实时抢占的终局（The Realtime Preemption End Game，LWN） {#lwn-preempt-rt}

**来源：** [lwn.net](https://lwn.net/Articles/989212/) · <span class="lvl lvl-i">进阶</span> · 2024 年 9 月

LWN 对 PREEMPT_RT 最终合入主线 Linux 6.12 的权威报道，为长达约二十年的树外实时补丁画上句号。文章解释了实时抢占的机制（可睡眠自旋锁、线程化中断）、为何 `printk` 重写是最后一个障碍，以及合入主线的实际意义：任何原生 6.12+ 内核在 x86/ARM64/RISC-V 上都可以启用完整 RT 抢占编译，驱动在树内维护、无需再打补丁。这是每一位交易系统构建者在 2026 年选择内核时都应了解的背景。

## SUSE Labs — CPU 隔离系列（Frederic Weisbecker） {#suse-cpu-isolation}

**来源：** [suse.com](https://www.suse.com/c/cpu-isolation-practical-example-part-5/) · <span class="lvl lvl-a">高级</span> · 2021–2022 年

由 `nohz_full` 的内核维护者撰写的六篇深度系列，剖析 CPU 隔离在内核内部到底做了什么：full-dynticks 的内部机制、残留的 1 Hz 时钟滴答、housekeeping CPU、`rcu_nocbs` 卸载，以及配置并验证一个完全隔离 CPU 的完整实操示例（含 TSC/时钟源问题排查）。各种低延迟部署清单里照抄的那些内核参数，其背后的权威解释就在这里。作者正是其 CPU 隔离文档在 2026 年被合入主线内核管理员指南的那位工程师。

## HFT 中的 CPU 隔离：`isolcpus` 没有做到的事 {#isolcpus-hft}

**来源：** [nikhilpadala.com](https://nikhilpadala.com/blog/cpu-optimization-linux-latency/) · <span class="lvl lvl-i">进阶</span> · 2024 年 12 月

一篇较新的实践者文章，破除了"单靠 `isolcpus` 就能隔离核心"的误解：它只是把核心从调度器负载均衡中移除，却挡不住定时器滴答和 RCU 回调，它们仍会注入抖动（jitter）。文章用真实的交易基础设施案例——"已隔离"核心上每 4 ms 出现一次无法解释的 200 µs 尖刺——引出完整的 `isolcpus` + `nohz_full` + `rcu_nocbs` 配方，附有逐步配置与验证命令，并坦诚讨论了代价（损失核心、调试更难）。

## TuneD — Linux 调优守护进程 {#tuned}

**来源：** [tuned-project.org](https://tuned-project.org/) · [GitHub](https://github.com/redhat-performance/tuned) · <span class="lvl lvl-b">入门</span> · 持续维护

RHEL、Fedora 及其衍生发行版使用的基于配置文件的调优服务，也是 `latency-performance`、`network-latency`、`realtime` 和 `cpu-partitioning` 等配置文件背后的机制。`cpu-partitioning` 配置文件如今是 Red Hat 为延迟敏感型工作负载推荐的起点，因为它自动完成核心隔离、`nohz_full`、中断（IRQ）亲和性和 housekeeping CPU 分配。直接阅读 GitHub 上的配置文件源码，是吸收 Red Hat 多年积累的低延迟默认值的最快方式，胜过手工抄录 sysctl 片段。

## RHEL Performance Guide（Marko Myllynen） {#myllynen}

**来源：** [GitHub](https://github.com/myllynen/rhel-performance-guide) · <span class="lvl lvl-i">进阶</span> · 维护于 2021–2025 年

一份简明、有主见、由社区维护的 RHEL 9 时代性能调优指南，覆盖 CPU（调频策略、C-state、隔离、启动参数）、内存（大页（hugepages）、NUMA、swap）、I/O 和网络调优，并为每个领域给出监控命令。它的价值在于把零散的官方文档汇总成一页可操作的内容，并明确警示哪些可调项*不*该盲目套用。是从入门清单迈向完整 Red Hat 文档体系的良好桥梁。

## Brendan Gregg — Linux Performance {#gregg}

**来源：** [brendangregg.com](https://www.brendangregg.com/linuxperf.html) · <span class="lvl lvl-b">入门</span> → <span class="lvl lvl-a">高级</span> · 更新至 2026 年

Linux 性能可观测性的核心参考枢纽：`perf`、ftrace 与 eBPF/bpftrace 的工具图谱、火焰图、USE 方法、off-CPU 分析，以及"60 秒 Linux 性能分析"。它还链接了 Gregg 的两本标准教材——《Systems Performance》（第 2 版，2020）和《BPF Performance Tools》（2019）——它们定义了"先测量延迟真正来自哪里、再动手调优"的现代实践。基于 eBPF 的追踪已取代猜测，成为交易公司诊断调度抖动和网络协议栈延迟的标准手段。

## TLB Shootdowns：如何规避或化解（JabPerf） {#tlb-shootdowns}

**来源：** [jabperf.com](https://www.jabperf.com/how-to-deter-or-disarm-tlb-shootdowns/) · <span class="lvl lvl-a">高级</span> · 2021 年 10 月

讲解一个大多数调优清单都会遗漏的延迟来源：多线程进程中 `munmap`/`madvise`/`mprotect` 触发的 TLB shootdown 处理器间中断，它甚至能冻结"已隔离"的核心。基准测试显示，随着线程数增长，一个工作负载从 342 ms 恶化到 16.4 s；一则交易基础设施实战故事则展示了内核升级叠加 NUMA 自动均衡如何造成生产环境中的神秘抖动。对策包括：内存预分配、选择合适的分配器（jemalloc），以及 Intel 的硬件 Remote Action Request 特性。

## PREEMPT_RT 入门（Getting Started with PREEMPT_RT，Realtime Linux 项目） {#preempt-rt-guide}

**来源：** [realtime-linux.org](https://realtime-linux.org/getting-started-with-preempt_rt-guide/) · <span class="lvl lvl-b">入门</span> · 2024–2025 年

上游社区为已进入主线的 PREEMPT_RT 内核编写的入门指南：如何启用 `CONFIG_PREEMPT_RT`、RT 之下有哪些变化（线程化 IRQ、可睡眠锁、优先级继承）、如何编写并调度实时应用线程（`SCHED_FIFO`、优先级、`mlockall`），以及用 `cyclictest` 和 `rtla` 测量延迟。在得知 RT 已合入主线 6.12+ 之后、在决定采用 RHEL-RT 或自行编译内核之前，这是顺理成章的第一篇读物。

## Red Hat Summit 2018 — 性能分析与调优（幻灯片） {#rh-summit}

**来源：** [PDF](https://www.redhat.com/files/summit/session-assets/2018/Performance-analysis-and-tuning-of-Red-Hat-Enterprise-Linux-Part-1.pdf) · <span class="lvl lvl-i">进阶</span> · 2018 年（历史资料）

Red Hat 长期在 Summit 上开讲的性能分析与调优系列演讲的幻灯片。原版视频已经下线，但幻灯片留存了下来，依然展示了 Red Hat 性能工程师的思考方式：先测量，以 tuned 配置文件为基线，再逐一攻克 NUMA 布局、大页（hugepages）和中断（IRQ）安置。作为历史参考保留；现行资料请使用上面的 RHEL 9 文档。

## RHEL 7 低延迟性能调优（Low Latency Performance Tuning for RHEL 7，历史 PDF） {#rhel7-legacy}

**来源：** [PDF](https://access.redhat.com/sites/default/files/attachments/201501-perf-brief-low-latency-tuning-rhel7-v2.1.pdf) · <span class="lvl lvl-i">进阶</span> · 2015–2017 年（历史资料 — RHEL 7 已 EOL）

本站最初链接的那篇如今已置于付费墙后的知识库文章的免费 PDF 版本。它涵盖 RHEL 7 时代实践的 tuned 配置文件、C-state 与 P-state 控制、使用 `numactl` 进行 NUMA 绑定、中断（IRQ）亲和性、内核定时器滴答选项和网络可调参数。作为明确标注的历史参考保留：这些技术显然是当今实践的前身，但 RHEL 7 已于 2024 年 6 月结束维护——新系统请依照 RHEL 9/10 指南构建。
