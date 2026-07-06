---
title: "Summaries — Networking, NICs & Kernel Bypass"
description: "Plain-language summaries of the networking, NIC, kernel-bypass and switch references in the collection."
permalink: /summaries/networking/
translation: /zh/summaries/networking/
nav: home
---

# Reference Summaries: Networking, NICs & Kernel Bypass

[← Back to the reference list](/#networking-nics--kernel-bypass) · Each entry below summarizes one reference from the collection, so you can decide whether it is worth your reading time.

---

## Onload User Guide (AMD UG1586) {#onload-guide}

**Source:** [docs.amd.com](https://docs.amd.com/r/en-US/ug1586-onload-user) · <span class="lvl lvl-a">advanced</span> · living document, v1.5 (2026)

The canonical, actively maintained manual for the entire AMD Solarflare acceleration stack: Onload (transparent sockets acceleration), **ef_vi** (the lowest-latency raw layer-2 API) and **TCPDirect**. Covers installation, tuning knobs (spinning, interrupt handling, NUMA/IRQ affinity), hardware timestamping, and per-adapter latency test results for the X2/X3/X4 series. This replaces the old support.solarflare.com portal and openonload.org, both of which this site linked for years and both of which are now gone.

## OpenOnload on GitHub {#openonload}

**Source:** [github.com/Xilinx-CNS/onload](https://github.com/Xilinx-CNS/onload) · <span class="lvl lvl-i">intermediate</span> · open-sourced 2021, active (v9.x)

OpenOnload — the user-level network stack that accelerates unmodified BSD-sockets applications — has been fully open source since 2021. The repository shows how Onload preserves full sockets semantics (fork/exec, epoll, fd passing) while bypassing the kernel, and now includes an AF_XDP backend so it can even run on non-Solarflare NICs. The famous "OpenOnload Google Talk" PDF that used to live on openonload.org survives only on the [Wayback Machine](https://web.archive.org/web/2020/http://www.openonload.org/openonload-google-talk.pdf).

## AMD Solarflare X4 Launch {#solarflare-x4}

**Source:** [amd.com blog](https://www.amd.com/en/blogs/2025/new-amd-solarflare-x4-low-latency-ethernet-adapter-set.html) · [ServeTheHome coverage](https://www.servethehome.com/amd-solarflare-x4-nics-launched-for-low-latency-trading/) · <span class="lvl lvl-b">beginner</span> · October 2025

AMD's launch post for the current flagship trading NIC family: the X4522 (2× 10/25/50GbE) and X4542 (up to 100GbE), built on a custom low-latency ASIC with PCIe Gen5 and CTPIO cut-through transmit, claiming up to 40% lower latency than prior Solarflare generations. A good snapshot of where the mainstream software-trading NIC market stands in 2025–2026 — AMD cites 9 of the top 10 global exchanges as Solarflare users.

## Performance Tuning for Mellanox Adapters (NVIDIA) {#mellanox-tuning}

**Source:** [enterprise-support.nvidia.com](https://enterprise-support.nvidia.com/s/article/performance-tuning-for-mellanox-adapters) · <span class="lvl lvl-a">advanced</span> · maintained, current through 2025

The successor to the classic "Performance Tuning Guidelines for Mellanox Network Adapters" PDF (the old mellanox.com link is dead). Walks through BIOS settings, PCIe configuration, NUMA and CPU affinity, IRQ moderation, and the `mlnx_tune` utility that automates the checks for ConnectX-class adapters. The system-level advice — C-states, core isolation, interrupt steering — generalizes to any low-latency NIC deployment.

## NVIDIA XLIO Documentation (successor to VMA) {#xlio}

**Source:** [networking-docs.nvidia.com/xlioswum](https://networking-docs.nvidia.com/xlioswum) · <span class="lvl lvl-a">advanced</span> · Rev 3.6x–3.70, 2025–2026

XLIO is NVIDIA's kernel-bypass sockets library and the designated successor to Mellanox VMA (forked from VMA 9.2.2). The docs cover the `LD_PRELOAD` usage model, TCP/UDP offload paths, polling/spinning tuning and socket-scaling limits on ConnectX NICs. Anyone with VMA tuning notes from the 2018 era should migrate here; legacy VMA docs remain at [networking-docs.nvidia.com/vmaswum](https://networking-docs.nvidia.com/vmaswum) for older deployments.

## Rivermax + NEIO FastSockets for Financial Services {#rivermax}

**Source:** [developer.nvidia.com blog](https://developer.nvidia.com/blog/maximizing-low-latency-networking-performance-for-financial-services-with-nvidia-rivermax-and-neio-fastsocket/) · <span class="lvl lvl-i">intermediate</span> · September 2025

NVIDIA's current pitch for trading workloads on ConnectX: Rivermax kernel-bypass zero-copy streaming plus the NEIO FastSockets library on top. Benchmarks show ~0.25 µs serialization delay (claimed 8× faster than standard sockets) and 3.35M pps dropless UDP receive, with GPUDirect for NIC-to-GPU paths. Useful for understanding how the Mellanox-heritage stack now competes with Onload for market-data workloads.

## RDMA Aware Networks Programming User Manual {#rdma-manual}

**Source:** [networking-docs.nvidia.com](https://networking-docs.nvidia.com/rdmaawareprogramming) · <span class="lvl lvl-a">advanced</span> · v1.7, maintained

The canonical replacement for the dead mellanox.com RDMA programming PDF this site used to link. Explains RDMA architecture, verbs, memory registration, queue pairs, completion handling and the `rdma_cm` connection API, with application skeletons. Note the role change since 2018: InfiniBand is essentially absent from exchange-facing paths — in trading, RDMA now appears as **RoCEv2** on internal fabrics (distribution, storage, risk, capture; ~2–5 µs class), while front-end market data stays Ethernet UDP multicast + kernel-bypass NICs.

## Arista 7130 Series — Layer-1 & FPGA Switching {#arista-7130}

**Source:** [arista.com](https://www.arista.com/en/products/7130-series) · <span class="lvl lvl-i">intermediate</span> · current; 25G generation Oct 2023

After Cisco end-of-saled the entire Nexus 3550 (ex-Exablaze) switch line in April 2023 with no replacement, Arista's 7130 family (from the Metamako acquisition) became the de facto ultra-low-latency switching platform. The hub covers Layer-1 crosspoint switches (~4 ns port-to-port), MetaMux (~39 ns FPGA multiplexing for exchange-facing links), MetaWatch (tap/aggregation with sub-ns timestamping), and the 25G generation (7132LB: 150 ns L3 at 25GbE). Note the current frontier: 25GbE run FEC-off over short links, since FEC alone adds ~100+ ns.

## Measuring the Latency of a 4 ns Switch (Arista) {#arista-4ns}

**Source:** [PDF](https://www.arista.com/assets/data/pdf/Latency-4ns-Switch-Solution-Brief.pdf) · <span class="lvl lvl-a">advanced</span> · ~2016, still the reference methodology

A short engineering paper on how you even measure a device whose port-to-port latency (~4 ns) is below the resolution of ordinary test gear. Explains layer-1 switching, measurement methodology and error sources, and why sub-10 ns claims need careful qualification. Teaches healthy skepticism about vendor latency numbers and the physics floor of the current arms race.

## Cisco Nexus SmartNIC (ex-ExaNIC) {#exanic}

**Source:** [datasheet](https://www.cisco.com/c/en/us/products/collateral/interfaces-modules/nexus-smartnic/datasheet-c78-743827.html) · [exanic-software on GitHub](https://github.com/cisco/exanic-software) · <span class="lvl lvl-i">intermediate</span> · maintenance mode

Cisco acquired Exablaze in December 2019 and rebranded the ExaNIC line as Nexus SmartNIC (K3P-S = ExaNIC X25, K3P-Q = X100), with published software trigger-to-response as low as 568 ns and 4 ns RX timestamps. The `libexanic` driver stack remains open on GitHub. Listed with a caveat: the product line is effectively in maintenance mode and Cisco has exited the ULL switch business, so ecosystem momentum has moved to AMD Solarflare and the FPGA vendors. (The old exablaze.com blog this site linked is gone entirely.)

## Kernel Bypass (Cloudflare) {#cf-kernel-bypass}

**Source:** [blog.cloudflare.com](https://blog.cloudflare.com/kernel-bypass/) · <span class="lvl lvl-b">beginner</span> · 2015 (evergreen intro)

A concise map of why the Linux kernel tops out around ~1M packets/second per core, and the escape hatches: PACKET_MMAP, PF_RING, DPDK, Netmap and Solarflare's ef_vi. Marek Majkowski explains the shared costs — syscalls, copies, interrupts — that every bypass technique tries to eliminate. Still the clearest beginner framing of the problem space; pair with newer AF_XDP material for the modern in-kernel answer.

## How to Receive a Million Packets Per Second (Cloudflare) {#cf-million-packets}

**Source:** [blog.cloudflare.com](https://blog.cloudflare.com/how-to-receive-a-million-packets/) · <span class="lvl lvl-i">intermediate</span> · 2015 (techniques still current)

A hands-on walk from a naive UDP receiver (~370k pps) to 1.1M+ pps using multi-queue NICs, RSS, `SO_REUSEPORT` and NUMA pinning — all without leaving the kernel. Demonstrates concretely where the Linux stack's per-core limits are and how much headroom correct queue/core alignment buys. The flow-steering and NUMA-locality techniques are exactly what trading systems tune even when they *do* use bypass.

## Why We Use the Linux Kernel's TCP Stack (Cloudflare) {#cf-kernel-tcp}

**Source:** [blog.cloudflare.com](https://blog.cloudflare.com/why-we-use-the-linux-kernels-tcp-stack/) · <span class="lvl lvl-b">beginner</span> · 2016 (reasoning still valid)

The counterpoint to bypass-everything enthusiasm: what you give up when you leave the kernel — routing, firewalling, TCP correctness, tooling, operability — and why full userspace stacks only pay off for narrowly scoped, latency-critical applications. Frames the decision matrix (kernel + tuning vs partial bypass vs full bypass) that trading firms still apply per component today.

## Extreme HTTP Performance Tuning (talawah.io) {#talawah}

**Source:** [talawah.io](https://talawah.io/blog/extreme-http-performance-tuning-one-point-two-million/) · <span class="lvl lvl-a">advanced</span> · May 2021

Marc Richards' meticulously measured tour of modern Linux network tuning: speculative-execution mitigations, syscall auditing, iptables removal, perfect packet locality (RSS/XPS/busy-poll), interrupt strategy and qdisc choice — with flame graphs and bpftrace evidence for every step, achieving a 5× throughput gain without kernel bypass. The single best current resource for squeezing the in-kernel path, directly transferable to trading-adjacent servers that can't run Onload/DPDK.

## Monitoring and Tuning the Linux Networking Stack (packagecloud) {#packagecloud}

**Source:** [blog.packagecloud.io](https://blog.packagecloud.io/monitoring-tuning-linux-networking-stack-receiving-data/) · <span class="lvl lvl-a">advanced</span> · 2016, still the standard deep dive

The definitive source-level walkthrough of the Linux receive path: driver initialization, DMA rings, NAPI, softIRQs, RSS/RPS, GRO, and every relevant `ethtool`/`proc` knob, with kernel source references throughout. Explains what all the tuning advice *actually does* under the hood — prerequisite knowledge for understanding what bypass stacks skip. A companion transmit-side post exists on the same blog.

## Linux Network Performance Ultimate Guide (ntk148v) {#ntk148v}

**Source:** [ntk148v.github.io](https://ntk148v.github.io/posts/linux-network-performance-ultimate-guide/) · <span class="lvl lvl-i">intermediate</span> · 2023

A modern, consolidated network-tuning guide that synthesizes the packagecloud series, Red Hat's docs and the Cloudflare posts into one maintained document: the kernel RX/TX path, ring buffers, interrupt coalescing, IRQ affinity, RSS/RPS/RFS/aRFS, TCP buffer and qdisc tuning, closing with kernel bypass (DPDK, PF_RING) and XDP/AF_XDP. The practical, current replacement for the 2013-era multiqueue and TCP-tuning links this site used to carry.

## Understanding TCP/IP Network Stack (CUBRID) {#cubrid}

**Source:** [cubrid.org](https://www.cubrid.org/blog/3826497) · <span class="lvl lvl-b">beginner</span> · 2013 original (URL updated)

The classic long-form explanation of how data moves through the Linux TCP/IP stack: layered architecture, `sk_buff`, transmit/receive paths, driver-NIC interaction and flow control, illustrated with actual kernel code. Conceptual rather than version-specific, so it ages well as a foundations piece — the mental model you need before deciding what to tune or bypass. (The old cubrid.org URL died; this is the current location.)

## Zero Copy I: User-Mode Perspective (Linux Journal) {#zero-copy}

**Source:** [linuxjournal.com](https://www.linuxjournal.com/article/6345) · <span class="lvl lvl-i">intermediate</span> · 2003 (classic)

The classic explanation of why copying between kernel and user space costs so much and the zero-copy techniques that avoid it: `sendfile`, memory mapping, and the design trade-offs of each. Twenty years on, the same ideas underpin `MSG_ZEROCOPY`, kernel-bypass DMA and io_uring's registered buffers. Short, foundational, still worth the read.

## Tick-to-Trade Latency (Databento) {#tick-to-trade}

**Source:** [databento.com](https://databento.com/microstructure/tick-to-trade) · <span class="lvl lvl-b">beginner</span> · 2024

A current, practitioner-written definition of the metric this whole stack exists to optimize. Gives 2024-era reference numbers — sub-2 µs software tick-to-trade with Solarflare-class NICs and kernel bypass, sub-microsecond as standard for FPGA systems, low-double-digit nanoseconds at the competitive frontier — plus how firms actually measure it with taps and nanosecond-timestamping capture devices. Read this first to calibrate expectations about what each technology tier buys.
