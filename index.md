---
title: "Low Latency System"
description: "A curated, up-to-date collection of resources for building low-latency financial trading systems."
nav: home
translation: /zh/
---

# Low Latency System

A curated collection of resources for building **low-latency systems**, with a focus on financial trading. Every reference below carries a <a class="sum" href="/guide/">summary</a> badge linking to a plain-language summary of that reference, and a level tag — <span class="lvl lvl-b">beginner</span> <span class="lvl lvl-i">intermediate</span> <span class="lvl lvl-a">advanced</span> — so you can decide what to read before you click.

> **New here?** Read **[The Complete Guide](/guide/)** — a single page that organizes this whole topic from beginner to expert: what latency is, where the nanoseconds go, and which techniques buy what.

*Content fully refreshed in July 2026; facts re-checked and corrected in September 2026. The research behind the update — every link checked, dead links replaced, new state-of-the-art references added — is documented in the [research notes](/research/).*

## Start Here {#start-here}

- [Tick-to-Trade Latency](https://databento.com/microstructure/tick-to-trade) [summary](/summaries/networking/#tick-to-trade){:.sum} <span class="lvl lvl-b">beginner</span> — what the metric means and 2024-era reference numbers for software, FPGA and ASIC tiers.
- [Kernel Bypass](https://blog.cloudflare.com/kernel-bypass/) [summary](/summaries/networking/#cf-kernel-bypass){:.sum} <span class="lvl lvl-b">beginner</span> — the clearest introduction to why and how trading systems leave the kernel.
- [What Every Programmer Should Know about How CPUs Work — Matt Godbolt](https://www.youtube.com/watch?v=-HNpim5x-IE) [summary](/summaries/software/#godbolt-cpu){:.sum} <span class="lvl lvl-b">beginner</span> — the best current 43-minute "mechanical sympathy" primer.
- [What is Low Latency C++? — Timur Doumler (2 parts)](https://www.youtube.com/watch?v=EzmNeAhWqVs) [summary](/summaries/software/#doumler){:.sum} <span class="lvl lvl-i">intermediate</span> — the best structured pedagogical treatment of the whole topic.

## Linux OS Tuning {#linux-os-tuning}

- [Monitoring and Managing System Status and Performance (RHEL 10)](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/monitoring_and_managing_system_status_and_performance/index) [summary](/summaries/linux-tuning/#rhel-perf){:.sum} <span class="lvl lvl-b">beginner</span> — the modern successor to the retired RHEL 7 Performance Tuning Guide.
- [Optimizing RHEL for Real Time for Low Latency Operation (RHEL 10)](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux_for_real_time/10/html-single/optimizing_rhel_for_real_time_for_low_latency_operation/index) [summary](/summaries/linux-tuning/#rhel-rt){:.sum} <span class="lvl lvl-i">intermediate</span> — Red Hat's canonical low-latency guide; since RHEL 9 the real-time kernel is included with standard RHEL.
- [Erik Rigtorp — Low Latency Tuning Guide](https://rigtorp.se/low-latency-guide/) [summary](/summaries/linux-tuning/#rigtorp-guide){:.sum} <span class="lvl lvl-i">intermediate</span> — the most-cited practitioner checklist for HFT-style x86_64 tuning.
- [TuneD — the Linux tuning daemon](https://tuned-project.org/) [summary](/summaries/linux-tuning/#tuned){:.sum} <span class="lvl lvl-b">beginner</span> — home of the `latency-performance`, `network-latency` and `cpu-partitioning` profiles.
- [Getting Started with PREEMPT_RT](https://realtime-linux.org/getting-started-with-preempt_rt-guide/) [summary](/summaries/linux-tuning/#preempt-rt-guide){:.sum} <span class="lvl lvl-b">beginner</span> — real-time preemption is in mainline Linux since 6.12 (2024); here's how to use it.
- [The Realtime Preemption End Game (LWN)](https://lwn.net/Articles/989212/) [summary](/summaries/linux-tuning/#lwn-preempt-rt){:.sum} <span class="lvl lvl-i">intermediate</span> — how and why PREEMPT_RT finally merged after 20 years.
- [SUSE Labs — CPU Isolation series](https://www.suse.com/c/cpu-isolation-practical-example-part-5/) [summary](/summaries/linux-tuning/#suse-cpu-isolation){:.sum} <span class="lvl lvl-a">advanced</span> — the `nohz_full` maintainer explains what CPU isolation really does inside the kernel.
- [CPU Isolation for HFT: what `isolcpus` doesn't do](https://nikhilpadala.com/blog/cpu-optimization-linux-latency/) [summary](/summaries/linux-tuning/#isolcpus-hft){:.sum} <span class="lvl lvl-i">intermediate</span> — a trading case study of the full `isolcpus` + `nohz_full` + `rcu_nocbs` recipe.
- [RHEL Performance Guide (Myllynen)](https://github.com/myllynen/rhel-performance-guide) [summary](/summaries/linux-tuning/#myllynen){:.sum} <span class="lvl lvl-i">intermediate</span> — the scattered official docs condensed into one actionable page.
- [Brendan Gregg — Linux Performance](https://www.brendangregg.com/linuxperf.html) [summary](/summaries/linux-tuning/#gregg){:.sum} <span class="lvl lvl-b">beginner</span> — the observability hub: perf, ftrace, eBPF, flame graphs, and the standard textbooks.
- [TLB Shootdowns (JabPerf)](https://www.jabperf.com/how-to-deter-or-disarm-tlb-shootdowns/) [summary](/summaries/linux-tuning/#tlb-shootdowns){:.sum} <span class="lvl lvl-a">advanced</span> — the latency source most tuning checklists miss.
- Historical: [Low Latency Tuning for RHEL 7 (PDF)](https://access.redhat.com/sites/default/files/attachments/201501-perf-brief-low-latency-tuning-rhel7-v2.1.pdf) [summary](/summaries/linux-tuning/#rhel7-legacy){:.sum} · [Red Hat Summit 2018 slides (PDF)](https://www.redhat.com/files/summit/session-assets/2018/Performance-analysis-and-tuning-of-Red-Hat-Enterprise-Linux-Part-1.pdf) [summary](/summaries/linux-tuning/#rh-summit){:.sum} — RHEL 7 is EOL; kept for background.

## Server Hardware & CPUs {#server-hardware--cpus}

Since this site's last update, the landscape changed: HPE retired its overclocked "Trade and Match" servers, and boutique vendors (Blackcore, Hypertec CIARA) now own that niche. On the mainstream side, AMD EPYC 9005 F-series (e.g. 9575F), EPYC 4004/4005 X3D parts with 3D V-Cache, and Intel Xeon 6 with Latency Optimized Mode are the current shipping choices (AMD's Zen 6 EPYC 9006 "Venice", announced July 2026 with a 5 GHz 9686F part, reaches servers from Q4 2026; Intel's next P-core Xeon slipped to 2027). Single-socket is the settled architecture for the latency path, and core-to-core latency is now a first-class CPU selection metric.

- [AMD EPYC 9005 BIOS & Workload Tuning Guide](https://docs.amd.com/v/u/en-US/58467_amd-epyc-9005-tg-bios-and-workload) [summary](/summaries/hardware/#amd-9005-tuning){:.sum} <span class="lvl lvl-a">advanced</span> — the current canonical AMD server tuning document.
- [Dell PowerEdge 16G BIOS Tuning — Low Latency](https://infohub.delltechnologies.com/en-us/l/poweredge-bios-performance-and-workload-tuning-guide-for-16g/low-latency-17/) [summary](/summaries/hardware/#dell-16g){:.sum} <span class="lvl lvl-i">intermediate</span> — replaces Dell's dead 2010 low-latency whitepaper.
- [Configuring HPE ProLiant Servers for Low-Latency Applications](https://www.hpe.com/psnow/doc/c01804533) [summary](/summaries/hardware/#hpe-low-latency){:.sum} <span class="lvl lvl-i">intermediate</span> — the long-running HPE whitepaper at its new home, updated through Gen11 (on Gen12, use the built-in "Low Latency" workload profile).
- [Intel Xeon 6 Performance & Power Profiles (PDF)](https://builders.intel.com/docs/networkbuilders/intel-xeon-6-processors-performance-and-power-profiles-1746785299.pdf) [summary](/summaries/hardware/#intel-xeon6-profiles){:.sum} <span class="lvl lvl-a">advanced</span> — Latency Optimized Mode and Efficiency Latency Control, from Intel.
- [Phoronix — Xeon 6 Latency Optimized Mode benchmarks](https://www.phoronix.com/review/intel-latency-optimized-mode) [summary](/summaries/hardware/#phoronix-lom){:.sum} <span class="lvl lvl-i">intermediate</span> — independent measurements: ~17% geo-mean gain.
- [Xeon 6 High-Priority Cores (SST-BF) Explained](https://www.servethehome.com/intel-xeon-6-high-priority-and-low-priority-cores-explained/) [summary](/summaries/hardware/#sth-sst){:.sum} <span class="lvl lvl-b">beginner</span> — guaranteed-fast cores for your hot threads.
- [Chips and Cheese — Xeon 6 memory subsystem](https://chipsandcheese.com/p/a-look-into-intel-xeon-6s-memory) [summary](/summaries/hardware/#chips-cheese-xeon6){:.sum} <span class="lvl lvl-a">advanced</span> — measured L3/DRAM/core-to-core latencies, Intel mesh vs AMD chiplets.
- [core-to-core-latency](https://github.com/nviennot/core-to-core-latency) [summary](/summaries/hardware/#c2c-latency){:.sum} <span class="lvl lvl-i">intermediate</span> — measure your CPU's topology; community results database.
- [Blackcore — why overclocked trading servers](https://www.blackcoretech.com/knowledge/maximize-your-trading-potential-embrace-overclocking-to-stay-ahead-of-the-curve) [summary](/summaries/hardware/#blackcore){:.sum} <span class="lvl lvl-b">beginner</span> — the boutique-server perspective (vendor content).
- [hft-server-settings](https://github.com/twanas/hft-server-settings) [summary](/summaries/hardware/#hft-server-settings){:.sum} <span class="lvl lvl-i">intermediate</span> — a community checklist of the settings practitioners actually change.
- Historical: [Intel "Optimizing Applications for Latency" parts 1 & 2](https://www.intel.com/content/www/us/en/developer/articles/technical/optimizing-computer-applications-for-latency-part-1-configuring-the-hardware.html) [summary](/summaries/hardware/#intel-latency-articles){:.sum} · [IBM/Lenovo Linux tuning Redpaper (2008)](https://lenovopress.lenovo.com/redp4285) [summary](/summaries/hardware/#lenovo-redp){:.sum}

## Networking, NICs & Kernel Bypass {#networking-nics--kernel-bypass}

The vendor map was redrawn: Solarflare → Xilinx → **AMD** (X4-series NICs, Oct 2025), Mellanox → **NVIDIA** (VMA replaced by XLIO), Exablaze → **Cisco** (which then exited the ULL switch business in 2023, leaving **Arista 7130** as the de facto layer-1 platform).

### Understand the kernel path first

- [Understanding TCP/IP Network Stack (CUBRID)](https://www.cubrid.org/blog/3826497) [summary](/summaries/networking/#cubrid){:.sum} <span class="lvl lvl-b">beginner</span> — the mental model: how a packet moves through Linux.
- [Monitoring and Tuning the Linux Networking Stack (packagecloud)](https://blog.packagecloud.io/monitoring-tuning-linux-networking-stack-receiving-data/) [summary](/summaries/networking/#packagecloud){:.sum} <span class="lvl lvl-a">advanced</span> — the definitive source-level RX-path walkthrough.
- [Linux Network Performance Ultimate Guide (ntk148v)](https://ntk148v.github.io/posts/linux-network-performance-ultimate-guide/) [summary](/summaries/networking/#ntk148v){:.sum} <span class="lvl lvl-i">intermediate</span> — one maintained page consolidating all the classic tuning material.
- [How to Receive a Million Packets Per Second (Cloudflare)](https://blog.cloudflare.com/how-to-receive-a-million-packets/) [summary](/summaries/networking/#cf-million-packets){:.sum} <span class="lvl lvl-i">intermediate</span> — RSS, `SO_REUSEPORT` and NUMA pinning, hands-on.
- [Why We Use the Linux Kernel's TCP Stack (Cloudflare)](https://blog.cloudflare.com/why-we-use-the-linux-kernels-tcp-stack/) [summary](/summaries/networking/#cf-kernel-tcp){:.sum} <span class="lvl lvl-b">beginner</span> — the case *against* bypassing everything.
- [Extreme HTTP Performance Tuning (talawah.io)](https://talawah.io/blog/extreme-http-performance-tuning-one-point-two-million/) [summary](/summaries/networking/#talawah){:.sum} <span class="lvl lvl-a">advanced</span> — 5× throughput without bypass, every knob measured with flame graphs.
- [Zero Copy I: User-Mode Perspective](https://www.linuxjournal.com/article/6345) [summary](/summaries/networking/#zero-copy){:.sum} <span class="lvl lvl-i">intermediate</span> — the 2003 classic behind today's zero-copy techniques.

### Kernel bypass stacks

- [Onload User Guide (AMD UG1586)](https://docs.amd.com/r/en-US/ug1586-onload-user) [summary](/summaries/networking/#onload-guide){:.sum} <span class="lvl lvl-a">advanced</span> — Onload, ef_vi and TCPDirect: the whole Solarflare stack, actively maintained.
- [OpenOnload on GitHub](https://github.com/Xilinx-CNS/onload) [summary](/summaries/networking/#openonload){:.sum} <span class="lvl lvl-i">intermediate</span> — open source since 2021; now runs on non-Solarflare NICs via AF_XDP.
- [NVIDIA XLIO documentation](https://networking-docs.nvidia.com/xlioswum) [summary](/summaries/networking/#xlio){:.sum} <span class="lvl lvl-a">advanced</span> — the successor to Mellanox VMA for socket acceleration on ConnectX.
- [Rivermax + NEIO FastSockets for financial services](https://developer.nvidia.com/blog/maximizing-low-latency-networking-performance-for-financial-services-with-nvidia-rivermax-and-neio-fastsocket/) [summary](/summaries/networking/#rivermax){:.sum} <span class="lvl lvl-i">intermediate</span> — NVIDIA's 2025 answer for market-data workloads.

### NICs

- [AMD Solarflare X4 launch](https://www.amd.com/en/blogs/2025/new-amd-solarflare-x4-low-latency-ethernet-adapter-set.html) [summary](/summaries/networking/#solarflare-x4){:.sum} <span class="lvl lvl-b">beginner</span> — the current flagship trading NIC family (Oct 2025).
- [Performance Tuning for Mellanox Adapters (NVIDIA)](https://enterprise-support.nvidia.com/s/article/performance-tuning-for-mellanox-adapters) [summary](/summaries/networking/#mellanox-tuning){:.sum} <span class="lvl lvl-a">advanced</span> — successor to the classic Mellanox tuning PDF.
- [Cisco Nexus SmartNIC (ex-ExaNIC)](https://www.cisco.com/c/en/us/products/collateral/interfaces-modules/nexus-smartnic/datasheet-c78-743827.html) [summary](/summaries/networking/#exanic){:.sum} <span class="lvl lvl-i">intermediate</span> — 568 ns trigger-to-response; now in maintenance mode.

### Switches

- [Arista 7130 series](https://www.arista.com/en/products/7130-series) [summary](/summaries/networking/#arista-7130){:.sum} <span class="lvl lvl-i">intermediate</span> — layer-1 (~4 ns), MetaMux (~39 ns), MetaWatch; the de facto ULL switching platform.
- [Measuring the Latency of a 4 ns Switch (PDF)](https://www.arista.com/assets/data/pdf/Latency-4ns-Switch-Solution-Brief.pdf) [summary](/summaries/networking/#arista-4ns){:.sum} <span class="lvl lvl-a">advanced</span> — how to measure below your test gear's resolution.

### RDMA & InfiniBand

- [RDMA Aware Networks Programming User Manual](https://networking-docs.nvidia.com/rdmaawareprogramming) [summary](/summaries/networking/#rdma-manual){:.sum} <span class="lvl lvl-a">advanced</span> — verbs, queue pairs and `rdma_cm`, at its new NVIDIA home. Note: in trading, RDMA now means RoCEv2 on *internal* fabrics; exchange-facing paths are Ethernet UDP multicast.

## FPGA & Hardware Acceleration {#fpga--hardware-acceleration}

- [AMD Alveo UL3524 product brief (PDF)](https://www.xilinx.com/content/dam/xilinx/publications/product-briefs/2233051_Product_Brief_UL3524_Alveo_Accelerator_Card.pdf) [summary](/summaries/fpga/#ul3524){:.sum} <span class="lvl lvl-i">intermediate</span> — the first FPGA card purpose-built for trading: <3 ns transceivers.
- [Exegy + AMD: the 13.9 ns STAC-T0 record](https://www.exegy.com/exegy-amd-new-record/) [summary](/summaries/fpga/#stac-t0-record){:.sum} <span class="lvl lvl-i">intermediate</span> — the 2024 tick-to-trade world record.
- [Where next for ultra-low latency trading? (A-Team)](https://a-teaminsight.com/blog/as-the-latest-fpga-technology-from-amd-sets-the-gold-standard-where-next-for-ultra-low-latency-trading/) [summary](/summaries/fpga/#ateam-fpga){:.sum} <span class="lvl lvl-b">beginner</span> — software vs FPGA vs ASIC, strategically framed.
- [Orthogone — HFT FPGA engineering](https://orthogone.com/industries/high-frequency-trading-finance/) [summary](/summaries/fpga/#orthogone){:.sum} <span class="lvl lvl-i">intermediate</span> — a modern FPGA trading stack with real latency numbers.
- [FPGA trading with Nasdaq ITCH & OUCH (Design Gateway)](https://dgway.com/blog_E/2025/12/18/ultra-low-latency-fpga-trading-with-nasdaq-developed-itch-ouch/) [summary](/summaries/fpga/#dgway-itch-ouch){:.sum} <span class="lvl lvl-i">intermediate</span> — an all-hardware tick-to-trade pipeline, protocol by protocol.
- [Algo-Logic](https://www.algo-logic.com/) [summary](/summaries/fpga/#algo-logic){:.sum} <span class="lvl lvl-b">beginner</span> — long-running FPGA-for-finance vendor, still active; classic Lockwood slides on the [Wayback Machine](https://web.archive.org/web/20181221085356/http://www.hoti.org/hoti20/slides/Lockwood_AlgoLogic.pdf).

## Time Synchronization & Timestamping {#time-synchronization--timestamping}

- [How PTP is deployed at Meta](https://engineering.fb.com/2022/11/21/production-engineering/precision-time-protocol-at-meta/) [summary](/summaries/timing/#meta-ptp){:.sum} <span class="lvl lvl-i">intermediate</span> — tens-of-ns sync at hyperscale, fully open-sourced stack.
- [OCP Time Appliances Project — Time Card](https://github.com/Time-Appliances-Project/Time-Card) [summary](/summaries/timing/#time-card){:.sum} <span class="lvl lvl-a">advanced</span> — open hardware that turns any server into a stratum-1 time appliance.
- [White Rabbit (CERN)](https://white-rabbit.web.cern.ch/) [summary](/summaries/timing/#white-rabbit){:.sum} <span class="lvl lvl-i">intermediate</span> — sub-nanosecond timing, now the IEEE 1588-2019 High Accuracy profile.
- [Deutsche Börse / Eurex White Rabbit service](https://www.eurex.com/ex-en/support/initiatives/archive/high-precision-time-white-rabbit-pilot) [summary](/summaries/timing/#eurex-wr){:.sum} <span class="lvl lvl-b">beginner</span> — how an exchange distributes its own clock to co-location customers.
- [linuxptp documentation](https://linuxptp.nwtime.org/documentation/) [summary](/summaries/timing/#linuxptp){:.sum} <span class="lvl lvl-i">intermediate</span> — ptp4l, phc2sys, ts2phc: the standard Linux PTP stack.
- [MiFID II RTS 25 and time sync on Linux (Red Hat)](https://www.redhat.com/en/blog/mifid-ii-rts-25-and-time-synchronisation-red-hat-enterprise-linux-and-red-hat-virtualization) [summary](/summaries/timing/#mifid-rts25){:.sum} <span class="lvl lvl-b">beginner</span> — the regulatory requirement mapped to Linux capabilities. Note: in the EU, RTS 25 was replaced from 2 Mar 2026 by [Delegated Regulation (EU) 2025/1155](https://eur-lex.europa.eu/eli/reg_del/2025/1155/oj/eng) (same 100 µs limit, finer 0.1 µs granularity for the fastest tier).
- [Real-world PTP accuracy across NICs (scottstuff.net)](https://scottstuff.net/posts/2025/05/07/connectx-4-and-linuxptp/) [summary](/summaries/timing/#nic-ptp-accuracy){:.sum} <span class="lvl lvl-a">advanced</span> — measured 2025 data: NIC choice changes PTP accuracy by 45×.
- [Intel rdtsc benchmarking whitepaper (mirror)](https://cis.temple.edu/~qzeng/cis3207-spring18/files/ia-32-ia-64-benchmark-code-execution-paper.pdf) [summary](/summaries/timing/#rdtsc-paper){:.sum} <span class="lvl lvl-a">advanced</span> — the classic Paoloni paper on cycle-accurate timing (Intel removed the original).

## Measurement, Benchmarks & Jitter Detection {#measurement-benchmarks--jitter-detection}

- [STAC benchmarks](https://docs.stacresearch.com/) [summary](/summaries/tools/#stac){:.sum} <span class="lvl lvl-i">intermediate</span> — STAC-T0/N1/TS: the audited industry benchmarks, with full configuration disclosures.
- [cns-sysjitter](https://github.com/Xilinx-CNS/cns-sysjitter) [summary](/summaries/tools/#sysjitter){:.sum} <span class="lvl lvl-a">advanced</span> — Solarflare's classic OS-jitter meter at its new GitHub home.
- [cyclictest & rt-tests](https://man.archlinux.org/man/cyclictest.8.en) [summary](/summaries/tools/#cyclictest){:.sum} <span class="lvl lvl-i">intermediate</span> — the standard RT latency suite; pair with the in-kernel `rtla` tracer on 5.17+.
- [hiccups](https://github.com/erez-strauss/hiccups) [summary](/summaries/tools/#hiccups){:.sum} <span class="lvl lvl-a">advanced</span> — a simple OS-hiccup meter (unmaintained but readable in one sitting).
- [sockperf](https://github.com/Mellanox/sockperf) [summary](/summaries/tools/#sockperf){:.sum} <span class="lvl lvl-b">beginner</span> — socket-level latency benchmarking with percentile histograms.
- [Corvil Analytics (Pico)](https://www.pico.net/products/corvil-analytics/) [summary](/summaries/tools/#corvil){:.sum} <span class="lvl lvl-b">beginner</span> — what production-grade nanosecond latency monitoring looks like.
- [c2clat](https://github.com/rigtorp/c2clat) & [core-to-core-latency](https://github.com/nviennot/core-to-core-latency) [summary](/summaries/tools/#c2clat){:.sum} <span class="lvl lvl-i">intermediate</span> — characterize inter-core latency before choosing a pinning layout.
- [Google Benchmark](https://github.com/google/benchmark) [summary](/summaries/tools/#google-benchmark-tools){:.sum} <span class="lvl lvl-b">beginner</span> — the standard C++ microbenchmark harness.

## Programming Languages & Code {#programming-languages--code}

### C++

- [When Nanoseconds Matter — David Gross (CppCon 2024 keynote)](https://www.youtube.com/watch?v=sX2nF1fW7kI) [summary](/summaries/software/#gross-2024){:.sum} <span class="lvl lvl-a">advanced</span> — the current definitive HFT C++ talk.
- [Trading at Light Speed — David Gross (Meeting C++ 2022)](https://www.youtube.com/watch?v=8uAW5FQtcvE) [summary](/summaries/software/#gross-2022){:.sum} <span class="lvl lvl-a">advanced</span> — the companion full-system design talk.
- [When a Microsecond Is an Eternity — Carl Cook (CppCon 2017)](https://www.youtube.com/watch?v=NH1Tta7purM) [summary](/summaries/software/#carl-cook){:.sum} <span class="lvl lvl-i">intermediate</span> — the classic introduction to HFT C++.
- [What is Low Latency C++? — Timur Doumler (C++Now 2023)](https://www.youtube.com/watch?v=EzmNeAhWqVs) [summary](/summaries/software/#doumler){:.sum} <span class="lvl lvl-i">intermediate</span> — two parts; the best structured modern treatment.
- [SPSC Lock-free FIFO From the Ground Up — Charles Frasch (CppCon 2023)](https://www.youtube.com/watch?v=K3P_Lmq6pw0) [summary](/summaries/software/#frasch-spsc){:.sum} <span class="lvl lvl-i">intermediate</span> — build the workhorse queue, benchmark every step.
- [C++ Design Patterns for Low-latency Applications (arXiv)](https://arxiv.org/abs/2309.04259) [summary](/summaries/software/#hft-paper){:.sum} <span class="lvl lvl-a">advanced</span> — the HFT folklore, measured with statistical rigor.
- [Google Benchmark](https://github.com/google/benchmark) [summary](/summaries/software/#google-benchmark){:.sum} <span class="lvl lvl-b">beginner</span> — prove your optimization actually helped.

### Concurrency & lock-free

- [C++ Atomics, From Basic to Advanced — Fedor Pikus (CppCon 2017)](https://www.youtube.com/watch?v=ZQFzMfHIxng) [summary](/summaries/software/#pikus-atomics){:.sum} <span class="lvl lvl-a">advanced</span> — what atomics really do; still canonical.
- [Lock-Free Programming — Herb Sutter (CppCon 2014, parts I & II)](https://www.youtube.com/watch?v=c1gO9aB9nbs) [summary](/summaries/software/#sutter-lockfree){:.sum} <span class="lvl lvl-a">advanced</span> — the conceptual foundation (Part II link fixed — the old one was truncated).
- [Erik Rigtorp — essays & libraries](https://rigtorp.se/) [summary](/summaries/software/#rigtorp){:.sum} <span class="lvl lvl-a">advanced</span> — SPSCQueue, MPMCQueue, Seqlock, and the awesome-lockfree list.
- [1024cores — Dmitry Vyukov](https://sites.google.com/site/1024cores/) [summary](/summaries/software/#vyukov-1024cores){:.sum} <span class="lvl lvl-a">advanced</span> — **the old 1024cores.net domain is hijacked; use this Google Sites URL.**

### Java

- [LMAX Disruptor](https://github.com/LMAX-Exchange/disruptor) & [Chronicle Queue](https://github.com/OpenHFT/Chronicle-Queue) [summary](/summaries/software/#disruptor-chronicle){:.sum} <span class="lvl lvl-i">intermediate</span> — the GC-free Java toolkit.
- [Aeron](https://github.com/aeron-io/aeron) [summary](/summaries/software/#aeron){:.sum} <span class="lvl lvl-a">advanced</span> — low-latency messaging and clustering; note the new `aeron-io` org.
- [Evolution of Financial Exchange Architectures — Martin Thompson](https://www.youtube.com/watch?v=qDhTjE0XmkE) [summary](/summaries/software/#thompson-exchange){:.sum} <span class="lvl lvl-i">intermediate</span> — 25 years of exchange architecture in one talk.
- [Azul Platform Prime (Zing) & the C4 collector](https://docs.azul.com/prime/c4-garbage-collection.html) [summary](/summaries/software/#azul-zing){:.sum} <span class="lvl lvl-i">intermediate</span> — pauseless Java; compare with OpenJDK's free generational ZGC (ZGC's only mode since JDK 24).
- Historical: [SubMicroTrading blog](https://submicro.blogspot.com) + [open-sourced code](https://github.com/gsitgithub/SubMicroTrading) [summary](/summaries/software/#submicro){:.sum} · [epickrram (LMAX) jitter posts](https://epickrram.blogspot.com/2015/09/reducing-system-jitter.html) [summary](/summaries/software/#epickrram){:.sum}

### Rust

- [Rust vs C++ for trading systems (Databento)](https://databento.com/blog/rust-vs-cpp) [summary](/summaries/software/#rust-vs-cpp){:.sum} <span class="lvl lvl-i">intermediate</span> — the grounded industry view; pair with their counterpoint, [why the feed handler stayed in C++](https://databento.com/blog/why-we-didnt-rewrite-our-feed-handler-in-rust).
- [hftbacktest](https://github.com/nkaz001/hftbacktest) [summary](/summaries/software/#hftbacktest){:.sum} <span class="lvl lvl-a">advanced</span> — the flagship open-source Rust HFT project.

### Communities & protocols

- [Mechanical Sympathy Google Group](https://groups.google.com/g/mechanical-sympathy) [summary](/summaries/software/#mech-sympathy){:.sum} <span class="lvl lvl-i">intermediate</span> — quieter than its heyday, but the archive is gold.
- [Databento Microstructure Guides](https://databento.com/microstructure/itch) [summary](/summaries/software/#databento-micro){:.sum} <span class="lvl lvl-b">beginner</span> — ITCH, OUCH, MoldUDP64 and tick-to-trade, explained properly.

## Contribute {#contribute}

Found a dead link, or a resource that belongs here? Please [open a GitHub issue](https://github.com/datar/low-latency-system/issues).

## About Me {#about-me}

- [My LinkedIn page](https://www.linkedin.com/in/masterchen/)
- [chenxing@live.com](mailto:chenxing@live.com)
- WeChat: trador
