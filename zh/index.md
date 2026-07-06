---
title: "低延迟系统"
description: "为构建低延迟金融交易系统精心整理、持续更新的资源合集。"
lang: zh
nav: home
permalink: /zh/
translation: /
---

# 低延迟系统

一份精心整理的**低延迟系统**构建资源合集，聚焦金融交易领域。下方每条参考资料都带有一个 <a class="sum" href="/zh/guide/">摘要</a> 徽章，链接到该资料的通俗摘要，并配有级别标签——<span class="lvl lvl-b">入门</span> <span class="lvl lvl-i">进阶</span> <span class="lvl lvl-a">高级</span>——让你在点击之前就能判断要不要读。

> **初次接触这个领域？** 请先读 **[完整指南](/zh/guide/)**——用一个页面把整个主题从入门到专家梳理清楚：延迟是什么、纳秒都花在了哪里、每项技术分别能换来多少收益。

*内容已于 2026 年 7 月全面更新。本次更新背后的调研——逐一核查每个链接、替换失效链接、补充最新的前沿参考——记录在[研究笔记](/research/)（英文）中。*

## 从这里开始 {#start-here}

- [Tick-to-Trade Latency](https://databento.com/microstructure/tick-to-trade) [摘要](/zh/summaries/networking/#tick-to-trade){:.sum} <span class="lvl lvl-b">入门</span> — 这个指标的含义，以及 2024 年前后软件、FPGA、ASIC 三个层级的参考数字。
- [Kernel Bypass](https://blog.cloudflare.com/kernel-bypass/) [摘要](/zh/summaries/networking/#cf-kernel-bypass){:.sum} <span class="lvl lvl-b">入门</span> — 关于交易系统为何以及如何绕过内核的最清晰的入门介绍。
- [What Every Programmer Should Know about How CPUs Work — Matt Godbolt](https://www.youtube.com/watch?v=-HNpim5x-IE) [摘要](/zh/summaries/software/#godbolt-cpu){:.sum} <span class="lvl lvl-b">入门</span> — 目前最好的 43 分钟"机械同情（mechanical sympathy）"入门讲座。
- [What is Low Latency C++? — Timur Doumler（共 2 部分）](https://www.youtube.com/watch?v=EzmNeAhWqVs) [摘要](/zh/summaries/software/#doumler){:.sum} <span class="lvl lvl-i">进阶</span> — 对整个主题最好的结构化教学式讲解。

## Linux 操作系统调优 {#linux-os-tuning}

- [Monitoring and Managing System Status and Performance (RHEL 9)](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html-single/monitoring_and_managing_system_status_and_performance/index) [摘要](/zh/summaries/linux-tuning/#rhel-perf){:.sum} <span class="lvl lvl-b">入门</span> — 已退役的 RHEL 7 性能调优指南的现代继任者。
- [Optimizing RHEL for Real Time for Low Latency Operation](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux_for_real_time/9/html-single/optimizing_rhel_9_for_real_time_for_low_latency_operation/index) [摘要](/zh/summaries/linux-tuning/#rhel-rt){:.sum} <span class="lvl lvl-i">进阶</span> — Red Hat 权威的低延迟指南；自 RHEL 9 起，实时内核已随标准版 RHEL 一同提供。
- [Erik Rigtorp — Low Latency Tuning Guide（低延迟调优指南）](https://rigtorp.se/low-latency-guide/) [摘要](/zh/summaries/linux-tuning/#rigtorp-guide){:.sum} <span class="lvl lvl-i">进阶</span> — 被引用最多的 HFT 风格 x86_64 调优实践清单。
- [TuneD — Linux 调优守护进程](https://tuned-project.org/) [摘要](/zh/summaries/linux-tuning/#tuned){:.sum} <span class="lvl lvl-b">入门</span> — `latency-performance`、`network-latency` 和 `cpu-partitioning` 等配置文件的官方主页。
- [Getting Started with PREEMPT_RT](https://realtime-linux.org/getting-started-with-preempt_rt-guide/) [摘要](/zh/summaries/linux-tuning/#preempt-rt-guide){:.sum} <span class="lvl lvl-b">入门</span> — 实时抢占自 Linux 6.12（2024 年）起已进入主线内核；这份指南教你怎么用。
- [The Realtime Preemption End Game (LWN)](https://lwn.net/Articles/989212/) [摘要](/zh/summaries/linux-tuning/#lwn-preempt-rt){:.sum} <span class="lvl lvl-i">进阶</span> — PREEMPT_RT 历经 20 年最终合入主线的来龙去脉。
- [SUSE Labs — CPU Isolation 系列](https://www.suse.com/c/cpu-isolation-practical-example-part-5/) [摘要](/zh/summaries/linux-tuning/#suse-cpu-isolation){:.sum} <span class="lvl lvl-a">高级</span> — `nohz_full` 的维护者亲自讲解 CPU 隔离在内核里究竟做了什么。
- [CPU Isolation for HFT: what `isolcpus` doesn't do](https://nikhilpadala.com/blog/cpu-optimization-linux-latency/) [摘要](/zh/summaries/linux-tuning/#isolcpus-hft){:.sum} <span class="lvl lvl-i">进阶</span> — 一个交易场景的案例研究，讲透 `isolcpus` + `nohz_full` + `rcu_nocbs` 的完整配方。
- [RHEL Performance Guide (Myllynen)](https://github.com/myllynen/rhel-performance-guide) [摘要](/zh/summaries/linux-tuning/#myllynen){:.sum} <span class="lvl lvl-i">进阶</span> — 把散落各处的官方文档浓缩成一页可直接照做的指南。
- [Brendan Gregg — Linux Performance](https://www.brendangregg.com/linuxperf.html) [摘要](/zh/summaries/linux-tuning/#gregg){:.sum} <span class="lvl lvl-b">入门</span> — 可观测性的大本营：perf、ftrace、eBPF、火焰图，以及该领域的标准教材。
- [TLB Shootdowns (JabPerf)](https://www.jabperf.com/how-to-deter-or-disarm-tlb-shootdowns/) [摘要](/zh/summaries/linux-tuning/#tlb-shootdowns){:.sum} <span class="lvl lvl-a">高级</span> — 大多数调优清单都漏掉的延迟来源。
- 历史资料：[Low Latency Tuning for RHEL 7 (PDF)](https://access.redhat.com/sites/default/files/attachments/201501-perf-brief-low-latency-tuning-rhel7-v2.1.pdf) [摘要](/zh/summaries/linux-tuning/#rhel7-legacy){:.sum} · [Red Hat Summit 2018 幻灯片 (PDF)](https://www.redhat.com/files/summit/session-assets/2018/Performance-analysis-and-tuning-of-Red-Hat-Enterprise-Linux-Part-1.pdf) [摘要](/zh/summaries/linux-tuning/#rh-summit){:.sum} — RHEL 7 已停止支持；保留仅作背景参考。

## 服务器硬件与 CPU {#server-hardware--cpus}

自本站上次更新以来，格局已经改变：HPE 停产了其超频的"Trade and Match"服务器系列，精品厂商（Blackcore、Hypertec CIARA）如今占据了这个细分市场。在主流阵营，AMD EPYC 9005 F 系列（如 9575F）、带 3D V-Cache 的 EPYC 4004/4005 X3D 型号，以及配备 Latency Optimized Mode 的 Intel Xeon 6 是当前的选择。单路（single-socket）已是延迟路径上的定论架构，而核间延迟（core-to-core latency）如今已是选择 CPU 时的一等指标。

- [AMD EPYC 9005 BIOS & Workload Tuning Guide](https://docs.amd.com/v/u/en-US/58467_amd-epyc-9005-tg-bios-and-workload) [摘要](/zh/summaries/hardware/#amd-9005-tuning){:.sum} <span class="lvl lvl-a">高级</span> — 当前 AMD 服务器调优的权威文档（rev 2.2，2026 年 6 月）。
- [Dell PowerEdge 16G BIOS Tuning — Low Latency](https://infohub.delltechnologies.com/en-us/l/poweredge-bios-performance-and-workload-tuning-guide-for-16g/low-latency-17/) [摘要](/zh/summaries/hardware/#dell-16g){:.sum} <span class="lvl lvl-i">进阶</span> — 取代了 Dell 那份已失效的 2010 年低延迟白皮书。
- [Configuring HPE ProLiant Servers for Low-Latency Applications](https://www.hpe.com/psnow/doc/c01804533) [摘要](/zh/summaries/hardware/#hpe-low-latency){:.sum} <span class="lvl lvl-i">进阶</span> — HPE 长期维护的白皮书搬到了新地址，已更新至 Gen11。
- [Intel Xeon 6 Performance & Power Profiles (PDF)](https://builders.intel.com/docs/networkbuilders/intel-xeon-6-processors-performance-and-power-profiles-1746785299.pdf) [摘要](/zh/summaries/hardware/#intel-xeon6-profiles){:.sum} <span class="lvl lvl-a">高级</span> — Latency Optimized Mode 与 Efficiency Latency Control，来自 Intel 官方。
- [Phoronix — Xeon 6 Latency Optimized Mode 基准测试](https://www.phoronix.com/review/intel-latency-optimized-mode) [摘要](/zh/summaries/hardware/#phoronix-lom){:.sum} <span class="lvl lvl-i">进阶</span> — 独立测量数据：几何平均约 17% 的提升。
- [Xeon 6 High-Priority Cores (SST-BF) Explained](https://www.servethehome.com/intel-xeon-6-high-priority-and-low-priority-cores-explained/) [摘要](/zh/summaries/hardware/#sth-sst){:.sum} <span class="lvl lvl-b">入门</span> — 为你的热点线程提供有保障的高频核心。
- [Chips and Cheese — Xeon 6 内存子系统](https://chipsandcheese.com/p/a-look-into-intel-xeon-6s-memory) [摘要](/zh/summaries/hardware/#chips-cheese-xeon6){:.sum} <span class="lvl lvl-a">高级</span> — 实测的 L3/DRAM/核间延迟，Intel mesh 对比 AMD chiplet。
- [core-to-core-latency](https://github.com/nviennot/core-to-core-latency) [摘要](/zh/summaries/hardware/#c2c-latency){:.sum} <span class="lvl lvl-i">进阶</span> — 测量你的 CPU 拓扑；附社区测量结果数据库。
- [Blackcore — 为什么要用超频交易服务器](https://www.blackcoretech.com/knowledge/maximize-your-trading-potential-embrace-overclocking-to-stay-ahead-of-the-curve) [摘要](/zh/summaries/hardware/#blackcore){:.sum} <span class="lvl lvl-b">入门</span> — 精品服务器厂商的视角（厂商内容）。
- [hft-server-settings](https://github.com/twanas/hft-server-settings) [摘要](/zh/summaries/hardware/#hft-server-settings){:.sum} <span class="lvl lvl-i">进阶</span> — 一份社区清单，收录从业者真正会改动的那些设置。
- 历史资料：[Intel "Optimizing Applications for Latency" 第 1、2 部分](https://www.intel.com/content/www/us/en/developer/articles/technical/optimizing-computer-applications-for-latency-part-1-configuring-the-hardware.html) [摘要](/zh/summaries/hardware/#intel-latency-articles){:.sum} · [IBM/Lenovo Linux 调优 Redpaper（2008）](https://lenovopress.lenovo.com/redp4285) [摘要](/zh/summaries/hardware/#lenovo-redp){:.sum}

## 网络、网卡与内核旁路 {#networking-nics--kernel-bypass}

厂商版图已被重绘：Solarflare → Xilinx → **AMD**（X4 系列网卡，2025 年 10 月），Mellanox → **NVIDIA**（VMA 被 XLIO 取代），Exablaze → **Cisco**（随后于 2023 年退出超低延迟交换机业务，让 **Arista 7130** 成为事实上的 layer-1 平台）。

### 先理解内核路径

- [Understanding TCP/IP Network Stack (CUBRID)](https://www.cubrid.org/blog/3826497) [摘要](/zh/summaries/networking/#cubrid){:.sum} <span class="lvl lvl-b">入门</span> — 建立心智模型：一个数据包如何穿过 Linux。
- [Monitoring and Tuning the Linux Networking Stack (packagecloud)](https://blog.packagecloud.io/monitoring-tuning-linux-networking-stack-receiving-data/) [摘要](/zh/summaries/networking/#packagecloud){:.sum} <span class="lvl lvl-a">高级</span> — 对接收路径（RX path）最权威的源码级逐步剖析。
- [Linux Network Performance Ultimate Guide (ntk148v)](https://ntk148v.github.io/posts/linux-network-performance-ultimate-guide/) [摘要](/zh/summaries/networking/#ntk148v){:.sum} <span class="lvl lvl-i">进阶</span> — 一个持续维护的页面，整合了所有经典调优资料。
- [How to Receive a Million Packets Per Second (Cloudflare)](https://blog.cloudflare.com/how-to-receive-a-million-packets/) [摘要](/zh/summaries/networking/#cf-million-packets){:.sum} <span class="lvl lvl-i">进阶</span> — RSS、`SO_REUSEPORT` 与 NUMA 绑定，全程实操。
- [Why We Use the Linux Kernel's TCP Stack (Cloudflare)](https://blog.cloudflare.com/why-we-use-the-linux-kernels-tcp-stack/) [摘要](/zh/summaries/networking/#cf-kernel-tcp){:.sum} <span class="lvl lvl-b">入门</span> — *反对*把一切都绕过内核的理由。
- [Extreme HTTP Performance Tuning (talawah.io)](https://talawah.io/blog/extreme-http-performance-tuning-one-point-two-million/) [摘要](/zh/summaries/networking/#talawah){:.sum} <span class="lvl lvl-a">高级</span> — 不绕过内核实现 5 倍吞吐，每个调优旋钮都用火焰图度量过。
- [Zero Copy I: User-Mode Perspective](https://www.linuxjournal.com/article/6345) [摘要](/zh/summaries/networking/#zero-copy){:.sum} <span class="lvl lvl-i">进阶</span> — 2003 年的经典之作，是今天各种零拷贝（zero-copy）技术的源头。

### 内核旁路协议栈

- [Onload User Guide (AMD UG1586)](https://docs.amd.com/r/en-US/ug1586-onload-user) [摘要](/zh/summaries/networking/#onload-guide){:.sum} <span class="lvl lvl-a">高级</span> — Onload、ef_vi 和 TCPDirect：整个 Solarflare 协议栈，仍在积极维护。
- [OpenOnload on GitHub](https://github.com/Xilinx-CNS/onload) [摘要](/zh/summaries/networking/#openonload){:.sum} <span class="lvl lvl-i">进阶</span> — 2021 年起开源；现在可通过 AF_XDP 运行在非 Solarflare 网卡上。
- [NVIDIA XLIO 文档](https://networking-docs.nvidia.com/xlioswum) [摘要](/zh/summaries/networking/#xlio){:.sum} <span class="lvl lvl-a">高级</span> — Mellanox VMA 的继任者，用于 ConnectX 上的 socket 加速。
- [Rivermax + NEIO FastSockets for financial services](https://developer.nvidia.com/blog/maximizing-low-latency-networking-performance-for-financial-services-with-nvidia-rivermax-and-neio-fastsocket/) [摘要](/zh/summaries/networking/#rivermax){:.sum} <span class="lvl lvl-i">进阶</span> — NVIDIA 2025 年面向行情（market data）工作负载的答卷。

### 网卡

- [AMD Solarflare X4 发布](https://www.amd.com/en/blogs/2025/new-amd-solarflare-x4-low-latency-ethernet-adapter-set.html) [摘要](/zh/summaries/networking/#solarflare-x4){:.sum} <span class="lvl lvl-b">入门</span> — 当前的旗舰交易网卡家族（2025 年 10 月）。
- [Performance Tuning for Mellanox Adapters (NVIDIA)](https://enterprise-support.nvidia.com/s/article/performance-tuning-for-mellanox-adapters) [摘要](/zh/summaries/networking/#mellanox-tuning){:.sum} <span class="lvl lvl-a">高级</span> — 经典 Mellanox 调优 PDF 的继任文档。
- [Cisco Nexus SmartNIC（前 ExaNIC）](https://www.cisco.com/c/en/us/products/collateral/interfaces-modules/nexus-smartnic/datasheet-c78-743827.html) [摘要](/zh/summaries/networking/#exanic){:.sum} <span class="lvl lvl-i">进阶</span> — 568 ns 的触发到响应（trigger-to-response）；现已进入维护模式。

### 交换机

- [Arista 7130 系列](https://www.arista.com/en/products/7130-series) [摘要](/zh/summaries/networking/#arista-7130){:.sum} <span class="lvl lvl-i">进阶</span> — layer-1（约 4 ns）、MetaMux（约 39 ns）、MetaWatch；事实上的超低延迟交换平台。
- [Measuring the Latency of a 4 ns Switch (PDF)](https://www.arista.com/assets/data/pdf/Latency-4ns-Switch-Solution-Brief.pdf) [摘要](/zh/summaries/networking/#arista-4ns){:.sum} <span class="lvl lvl-a">高级</span> — 如何测量低于测试仪器分辨率的延迟。

### RDMA 与 InfiniBand

- [RDMA Aware Networks Programming User Manual](https://networking-docs.nvidia.com/rdmaawareprogramming) [摘要](/zh/summaries/networking/#rdma-manual){:.sum} <span class="lvl lvl-a">高级</span> — verbs、队列对（queue pair）与 `rdma_cm`，已迁至 NVIDIA 的新地址。注意：在交易场景中，RDMA 如今指*内部*网络上的 RoCEv2；面向交易所的路径是以太网 UDP 组播。

## FPGA 与硬件加速 {#fpga--hardware-acceleration}

- [AMD Alveo UL3524 产品简介 (PDF)](https://www.xilinx.com/content/dam/xilinx/publications/product-briefs/2233051_Product_Brief_UL3524_Alveo_Accelerator_Card.pdf) [摘要](/zh/summaries/fpga/#ul3524){:.sum} <span class="lvl lvl-i">进阶</span> — 第一款专为交易打造的 FPGA 卡：<3 ns 收发器。
- [Exegy + AMD：13.9 ns 的 STAC-T0 纪录](https://www.exegy.com/exegy-amd-new-record/) [摘要](/zh/summaries/fpga/#stac-t0-record){:.sum} <span class="lvl lvl-i">进阶</span> — 2024 年的 tick-to-trade 世界纪录。
- [Where next for ultra-low latency trading? (A-Team)](https://a-teaminsight.com/blog/as-the-latest-fpga-technology-from-amd-sets-the-gold-standard-where-next-for-ultra-low-latency-trading/) [摘要](/zh/summaries/fpga/#ateam-fpga){:.sum} <span class="lvl lvl-b">入门</span> — 从战略视角比较软件、FPGA 与 ASIC。
- [Orthogone — HFT FPGA 工程](https://orthogone.com/industries/high-frequency-trading-finance/) [摘要](/zh/summaries/fpga/#orthogone){:.sum} <span class="lvl lvl-i">进阶</span> — 一套现代 FPGA 交易栈，附真实延迟数字。
- [FPGA trading with Nasdaq ITCH & OUCH (Design Gateway)](https://dgway.com/blog_E/2025/12/18/ultra-low-latency-fpga-trading-with-nasdaq-developed-itch-ouch/) [摘要](/zh/summaries/fpga/#dgway-itch-ouch){:.sum} <span class="lvl lvl-i">进阶</span> — 一条全硬件的 tick-to-trade 流水线，逐个协议讲解。
- [Algo-Logic](https://www.algo-logic.com/) [摘要](/zh/summaries/fpga/#algo-logic){:.sum} <span class="lvl lvl-b">入门</span> — 深耕金融 FPGA 领域多年的厂商，至今活跃；Lockwood 的经典幻灯片见 [Wayback Machine](https://web.archive.org/web/20181221085356/http://www.hoti.org/hoti20/slides/Lockwood_AlgoLogic.pdf)。

## 时钟同步与时间戳 {#time-synchronization--timestamping}

- [How PTP is deployed at Meta](https://engineering.fb.com/2022/11/21/production-engineering/precision-time-protocol-at-meta/) [摘要](/zh/summaries/timing/#meta-ptp){:.sum} <span class="lvl lvl-i">进阶</span> — 超大规模下数十纳秒级的同步，整套栈完全开源。
- [OCP Time Appliances Project — Time Card](https://github.com/Time-Appliances-Project/Time-Card) [摘要](/zh/summaries/timing/#time-card){:.sum} <span class="lvl lvl-a">高级</span> — 开放硬件，把任何服务器变成 stratum-1 时间设备。
- [White Rabbit (CERN)](https://white-rabbit.web.cern.ch/) [摘要](/zh/summaries/timing/#white-rabbit){:.sum} <span class="lvl lvl-i">进阶</span> — 亚纳秒级授时，现已成为 IEEE 1588-2019 High Accuracy profile。
- [Deutsche Börse / Eurex White Rabbit 服务](https://www.eurex.com/ex-en/support/initiatives/archive/high-precision-time-white-rabbit-pilot) [摘要](/zh/summaries/timing/#eurex-wr){:.sum} <span class="lvl lvl-b">入门</span> — 交易所如何把自己的时钟分发给托管（co-location）客户。
- [linuxptp 文档](https://linuxptp.nwtime.org/documentation/) [摘要](/zh/summaries/timing/#linuxptp){:.sum} <span class="lvl lvl-i">进阶</span> — ptp4l、phc2sys、ts2phc：标准的 Linux PTP 栈。
- [MiFID II RTS 25 and time sync on Linux (Red Hat)](https://www.redhat.com/en/blog/mifid-ii-rts-25-and-time-synchronisation-red-hat-enterprise-linux-and-red-hat-virtualization) [摘要](/zh/summaries/timing/#mifid-rts25){:.sum} <span class="lvl lvl-b">入门</span> — 把监管要求逐条映射到 Linux 的能力上。
- [Real-world PTP accuracy across NICs (scottstuff.net)](https://scottstuff.net/posts/2025/05/07/connectx-4-and-linuxptp/) [摘要](/zh/summaries/timing/#nic-ptp-accuracy){:.sum} <span class="lvl lvl-a">高级</span> — 2025 年实测数据：网卡选择能让 PTP 精度相差 45 倍。
- [Intel rdtsc 基准测试白皮书（镜像）](https://cis.temple.edu/~qzeng/cis3207-spring18/files/ia-32-ia-64-benchmark-code-execution-paper.pdf) [摘要](/zh/summaries/timing/#rdtsc-paper){:.sum} <span class="lvl lvl-a">高级</span> — Paoloni 关于时钟周期级精确计时的经典论文（Intel 已删除原文）。

## 测量、基准测试与抖动检测 {#measurement-benchmarks--jitter-detection}

- [STAC 基准测试](https://docs.stacresearch.com/) [摘要](/zh/summaries/tools/#stac){:.sum} <span class="lvl lvl-i">进阶</span> — STAC-T0/N1/TS：经审计的行业基准，附完整配置披露。
- [cns-sysjitter](https://github.com/Xilinx-CNS/cns-sysjitter) [摘要](/zh/summaries/tools/#sysjitter){:.sum} <span class="lvl lvl-a">高级</span> — Solarflare 经典的 OS 抖动测量工具，已迁至新的 GitHub 地址。
- [cyclictest & rt-tests](https://man.archlinux.org/man/cyclictest.8.en) [摘要](/zh/summaries/tools/#cyclictest){:.sum} <span class="lvl lvl-i">进阶</span> — 标准的实时延迟测试套件；在 5.17+ 内核上可搭配内核自带的 `rtla` 追踪器使用。
- [hiccups](https://github.com/erez-strauss/hiccups) [摘要](/zh/summaries/tools/#hiccups){:.sum} <span class="lvl lvl-a">高级</span> — 一个简单的 OS 卡顿（hiccup）测量工具（已停止维护，但一口气就能读完源码）。
- [sockperf](https://github.com/Mellanox/sockperf) [摘要](/zh/summaries/tools/#sockperf){:.sum} <span class="lvl lvl-b">入门</span> — socket 层的延迟基准测试，带百分位直方图。
- [Corvil Analytics (Pico)](https://www.pico.net/products/corvil-analytics/) [摘要](/zh/summaries/tools/#corvil){:.sum} <span class="lvl lvl-b">入门</span> — 生产级纳秒延迟监控长什么样。
- [c2clat](https://github.com/rigtorp/c2clat) & [core-to-core-latency](https://github.com/nviennot/core-to-core-latency) [摘要](/zh/summaries/tools/#c2clat){:.sum} <span class="lvl lvl-i">进阶</span> — 在决定绑核布局之前，先摸清核间延迟特性。
- [Google Benchmark](https://github.com/google/benchmark) [摘要](/zh/summaries/tools/#google-benchmark-tools){:.sum} <span class="lvl lvl-b">入门</span> — 标准的 C++ 微基准测试框架。

## 编程语言与代码 {#programming-languages--code}

### C++

- [When Nanoseconds Matter — David Gross（CppCon 2024 主题演讲）](https://www.youtube.com/watch?v=sX2nF1fW7kI) [摘要](/zh/summaries/software/#gross-2024){:.sum} <span class="lvl lvl-a">高级</span> — 当前最权威的 HFT C++ 演讲。
- [Trading at Light Speed — David Gross（Meeting C++ 2022）](https://www.youtube.com/watch?v=8uAW5FQtcvE) [摘要](/zh/summaries/software/#gross-2022){:.sum} <span class="lvl lvl-a">高级</span> — 与上一场配套的全系统设计演讲。
- [When a Microsecond Is an Eternity — Carl Cook（CppCon 2017）](https://www.youtube.com/watch?v=NH1Tta7purM) [摘要](/zh/summaries/software/#carl-cook){:.sum} <span class="lvl lvl-i">进阶</span> — HFT C++ 的经典入门演讲。
- [What is Low Latency C++? — Timur Doumler（C++Now 2023）](https://www.youtube.com/watch?v=EzmNeAhWqVs) [摘要](/zh/summaries/software/#doumler){:.sum} <span class="lvl lvl-i">进阶</span> — 共两部分；对该主题最好的结构化现代讲解。
- [SPSC Lock-free FIFO From the Ground Up — Charles Frasch（CppCon 2023）](https://www.youtube.com/watch?v=K3P_Lmq6pw0) [摘要](/zh/summaries/software/#frasch-spsc){:.sum} <span class="lvl lvl-i">进阶</span> — 从零构建这个主力队列，每一步都做基准测试。
- [C++ Design Patterns for Low-latency Applications (arXiv)](https://arxiv.org/abs/2309.04259) [摘要](/zh/summaries/software/#hft-paper){:.sum} <span class="lvl lvl-a">高级</span> — 用统计学的严谨态度实测那些 HFT 圈内口口相传的经验。
- [Google Benchmark](https://github.com/google/benchmark) [摘要](/zh/summaries/software/#google-benchmark){:.sum} <span class="lvl lvl-b">入门</span> — 用数据证明你的优化真的有效。

### 并发与无锁（lock-free）

- [C++ Atomics, From Basic to Advanced — Fedor Pikus（CppCon 2017）](https://www.youtube.com/watch?v=ZQFzMfHIxng) [摘要](/zh/summaries/software/#pikus-atomics){:.sum} <span class="lvl lvl-a">高级</span> — 原子操作到底做了什么；至今仍是权威。
- [Lock-Free Programming — Herb Sutter（CppCon 2014，第 I、II 部分）](https://www.youtube.com/watch?v=c1gO9aB9nbs) [摘要](/zh/summaries/software/#sutter-lockfree){:.sum} <span class="lvl lvl-a">高级</span> — 概念基础（第 II 部分链接已修复——旧链接被截断了）。
- [Erik Rigtorp — 文章与代码库](https://rigtorp.se/) [摘要](/zh/summaries/software/#rigtorp){:.sum} <span class="lvl lvl-a">高级</span> — SPSCQueue、MPMCQueue、Seqlock，以及 awesome-lockfree 清单。
- [1024cores — Dmitry Vyukov](https://sites.google.com/site/1024cores/) [摘要](/zh/summaries/software/#vyukov-1024cores){:.sum} <span class="lvl lvl-a">高级</span> — **旧的 1024cores.net 域名已被劫持；请使用这个 Google Sites 地址。**

### Java

- [LMAX Disruptor](https://github.com/LMAX-Exchange/disruptor) & [Chronicle Queue](https://github.com/OpenHFT/Chronicle-Queue) [摘要](/zh/summaries/software/#disruptor-chronicle){:.sum} <span class="lvl lvl-i">进阶</span> — 无 GC 的 Java 工具箱。
- [Aeron](https://github.com/aeron-io/aeron) [摘要](/zh/summaries/software/#aeron){:.sum} <span class="lvl lvl-a">高级</span> — 低延迟消息传递与集群；注意组织已迁移至新的 `aeron-io`。
- [Evolution of Financial Exchange Architectures — Martin Thompson](https://www.youtube.com/watch?v=qDhTjE0XmkE) [摘要](/zh/summaries/software/#thompson-exchange){:.sum} <span class="lvl lvl-i">进阶</span> — 一场演讲讲完 25 年的交易所架构演进。
- [Azul Platform Prime (Zing) 与 C4 收集器](https://docs.azul.com/prime/c4-garbage-collection.html) [摘要](/zh/summaries/software/#azul-zing){:.sum} <span class="lvl lvl-i">进阶</span> — 无停顿的 Java；可与 OpenJDK 免费的分代 ZGC（JDK 21+）对比。
- 历史资料：[SubMicroTrading 博客](https://submicro.blogspot.com) + [开源代码](https://github.com/gsitgithub/SubMicroTrading) [摘要](/zh/summaries/software/#submicro){:.sum} · [epickrram（LMAX）关于抖动的文章](https://epickrram.blogspot.com/2015/09/reducing-system-jitter.html) [摘要](/zh/summaries/software/#epickrram){:.sum}

### Rust

- [Rust vs C++ for trading systems (Databento)](https://databento.com/blog/rust-vs-cpp) [摘要](/zh/summaries/software/#rust-vs-cpp){:.sum} <span class="lvl lvl-i">进阶</span> — 2026 年年中业界脚踏实地的共识。
- [hftbacktest](https://github.com/nkaz001/hftbacktest) [摘要](/zh/summaries/software/#hftbacktest){:.sum} <span class="lvl lvl-a">高级</span> — 旗舰级的开源 Rust HFT 项目。

### 社区与协议

- [Mechanical Sympathy Google Group](https://groups.google.com/g/mechanical-sympathy) [摘要](/zh/summaries/software/#mech-sympathy){:.sum} <span class="lvl lvl-i">进阶</span> — 已不复当年的热闹，但存档是金矿。
- [Databento Microstructure Guides](https://databento.com/microstructure/itch) [摘要](/zh/summaries/software/#databento-micro){:.sum} <span class="lvl lvl-b">入门</span> — ITCH、OUCH、MoldUDP64 与 tick-to-trade，讲得明明白白。

## 参与贡献 {#contribute}

发现失效链接，或有值得收录的资源？欢迎[提交 GitHub issue](https://github.com/datar/low-latency-system/issues)。

## 关于我 {#about-me}

- [我的 LinkedIn 主页](https://www.linkedin.com/in/masterchen/)
- [chenxing@live.com](mailto:chenxing@live.com)
- 微信：trador
