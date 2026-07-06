---
title: "参考摘要 — 服务器硬件与 CPU"
description: "本站收录的服务器硬件与 CPU 调优参考资料的通俗摘要。"
permalink: /zh/summaries/hardware/
translation: /summaries/hardware/
lang: zh
nav: home
---

# 参考资料摘要：服务器硬件与 CPU

[← 返回资源列表](/zh/#server-hardware--cpus) · 下面每一条摘要对应收录中的一份参考资料，帮助你判断它是否值得花时间阅读。

---

## AMD EPYC 9005 BIOS 与工作负载调优指南（Pub 58467） {#amd-9005-tuning}

**来源：** [docs.amd.com](https://docs.amd.com/v/u/en-US/58467_amd-epyc-9005-tg-bios-and-workload) · <span class="lvl lvl-a">高级</span> · rev 2.2，2026 年 6 月

AMD 针对第五代 EPYC "Turin"（9005 系列）服务器的官方调优指南——接替了本站过去链接的、已下线的 2018 年 developer.amd.com 调优 PDF。它逐项讲解 SP5 平台上每一个与延迟相关的 BIOS 选项：性能/功耗确定性模式、C-state 控制、SMT、NUMA-per-socket（NPS）模式、Infinity Fabric 与内存时钟配置。按工作负载给出的推荐设置表，可直接映射到基于 EPYC 9575F 等芯片的低延迟交易配置。这是当前 AMD 服务器调优的权威文档。

## Dell PowerEdge 16G BIOS 调优指南 — 低延迟 {#dell-16g}

**来源：** [infohub.delltechnologies.com](https://infohub.delltechnologies.com/en-us/l/poweredge-bios-performance-and-workload-tuning-guide-for-16g/low-latency-17/) · <span class="lvl lvl-i">进阶</span> · 2023–2024 年，持续维护

Dell 对其经典的 2010 年白皮书《Configuring Low-Latency Environments on PowerEdge Servers》（原链接已失效）的现代替代。低延迟章节说明应选择哪个 System Profile，以及它如何禁用深度 C-state、锁定 CPU 调频策略并调整内存频率以获得确定性响应；周边章节则提供完整的 BIOS 选项参考，让你理解每项设置在吞吐/功耗/延迟之间的取舍。对于在通用 Dell 服务器上运行交易工作负载，可直接照做。

## 为低延迟应用配置 HPE ProLiant 服务器 {#hpe-low-latency}

**来源：** [hpe.com/psnow](https://www.hpe.com/psnow/doc/c01804533) · <span class="lvl lvl-i">进阶</span> · 更新至 ProLiant Gen11

HPE 长期维护的低延迟白皮书——与本站自 2018 年起链接的是同一个文档 ID，在 HPE 关闭旧支持门户后迁移到了新 URL。内容涵盖 BIOS 工作负载配置文件、禁用电源管理 SMI（HPE 机器上经典的数微秒级抖动来源）、抖动平滑选项，以及操作系统层面的建议。需要注意，HPE 的超频"Trade and Match" Apollo 服务器已停产；这一细分市场如今由精品厂商填补，但调优标准 ProLiant 服务器仍以本指南为准。

## Intel Xeon 6 性能与功耗配置（含 Latency Optimized Mode） {#intel-xeon6-profiles}

**来源：** [builders.intel.com PDF](https://builders.intel.com/docs/networkbuilders/intel-xeon-6-processors-performance-and-power-profiles-1746785299.pdf) · <span class="lvl lvl-a">高级</span> · 2025 年 4 月

Intel 官方技术文章，记录 Xeon 6（Granite Rapids）的性能与功耗配置：出厂默认模式、**Latency Optimized Mode**（保持 uncore/mesh 频率高位运行而非降频）以及 Efficiency Latency Control（ELC）。文章解释了每种配置对核心/uncore 频率行为的影响，以及何时应选择延迟优化设置而非每瓦性能优先的默认值。这是 Intel 已下线的"Optimizing Computer Applications for Latency"系列文章最接近的现代继任者。

## Phoronix — Xeon 6 Latency Optimized Mode 基准测试 {#phoronix-lom}

**来源：** [phoronix.com](https://www.phoronix.com/review/intel-latency-optimized-mode) · <span class="lvl lvl-i">进阶</span> · 2025 年初

对 Intel Latency Optimized Mode 的独立基准测试研究，量化了低延迟从业者过去通过手动锁定 uncore 频率来近似实现的这项设置：在广泛的测试套件中取得约 17% 的几何平均性能提升，功耗代价可以接受。文章还解释了该特性与 ELC 策略及 RAPL/TDP 限制的相互作用。在选型或调优 Xeon 6 交易服务器时的必读资料。

## Xeon 6 高优先级核心（Intel SST-BF）详解 {#sth-sst}

**来源：** [servethehome.com](https://www.servethehome.com/intel-xeon-6-high-priority-and-low-priority-cores-explained/) · <span class="lvl lvl-b">入门</span> · 2024–2025 年

一篇对 Intel Speed Select Base Frequency（SST-BF）的清晰讲解：它让一部分"高优先级"核心以更高的保证基础频率运行，其余核心则运行在较低频率。对延迟敏感型系统而言，这意味着关键交易线程可以绑定到保证高频的核心上，无论负载如何都能获得确定的时钟频率。这也是一篇很好的概念入门，解释了为什么随着核心数膨胀，频率不对称成了一个必不可少的工具。

## Chips and Cheese — Xeon 6 内存子系统深度剖析 {#chips-cheese-xeon6}

**来源：** [chipsandcheese.com](https://chipsandcheese.com/p/a-look-into-intel-xeon-6s-memory) · <span class="lvl lvl-a">高级</span> · 2025 年 9 月

一篇微架构分析，实测了驱动交易系统 CPU 选型的关键数字：Xeon 6 的 L3 延迟（约 33 ns）、本地 DRAM 延迟（约 131 ns）、核间延迟（50–80 ns，即使跨越 EMIB 拼接的计算芯片也是如此）。文章直接对比了 Intel 的 mesh 与 AMD EPYC 的 CCD/Infinity Fabric 设计：跨芯片缓存一致性（核间消息传递）Intel 占优，原生本地内存延迟则 AMD 占优。它说明了核间延迟为何已成为一项一等重要的硬件选型指标。

## core-to-core-latency — 测量你的 CPU 拓扑 {#c2c-latency}

**来源：** [GitHub](https://github.com/nviennot/core-to-core-latency) · <span class="lvl lvl-i">进阶</span> · 2022 年，至今仍是参考数据集

一个开源 Rust 工具，通过缓存一致性操作测量核间通信延迟，并拥有覆盖 Intel、AMD、ARM 和 Apple CPU 的大型社区结果数据库。其热力图直观呈现了在交易系统中影响巨大的拓扑效应：CCX 内约 8–25 ns，跨 CCD 边界 85–180 ns，跨插槽则有 2–3.6 倍的开销。交易公司和超频服务器厂商使用这类工具（连同 rigtorp 的 `c2clat`）来验证线程绑定方案并筛选候选 CPU。

## Blackcore — 为什么超频服务器在交易领域依然制胜 {#blackcore}

**来源：** [blackcoretech.com](https://www.blackcoretech.com/knowledge/maximize-your-trading-potential-embrace-overclocking-to-stay-ahead-of-the-curve) · <span class="lvl lvl-b">入门</span> · 2023–2025 年（厂商内容）

来自超频交易服务器领军厂商的观点，解释为什么超频依然制胜：除了原始时钟频率，与原厂服务器相比，深度调校的系统还能降低内存延迟（宣称约 34%）、缓存访问时间（约 30%）和 PCIe 延迟。其更大的 Knowledge Center 还涵盖体质筛选（binning）、液冷、16 小时烤机以及验证工具（Prime95、Intel MLC、pcie-lat、c2clat）。厂商立场在所难免——但它具体描绘了专为 HFT 打造的服务器与企业级机器的差异所在，而这正是 HPE 停掉 Apollo "Trade and Match" 产品线后放弃的细分市场。

## hft-server-settings — 社区检查清单 {#hft-server-settings}

**来源：** [GitHub](https://github.com/twanas/hft-server-settings) · <span class="lvl lvl-i">进阶</span> · 约 2020–2024 年

一份社区维护的、真实 HFT 部署中实际使用的服务器设置清单：BIOS 项目（禁用超线程和深度 C-state、静态高性能配置、NUMA 指引）、内核启动参数、中断（IRQ）/网卡/套接字对齐以及 CPU 绑定惯例。它的价值在于精炼——把厂商白皮书浓缩成从业者真正会改动的那些设置。是 Rigtorp 和 Red Hat 完整指南的优秀速查伴侣。

## Optimizing Computer Applications for Latency — Intel（第 1、2 部分） {#intel-latency-articles}

**来源：** [Part 1: hardware](https://www.intel.com/content/www/us/en/developer/articles/technical/optimizing-computer-applications-for-latency-part-1-configuring-the-hardware.html) · [Part 2: applications](https://www.intel.com/content/www/us/en/developer/articles/technical/optimizing-computer-applications-for-latency-part-2-tuning-applications.html) · <span class="lvl lvl-i">进阶</span> · 约 2017 年（历史资料）

Intel 的经典两部曲系列文章，在 software.intel.com 关站后迁移到了新地址（本站过去收录的旧 URL 已失效）。第 1 部分讲硬件配置——C-state、睿频、内存插法、BIOS 设置；第 2 部分讲应用调优——用 VTune 剖析、缓存优化与多线程。这些概念至今成立，但针对当前芯片，请结合上面的 Xeon 6 配置文档一起阅读。

## Lenovo/IBM Linux Performance and Tuning Guidelines {#lenovo-redp}

**来源：** [lenovopress.lenovo.com](https://lenovopress.lenovo.com/redp4285) · <span class="lvl lvl-b">入门</span> · 2008 年（历史资料 — 已被取代）

本站多年链接的老 IBM Redpaper，仍可在 Lenovo Press 上访问，但内容定格在 2008 年 4 月（IBM System x、内核 2.6）。仅作历史掌故保留：它的结构（先理解、再测量、然后调优 CPU/内存/磁盘/网络）塑造了一代调优指南，但其中每一条具体建议都已过时。请改用 RHEL 9 文档和上面的厂商指南。
