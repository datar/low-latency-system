---
title: "参考摘要 — 测量、基准测试与抖动检测"
description: "以通俗语言介绍本站收录的测量与基准测试参考资料。"
permalink: /zh/summaries/tools/
translation: /summaries/tools/
lang: zh
nav: home
---

# 参考资料摘要：测量、基准测试与抖动检测

[← 返回资源列表](/zh/#measurement-benchmarks--jitter-detection) · 以下每一条摘要对应本站收录的一篇参考资料，帮助你判断它是否值得花时间阅读。

---

## STAC 基准测试（STAC-T0、STAC-N1、STAC-TS） {#stac}

**来源：** [docs.stacresearch.com](https://docs.stacresearch.com/) · 当前纪录：[AMD240422](https://docs.stacresearch.com/news/AMD240422) · <span class="lvl lvl-i">进阶</span> · 持续运营（STAC 于 2024 年 7 月被 NAG 的母公司 n2 Group 收购）

交易技术领域的行业审计基准权威：STAC-T0 测量 tick-to-trade 网络栈往返延迟（当前纪录 13.9 ns，Exegy/AMD，2024 年），STAC-N1 测量行情数据负载下的主机网络栈，STAC-TS 则覆盖面向 MiFID II 的时间戳/时钟同步合规工具。报告会披露完整的软硬件配置，因此即使你从不亲自跑这些基准，它们也是很有价值的调优参考。需免费注册；本站原先收录的 `stacresearch.com/SFC170206` 链接现已迁移至 `docs.stacresearch.com/SFC170206`。

## cns-sysjitter — 测量操作系统引入的抖动 {#sysjitter}

**来源：** [GitHub（Xilinx-CNS）](https://github.com/Xilinx-CNS/cns-sysjitter) · <span class="lvl lvl-a">高级</span> · 工具约诞生于 2010 年代，仓库现由 AMD/Xilinx 维护

Solarflare 经典工具 `sysjitter`（v1.4，作者 David Riddoch）的开源归宿：它在每个隔离核心上跑一个自旋线程，记录线程被挤下 CPU 的每一个时间段，输出每核的中断次数与直方图。做完 `isolcpus`/`nohz_full` 调优后，这是验证真实效果的基准手段——时钟滴答、IRQ 和 SMI 真的消失了吗？它接替了本站之前链接的、已失效的 openonload.org 下载页。

## hiccups — Erez Strauss 的操作系统卡顿测量工具 {#hiccups}

**来源：** [GitHub](https://github.com/erez-strauss/hiccups) · <span class="lvl lvl-a">高级</span> · 已停止更新

一个测量操作系统"卡顿"（hiccup）——即忙自旋线程遭遇的调度中断——的小工具，思路与 sysjitter 一脉相承。仍然在线但已无人维护；之所以保留，是因为它简单到一次就能读完并按需改造。若需要仍在维护的替代品，可用上面的 cns-sysjitter，或内核 rt-tests 生态中的 `cyclictest`/`rtla`。

## cyclictest 与 rt-tests {#cyclictest}

**来源：** [man 手册页](https://man.archlinux.org/man/cyclictest.8.en) · [kernel.org 上的 rt-tests](https://git.kernel.org/pub/scm/utils/rt-tests/rt-tests.git) · <span class="lvl lvl-i">进阶</span> · 积极维护中

标准的实时延迟测量套件：`cyclictest` 测量定时器唤醒延迟的分布（实时内核的经典基准），`hwlatdetect` 用于定位固件/SMI 引入的延迟，配套的 `rteval` 工具（独立项目）则把它们打包用于负载下的整机评估。在较新的内核（5.17+）上，推荐搭配内核自带的 `rtla` 工具及其 `timerlat`/`osnoise` 追踪器——它不仅告诉你抖动*发生了*，还能告诉你是*哪条*内核路径造成的。

## sockperf — 套接字级延迟基准测试 {#sockperf}

**来源：** [GitHub（Mellanox）](https://github.com/Mellanox/sockperf) · <span class="lvl lvl-b">入门</span> · v3.10，2022 年

Mellanox/NVIDIA 出品的首选网络延迟微基准工具：支持 TCP/UDP 上的 ping-pong、带负载和吞吐量模式，并给出完整的分位数直方图（而不只是平均值——在延迟领域，尾部才是重点）。它既支持内核套接字，也支持内核旁路（kernel-bypass）协议栈（VMA/XLIO、Onload），因此成为网卡与协议栈调优前后对比的标准工具。可与 `iperf3`/`netperf`（测吞吐量）以及 MoonGen（硬件线速发包）配合使用。

## Corvil（Pico）— 生产环境延迟分析平台 {#corvil}

**来源：** [pico.net](https://www.pico.net/products/corvil-analytics/) · <span class="lvl lvl-b">入门</span> · 现行产品（2025 平台）

交易领域被动式、报文级延迟测量的主导商业平台：纳秒级时间戳、200 Gbps 持续抓包与实时分析（Corvil 12000）、600 多种交易协议解码器，以及带可追溯时间的 MiFID II/RegNMS/CAT 合规报告。它展示了超越 DIY 工具的生产级延迟监控是什么样子——无需 agent、无需改动应用，每一笔订单都能从行情 tick 一路测量到订单回报。Corvil 于 2019 年被 Pico 收购。

## c2clat 与 core-to-core-latency {#c2clat}

**来源：** [rigtorp/c2clat](https://github.com/rigtorp/c2clat) · [nviennot/core-to-core-latency](https://github.com/nviennot/core-to-core-latency) · <span class="lvl lvl-i">进阶</span> · 2020/2022 年，至今仍是标杆

两个开源工具，测量每一对 CPU 核心之间的缓存一致性（cache-coherence）延迟，并以矩阵/热力图呈现结果。它们揭示的拓扑效应决定了线程摆放策略：同一 CCX 内或 SMT 兄弟线程间约 8–25 ns，跨 chiplet 边界 85–180 ns，跨插槽（socket）则有数倍的代价。跑一遍这类工具如今已是任何新交易服务器的标准摸底步骤——厂商在 QA 中也在用。（[硬件章节](/zh/summaries/hardware/#c2c-latency)中亦有介绍。）

## Google Benchmark {#google-benchmark-tools}

**来源：** [GitHub](https://github.com/google/benchmark) · <span class="lvl lvl-b">入门</span> · v1.9.5，2026 年 1 月

标准的 C++ 微基准测试框架——统计上稳健的计时、自动确定迭代次数、`DoNotOptimize` 屏障以及性能计数器集成。列在这里是作为上述系统级工具在代码层面的补充；完整摘要见[软件章节](/zh/summaries/software/#google-benchmark)。
