---
title: "参考摘要 — 网络、网卡与内核旁路"
description: "本站收录的网络、网卡、内核旁路与交换机参考资料的通俗摘要。"
permalink: /zh/summaries/networking/
translation: /summaries/networking/
lang: zh
nav: home
---

# 参考资料摘要：网络、网卡与内核旁路

[← 返回资源列表](/zh/#networking-nics--kernel-bypass) · 下面每一条摘要对应收录中的一份参考资料，帮助你判断它是否值得花时间阅读。

---

## Onload User Guide（AMD UG1586） {#onload-guide}

**来源：** [docs.amd.com](https://docs.amd.com/r/en-US/ug1586-onload-user) · <span class="lvl lvl-a">高级</span> · 持续更新的文档，v1.5（2026）

覆盖整个 AMD Solarflare 加速栈的权威、持续维护的手册：Onload（透明套接字加速）、**ef_vi**（延迟最低的原生二层 API）和 **TCPDirect**。内容包括安装、调优选项（自旋、中断处理、NUMA/中断（IRQ）亲和性）、硬件时间戳（timestamp），以及 X2/X3/X4 系列各型号网卡的延迟测试数据。它取代了本站多年链接的旧 support.solarflare.com 门户和 openonload.org——两者如今都已消失。

## OpenOnload on GitHub {#openonload}

**来源：** [github.com/Xilinx-CNS/onload](https://github.com/Xilinx-CNS/onload) · <span class="lvl lvl-i">进阶</span> · 2021 年开源，活跃开发中（v9.x）

OpenOnload——无需修改即可加速 BSD 套接字应用的用户态网络栈——自 2021 年起已完全开源。该仓库展示了 Onload 如何在绕过内核的同时保留完整的套接字语义（fork/exec、epoll、fd 传递），如今还加入了 AF_XDP 后端，甚至可以在非 Solarflare 网卡上运行。曾经放在 openonload.org 上的著名"OpenOnload Google Talk" PDF 如今只存于 [Wayback Machine](https://web.archive.org/web/2020/http://www.openonload.org/openonload-google-talk.pdf)。

## AMD Solarflare X4 发布 {#solarflare-x4}

**来源：** [amd.com blog](https://www.amd.com/en/blogs/2025/new-amd-solarflare-x4-low-latency-ethernet-adapter-set.html) · [ServeTheHome coverage](https://www.servethehome.com/amd-solarflare-x4-nics-launched-for-low-latency-trading/) · <span class="lvl lvl-b">入门</span> · 2025 年 10 月

AMD 为当前旗舰交易网卡家族发布的博文：X4522（2× 10/25/50GbE）和 X4542（最高 100GbE），基于定制低延迟 ASIC，支持 PCIe Gen5 和 CTPIO 直通发送，宣称延迟比前代 Solarflare 产品最多降低 40%。这也是 2025–2026 年主流软件交易网卡市场格局的一个很好的快照——AMD 称全球前十大交易所中有 9 家在使用 Solarflare。

## Performance Tuning for Mellanox Adapters（NVIDIA） {#mellanox-tuning}

**来源：** [enterprise-support.nvidia.com](https://enterprise-support.nvidia.com/s/article/performance-tuning-for-mellanox-adapters) · <span class="lvl lvl-a">高级</span> · 持续维护，更新至 2025 年

经典 PDF《Performance Tuning Guidelines for Mellanox Network Adapters》的继任者（旧的 mellanox.com 链接已失效）。内容逐一讲解 BIOS 设置、PCIe 配置、NUMA 与 CPU 亲和性、中断合并（IRQ moderation），以及为 ConnectX 系列网卡自动化执行这些检查的 `mlnx_tune` 工具。其中系统层面的建议——C-state、核心隔离、中断引导——适用于任何低延迟网卡部署。

## NVIDIA XLIO 文档（VMA 的继任者） {#xlio}

**来源：** [networking-docs.nvidia.com/xlioswum](https://networking-docs.nvidia.com/xlioswum) · <span class="lvl lvl-a">高级</span> · Rev 3.6x–3.80，2025–2026 年 · 源码：[Mellanox/libxlio](https://github.com/Mellanox/libxlio)

XLIO 是 NVIDIA 的内核旁路（kernel bypass）套接字库，也是 Mellanox VMA 的指定继任者（从 VMA 9.2.2 分叉而来）。文档涵盖 `LD_PRELOAD` 使用模式、TCP/UDP 卸载路径、轮询/自旋调优，以及在 ConnectX 网卡上的套接字扩展上限。还留着 2018 年时代 VMA 调优笔记的读者都应迁移到这里；面向老部署的 VMA 旧文档仍保留在 [networking-docs.nvidia.com/vmaswum](https://networking-docs.nvidia.com/vmaswum)。

## Rivermax + NEIO FastSockets 金融服务方案 {#rivermax}

**来源：** [developer.nvidia.com blog](https://developer.nvidia.com/blog/maximizing-low-latency-networking-performance-for-financial-services-with-nvidia-rivermax-and-neio-fastsocket/) · <span class="lvl lvl-i">进阶</span> · 2025 年 9 月

NVIDIA 当前面向 ConnectX 交易工作负载的方案：Rivermax 内核旁路（kernel bypass）零拷贝流式传输，加上其上的 NEIO FastSockets 库。基准测试（在 Windows 上使用 25GbE ConnectX-6 运行）显示约 0.25 µs 的序列化延迟（宣称比标准套接字快 8 倍）和 3.35M pps 的无丢包 UDP 接收，并支持 GPUDirect 的网卡到 GPU 直达路径。有助于理解 Mellanox 血统的技术栈如今如何在行情数据工作负载上与 Onload 竞争。

## RDMA Aware Networks Programming User Manual {#rdma-manual}

**来源：** [networking-docs.nvidia.com](https://networking-docs.nvidia.com/rdmaawareprogramming) · <span class="lvl lvl-a">高级</span> · v1.7，持续维护

本站过去链接的、已失效的 mellanox.com RDMA 编程 PDF 的权威替代。它讲解 RDMA 架构、verbs、内存注册、队列对（queue pair）、完成事件处理和 `rdma_cm` 连接 API，并附应用骨架代码。注意 2018 年以来的角色变化：InfiniBand 在面向交易所的链路上已基本绝迹——在交易领域，RDMA 如今以 **RoCEv2** 的形式出现在内部网络中（分发、存储、风控、抓包；约 2–5 µs 级别），而前端行情数据仍然是以太网 UDP 组播加内核旁路（kernel bypass）网卡。

## Arista 7130 系列 — 一层交换与 FPGA 交换 {#arista-7130}

**来源：** [arista.com](https://www.arista.com/en/products/7130-series) · <span class="lvl lvl-i">进阶</span> · 现行产品；25G 世代于 2023 年 10 月推出

在 Cisco 于 2023 年 4 月对整个 Nexus 3550（原 Exablaze）交换机产品线宣布停售且无后续产品之后，Arista 的 7130 家族（来自对 Metamako 的收购）成为事实上的超低延迟交换平台。该产品页涵盖一层 crosspoint 交换机（端口到端口约 4 ns）、MetaMux（约 39 ns 的 FPGA 复用，用于面向交易所的链路）、MetaWatch（带亚纳秒级时间戳（timestamp）的分光/汇聚），以及 25G 世代（7132LB：25GbE 下 150 ns 的 L3 交换）。注意当前的前沿玩法：短链路上以关闭 FEC 的方式跑 25GbE，因为仅 FEC 一项每跳就会增加约 100–250 ns（RS-FEC 最高约 250 ns，见 [Arista](https://blogs.arista.com/blog/ultra-low-latency-25g-ethernet)）。

## 测量一台 4 ns 交换机的延迟（Arista） {#arista-4ns}

**来源：** [PDF](https://www.arista.com/assets/data/pdf/Latency-4ns-Switch-Solution-Brief.pdf) · <span class="lvl lvl-a">高级</span> · 约 2016 年，至今仍是参考方法论

一篇简短的工程论文，探讨当设备的端口到端口延迟（约 4 ns）低于普通测试仪器的分辨率时，究竟该如何测量。文章讲解一层交换、测量方法与误差来源，以及为什么低于 10 ns 的宣称数字需要谨慎甄别。它教会你对厂商延迟数字保持健康的怀疑态度，并认清当前军备竞赛的物理极限。

## Cisco Nexus SmartNIC（原 ExaNIC） {#exanic}

**来源：** [datasheet](https://www.cisco.com/c/en/us/products/collateral/interfaces-modules/nexus-smartnic/datasheet-c78-743827.html) · [exanic-software on GitHub](https://github.com/cisco/exanic-software) · <span class="lvl lvl-i">进阶</span> · 维护模式

Cisco 收购 Exablaze（2019 年 12 月宣布，2020 年 2 月完成），并将 ExaNIC 产品线更名为 Nexus SmartNIC（K3P-S 即 ExaNIC X25，K3P-Q 即 X100），官方公布的软件触发到响应延迟低至 568 ns，RX 时间戳（timestamp）精度 4 ns。`libexanic` 驱动栈仍在 GitHub 上开源。收录时附带提醒：该产品线实际上已进入维护模式，Cisco 也已退出超低延迟交换机业务，生态动能已转向 AMD Solarflare 和各 FPGA 厂商。（本站过去链接的旧 exablaze.com 博客已彻底消失。）

## Kernel Bypass（Cloudflare） {#cf-kernel-bypass}

**来源：** [blog.cloudflare.com](https://blog.cloudflare.com/kernel-bypass/) · <span class="lvl lvl-b">入门</span> · 2015 年（常青入门文）

一张简明的地图，讲清 Linux 内核为何在单核约 100 万包/秒左右触顶，以及各条逃生通道：PACKET_MMAP、PF_RING、DPDK、Netmap 和 Solarflare 的 ef_vi。Marek Majkowski 解释了每种旁路技术都试图消除的共同开销——系统调用、拷贝、中断。至今仍是对这一问题空间最清晰的入门讲解；建议搭配较新的 AF_XDP 资料了解现代的内核内方案。

## 如何每秒接收一百万个数据包（Cloudflare） {#cf-million-packets}

**来源：** [blog.cloudflare.com](https://blog.cloudflare.com/how-to-receive-a-million-packets/) · <span class="lvl lvl-i">进阶</span> · 2015 年（技术至今适用）

一次动手实践之旅：从朴素的 UDP 接收程序（约 37 万 pps）一路优化到 110 万+ pps，用到多队列网卡、RSS、`SO_REUSEPORT` 和 NUMA 绑定——全程不离开内核。它具体展示了 Linux 协议栈的单核极限在哪里，以及正确的队列/核心对齐能换来多少余量。这些流量引导和 NUMA 局部性技术，正是交易系统即使*使用*旁路时也要调优的内容。

## 我们为什么使用 Linux 内核的 TCP 栈（Cloudflare） {#cf-kernel-tcp}

**来源：** [blog.cloudflare.com](https://blog.cloudflare.com/why-we-use-the-linux-kernels-tcp-stack/) · <span class="lvl lvl-b">入门</span> · 2016 年（论证至今成立）

对"一切都旁路"式狂热的反方观点：离开内核你会失去什么——路由、防火墙、TCP 正确性、工具链、可运维性——以及为什么完全用户态协议栈只对范围狭窄、延迟攸关的应用才划算。文章勾勒的决策矩阵（内核+调优 vs 部分旁路 vs 完全旁路），交易公司至今仍在按组件逐一套用。

## 极限 HTTP 性能调优（talawah.io） {#talawah}

**来源：** [talawah.io](https://talawah.io/blog/extreme-http-performance-tuning-one-point-two-million/) · <span class="lvl lvl-a">高级</span> · 2021 年 5 月

Marc Richards 对现代 Linux 网络调优的一次细致实测巡礼：投机执行缓解措施、系统调用审计、移除 iptables、完美的数据包局部性（RSS/XPS/busy-poll）、中断策略与 qdisc 选择——每一步都有火焰图和 bpftrace 证据支撑，在不使用内核旁路（kernel bypass）的情况下取得 5 倍吞吐提升。这是压榨内核内路径的当前最佳资源，可直接迁移到那些无法运行 Onload/DPDK 的交易周边服务器上。

## Linux 网络栈的监控与调优（packagecloud） {#packagecloud}

**来源：** [blog.packagecloud.io](https://blog.packagecloud.io/monitoring-tuning-linux-networking-stack-receiving-data/) · <span class="lvl lvl-a">高级</span> · 2016 年，至今仍是标准深度长文

对 Linux 接收路径最权威的源码级讲解：驱动初始化、DMA 环形队列、NAPI、软中断（softIRQ）、RSS/RPS、GRO，以及每一个相关的 `ethtool`/`proc` 可调项，全程附内核源码引用。它解释了所有那些调优建议在底层*究竟做了什么*——也是理解旁路协议栈到底跳过了什么的前置知识。同一博客上还有配套的发送路径文章。

## Linux Network Performance Ultimate Guide（ntk148v） {#ntk148v}

**来源：** [ntk148v.github.io](https://ntk148v.github.io/posts/linux-network-performance-ultimate-guide/) · <span class="lvl lvl-i">进阶</span> · 2023 年

一份现代的网络调优综合指南，把 packagecloud 系列、Red Hat 文档和 Cloudflare 博文融汇为一份持续维护的文档：内核收发路径、环形缓冲区、中断合并、中断（IRQ）亲和性、RSS/RPS/RFS/aRFS、TCP 缓冲区与 qdisc 调优，最后以内核旁路（DPDK、PF_RING）和 XDP/AF_XDP 收尾。它是本站过去收录的 2013 年时代多队列与 TCP 调优链接的实用、现行替代品。

## 理解 TCP/IP 网络栈（CUBRID） {#cubrid}

**来源：** [cubrid.org](https://www.cubrid.org/blog/3826497) · <span class="lvl lvl-b">入门</span> · 2013 年原作（URL 已更新）

讲解数据如何在 Linux TCP/IP 协议栈中流动的经典长文：分层架构、`sk_buff`、收发路径、驱动与网卡的交互以及流量控制，配以真实内核代码演示。内容偏概念而非绑定特定版本，因此作为基础读物历久弥新——它给你的是在决定调什么、绕过什么之前必须具备的思维模型。（旧的 cubrid.org URL 已失效；此为现行地址。）

## Zero Copy I: User-Mode Perspective（Linux Journal） {#zero-copy}

**来源：** [linuxjournal.com](https://www.linuxjournal.com/article/6345) · <span class="lvl lvl-i">进阶</span> · 2003 年（经典）

关于内核态与用户态之间拷贝为何代价高昂、以及规避它的零拷贝技术的经典解读：`sendfile`、内存映射，以及各自的设计取舍。二十年过去，同样的思想仍是 `MSG_ZEROCOPY`、内核旁路 DMA 和 io_uring 注册缓冲区的基石。篇幅短小、奠基性强，至今仍值得一读。

## Tick-to-Trade 延迟（Databento） {#tick-to-trade}

**来源：** [databento.com](https://databento.com/microstructure/tick-to-trade) · <span class="lvl lvl-b">入门</span> · 2024 年

由从业者撰写的、对整个技术栈存在意义所系的那个指标的现行定义。它给出了 2024 年的参考数字——使用 Solarflare 级网卡加内核旁路（kernel bypass）的软件系统 tick-to-trade 低于 2 µs，FPGA 系统以亚微秒为常态，竞争最前沿则在低两位数纳秒级——还介绍了各家公司实际如何用分光器和纳秒级时间戳（timestamp）抓包设备进行测量。建议最先阅读这一篇，以校准对每一层技术能买到什么的预期。
