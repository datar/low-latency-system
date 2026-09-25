---
title: "参考摘要 — FPGA 与硬件加速"
description: "以通俗语言介绍本站收录的 FPGA 与硬件加速参考资料。"
permalink: /zh/summaries/fpga/
translation: /summaries/fpga/
lang: zh
nav: home
---

# 参考资料摘要：FPGA 与硬件加速

[← 返回资源列表](/zh/#fpga--hardware-acceleration) · 以下每一条摘要对应本站收录的一篇参考资料，帮助你判断它是否值得花时间阅读。

---

## AMD Alveo UL3524 — 产品简介（Product Brief） {#ul3524}

**来源：** [PDF](https://www.xilinx.com/content/dam/xilinx/publications/product-briefs/2233051_Product_Brief_UL3524_Alveo_Accelerator_Card.pdf) · <span class="lvl lvl-i">进阶</span> · 2023 年 9 月

这是首款专为电子交易打造的 FPGA 加速卡的官方产品简介：定制版 Virtex UltraScale+ VU2P，通过重新设计的收发器路径实现了**低于 3 ns** 的线到线（wire-to-wire）延迟——比上一代 FPGA 快约 7 倍。文档详细介绍了板卡外形、64 个 GTF 收发器以及参考设计。凡是讨论 2023 年之后的交易 FPGA，这份规格文档都是最具体的基准依据；更便宜的 Alveo UL3422（2024 年 10 月发布）把同样的收发器带到了更纤薄的板卡上。

## Exegy + AMD：13.9 ns 的 STAC-T0 纪录 {#stac-t0-record}

**来源：** [exegy.com](https://www.exegy.com/exegy-amd-new-record/) · 审计报告：[STAC AMD240422](https://docs.stacresearch.com/news/AMD240422) · <span class="lvl lvl-i">进阶</span> · 2024 年 6 月

已公开的最快 tick-to-trade 成绩：在 STAC-T0 基准测试中达到 13.9 ns 的最小可执行延迟，比此前 24.2 ns 的纪录低 49%，抖动（jitter）压缩到约 200 ps。该成绩使用现货可购的 Alveo UL3524 加上 Exegy 的 nxFramework 实现，关键路径采用了异步（无时钟）设计。它确立了 2024–2026 年线到线延迟下限的技术水准。（STAC 页面需免费注册后访问。）

## Where Next for Ultra-Low Latency Trading?（A-Team Insight，超低延迟交易的下一步） {#ateam-fpga}

**来源：** [a-teaminsight.com](https://a-teaminsight.com/blog/as-the-latest-fpga-technology-from-amd-sets-the-gold-standard-where-next-for-ultra-low-latency-trading/) · <span class="lvl lvl-b">入门</span> · 2024 年 11 月

对 13.9 ns 纪录之后延迟军备竞赛的行业分析。文章比较了软件、FPGA、ASIC 三个实现层级，解释了为什么 ASIC 是顶级机构的终局选择但僵化且资本投入巨大，而 FPGA 占据着"确定性但可重编程"的中间地带。还介绍了性价比更高的 UL3422，以及 AI/ML 推理即将在关键路径上扮演的角色。对于决定把工程资源投向何处，这是一篇很好的战略性综述。

## Orthogone — 现代 FPGA 交易技术栈 {#orthogone}

**来源：** [orthogone.com](https://orthogone.com/industries/high-frequency-trading-finance/) · <span class="lvl lvl-i">进阶</span> · 2025 年

一家厂商对完整 FPGA 交易技术栈的当前视角：超低延迟 IP 核，包括线到线延迟 17.1 ns 的以太网 MAC/PCS 和 6.2 ns 的 TX TCP/UDP 卸载引擎，外加内核旁路（kernel-bypass）PCIe DMA。文章展示了现代 CPU+FPGA 混合系统如何划分职责——数据路径放在 FPGA、策略迭代放在软件——并给出具体数字，例如在超频的 Blackcore 服务器上 FPGA 到主机的往返为 585 ns。它代表了 Exegy（2022 年收购 Enyx，2026 年 1 月又收购 NovaSparks）和 AMD 之外的 2025 年厂商生态。

## 用 FPGA 对接 Nasdaq ITCH 与 OUCH 协议（Design Gateway） {#dgway-itch-ouch}

**来源：** [dgway.com](https://dgway.com/blog_E/2025/12/18/ultra-low-latency-fpga-trading-with-nasdaq-developed-itch-ouch/) · <span class="lvl lvl-i">进阶</span> · 2025 年 12 月

一篇构建纯硬件 Nasdaq 协议交易流水线的实战讲解：入向是基于 MoldUDP64 的 ITCH 行情数据，中间是订单簿构建，出向是基于 SoupBinTCP 的 OUCH 报单——全部在 AMD Alveo X3522 的 FPGA 逻辑中完成。文章展示了真实 tick-to-trade 路径上存在哪些协议层，以及商业 IP 核如何组合成一条确定性流水线。是从概念走向可落地设计的一座好桥梁。

## Algo-Logic — FPGA 交易解决方案 {#algo-logic}

**来源：** [algo-logic.com](https://www.algo-logic.com/) · <span class="lvl lvl-b">入门</span> · 持续活跃至 2026 年

历史最悠久且仍在运营的金融 FPGA 厂商之一：提供 tick-to-trade 系统、预构建的订单簿与协议解析 gateware，以及 FPGA 即服务（FPGA-as-a-service），如今还与 AI 驱动的硬件加速交易一同推广。本站自 2018 年起就收录了 Algo-Logic；John Lockwood 关于其低延迟库的经典 HOT Interconnects 演讲幻灯片保存在 [Wayback Machine](https://web.archive.org/web/20181221085356/http://www.hoti.org/hoti20/slides/Lockwood_AlgoLogic.pdf)（hoti.org 的原始链接已失效）。
