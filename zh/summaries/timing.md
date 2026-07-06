---
title: "参考摘要 — 时间同步与时间戳"
description: "以通俗语言介绍本站收录的时钟同步与时间戳参考资料。"
permalink: /zh/summaries/timing/
translation: /summaries/timing/
lang: zh
nav: home
---

# 参考资料摘要：时间同步与时间戳

[← 返回资源列表](/zh/#time-synchronization--timestamping) · 以下每一条摘要对应本站收录的一篇参考资料，帮助你判断它是否值得花时间阅读。

---

## Meta 如何部署 PTP {#meta-ptp}

**来源：** [engineering.fb.com](https://engineering.fb.com/2022/11/21/production-engineering/precision-time-protocol-at-meta/) · <span class="lvl lvl-i">进阶</span> · 2022 年 11 月，2024 年更新（SPTP）

Meta 关于在超大规模数据中心用 PTP 取代 NTP 的权威文章，实现了数十纳秒级的时钟同步（最坏情况的不确定性窗口约 436 ns）——比他们精心调优过的 NTP 好约 100 倍。文章覆盖完整架构：基于 Time Card 的开放时间服务器、支持硬件时间戳的网卡，以及开源软件栈（服务 100 万以上客户端的 `ptp4u`、`fbclock`、`ptpcheck`、`oscillatord`，以及较新的 SPTP 客户端）。这是金融行业之外最好的生产环境 PTP 公开案例，可直接借鉴到交易基础设施中。

## OCP Time Appliances Project — Time Card {#time-card}

**来源：** [GitHub](https://github.com/Time-Appliances-Project/Time-Card) · <span class="lvl lvl-a">高级</span> · 2021–2024 年（2024 年 12 月发布版本）

这个开源硬件设计能把任何配备硬件时间戳网卡的 x86 服务器变成 stratum-1 时间设备：一块 PCIe 卡，集成 GNSS 接收器和用于守时（holdover）的微型原子钟，以标准的 `/dev/ptpX` 和 `/dev/ppsY` 设备暴露给 Linux，可直接被 chrony 和 ptp4l 使用。该设计正在被标准化为 IEEE P3335，并已有商用现货版本。过去需要专有 grandmaster 设备才能做到的事，如今已是开放硬件。

## White Rabbit — 来自 CERN 的亚纳秒授时 {#white-rabbit}

**来源：** [white-rabbit.web.cern.ch](https://white-rabbit.web.cern.ch/) · <span class="lvl lvl-i">进阶</span> · 持续进行中；协作组织于 2024 年成立

这项亚纳秒（sub-ns）级授时技术在 PTP 的基础上加入了同步以太网（Synchronous Ethernet）和相位测量，在公里级光纤上实现亚纳秒精度。White Rabbit 已被纳入 IEEE 1588-2019 标准，成为其 High Accuracy 配置；硬件、固件和软件全部开源。CERN 于 2024 年正式成立了 White Rabbit 协作组织，其中包括四家金融行业成员——它是交易领域"超越 PTP"授时方案的参照标杆。

## 德意志交易所 / Eurex 高精度时间服务（White Rabbit） {#eurex-wr}

**来源：** [eurex.com](https://www.eurex.com/ex-en/support/initiatives/archive/high-precision-time-white-rabbit-pilot) · <span class="lvl lvl-b">入门</span> · 2018–2019 年试点，2019 年 4 月起成为永久服务

一个真实交易所如何向交易参与者分发时间：托管（co-location）客户直接同步到德意志交易所生成自身高精度时间戳所用的那个时钟，通过专用光纤交叉互连（cross-connect）交付，精度达亚纳秒级。文章解释了为什么以交易所为锚点的授时对延迟测量和公平性分析至关重要。德意志交易所至今仍是 White Rabbit 协作组织的成员。

## linuxptp 文档（ptp4l、phc2sys、ts2phc） {#linuxptp}

**来源：** [linuxptp.nwtime.org](https://linuxptp.nwtime.org/documentation/) · <span class="lvl lvl-i">进阶</span> · 持续维护中

Linux 标准 PTP 软件栈的参考文档：`ptp4l`（普通/边界/透明时钟）、`phc2sys`（把系统时钟同步到网卡的 PTP 硬件时钟）、`ts2phc`、`pmc` 以及 `timemaster`（组合 PTP 和 NTP 时间源）。文档包含电信配置、单播、gPTP 及认证等实用示例配置。任何符合 MiFID II 要求的 Linux 时间戳部署，底层用的都是这套软件。

## MiFID II RTS 25 与 Linux 上的时间同步（Red Hat） {#mifid-rts25}

**来源：** [redhat.com](https://www.redhat.com/en/blog/mifid-ii-rts-25-and-time-synchronisation-red-hat-enterprise-linux-and-red-hat-virtualization) · <span class="lvl lvl-b">入门</span> · 2017 年，基础内容至今仍然准确

这篇文章把欧盟监管要求——RTS 25 规定高频交易机构与 UTC 的偏差不得超过 100 µs、时间戳粒度须达 1 µs——对应到 Linux 的实际能力数字：软件时间戳的 NTP/PTP 约 30 µs，硬件时间戳的 NTP 约 10 µs，硬件时间戳的 PTP 约 3 µs 或更好。文章解释了为什么硬件时间戳网卡对合规而言实际上是必备项。注意：在正在进行的 MiFID II 复审中 RTS 25 预计将被取代，但其技术指导依然成立。

## 各代网卡的实测 PTP 精度（scottstuff.net） {#nic-ptp-accuracy}

**来源：** [scottstuff.net](https://scottstuff.net/posts/2025/05/07/connectx-4-and-linuxptp/) · <span class="lvl lvl-a">高级</span> · 2025 年 5 月

一次对多代网卡（ConnectX-3 至 -6、Intel x710/E810）真实 PTP 精度的细致实地测量，结果表明硬件差异巨大：Intel E810 级别的网卡能把与 PHC 的偏差保持在约 10 ns，而运行 ptp4l 的 ConnectX-4 误差放大了 45 倍，尖峰高达数十微秒。文章还记录了一个隐蔽的生产环境陷阱——启用硬件时间戳的 chrony 和 ptp4l 共享同一个 PHC 时误差会相互耦合——并给出了规避方法。这正是厂商数据手册里缺失的、实测且新鲜的数据。

## Intel rdtsc 基准测试白皮书（Paoloni） {#rdtsc-paper}

**来源：** [镜像 PDF](https://cis.temple.edu/~qzeng/cis3207-spring18/files/ia-32-ia-64-benchmark-code-execution-paper.pdf) · [Intel 原文存档](https://web.archive.org/web/20230817174418/https://www.intel.com/content/dam/www/public/us/en/documents/white-papers/ia-32-ia-64-benchmark-code-execution-paper.pdf) · <span class="lvl lvl-a">高级</span> · 2010 年（经典）

Gabriele Paoloni 的经典 Intel 白皮书，讲解如何用 `rdtsc`/`rdtscp` 做周期级精度的代码计时：串行化指令、内存屏障（fencing），以及获得可重复测量结果的统计方法。Intel 已撤下原始页面（本站原先的链接已失效），但这篇论文仍是几乎所有基准测试框架都会引用的理论基础。现代实践在此之上还会加上不变 TSC（invariant TSC）、隔离核心，以及测量章节中介绍的各种抖动分析工具。
