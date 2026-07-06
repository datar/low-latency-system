---
title: "The Complete Guide to Low Latency Trading Systems"
description: "One page that organizes the entire topic of low-latency trading systems, from beginner concepts to expert techniques, with 2026 state-of-the-art numbers."
permalink: /guide/
translation: /zh/guide/
nav: guide
---

# The Complete Guide to Low-Latency Trading Systems

*One page, beginner to expert. Updated July 2026. Every technique here links back to a reference in the [collection](/) where you can go deeper.*

## Contents
{:.no_toc}

* TOC
{:toc}

---

## 1. What "low latency" means (start here) {#what-low-latency-means}

**Latency** is the time between a cause and its effect: a price changes on an exchange (*tick*), and your system sends an order in response (*trade*). That interval is called **tick-to-trade latency**, and it is the headline metric of this whole field ([Databento's definition](https://databento.com/microstructure/tick-to-trade) is the best modern one).

Why does it matter? In electronic markets, opportunities are claimed in arrival order. If two firms see the same mispricing, the faster one trades and the slower one misses — or worse, trades on stale information. "Low latency" in trading therefore doesn't mean "fast on average"; it means **fast and predictable at the tail**. A system that responds in 1 µs on average but occasionally takes 1 ms is broken: the slow responses cluster exactly when markets move and it matters most. That's why this discipline obsesses about **jitter** (variance) as much as speed, and why every serious measurement is reported as percentiles (p99, p99.9, max), never averages.

**The scale of things** — internalize these orders of magnitude before anything else:

| Time | What happens in it |
|---|---|
| 1 ns | Light travels ~30 cm; ~4-5 CPU clock cycles |
| ~1–4 ns | L1 cache hit; a layer-1 switch forwards a packet (Arista 7130: ~4 ns) |
| ~14 ns | The fastest published FPGA tick-to-trade (STAC-T0 record, 2024) |
| ~30–40 ns | L3 cache hit; FPGA market-data mux (MetaMux: ~39 ns) |
| ~100 ns | DRAM access; core-to-core message across CPU chiplets |
| ~500 ns–2 µs | A whole software tick-to-trade path with kernel bypass |
| ~10–50 µs | A well-tuned kernel network round trip; one bad TLB shootdown |
| ~1 ms | A garbage-collection pause you failed to design away; ~150 km of fiber |

Two consequences fall out of this table. First, **physics is a hard budget**: at nanosecond scale, cable length matters, which is why firms pay for co-location racks meters from the exchange's matching engine and why microwave links (which beat fiber's speed of light in glass) exist between Chicago and New York. Second, **every layer counts**: a perfectly tuned application loses if the OS interrupts it, the NIC buffers it, or the switch queues it. Low latency is a *full-stack* property.

> **Beginner reading path:** [Tick-to-trade latency](/summaries/networking/#tick-to-trade) → [Kernel bypass (Cloudflare)](/summaries/networking/#cf-kernel-bypass) → [Matt Godbolt on how CPUs work](/summaries/software/#godbolt-cpu) → [Timur Doumler, What is Low Latency C++?](/summaries/software/#doumler)

## 2. Anatomy of a trading system: where the nanoseconds go {#anatomy}

A minimal electronic trading system looks like this:

```
Exchange ──market data (UDP multicast)──▶ [switch] ─▶ [NIC] ─▶ feed handler
                                                                    │
                                                              order book / strategy
                                                                    │
Exchange ◀──order entry (TCP)─────────── [switch] ◀─ [NIC] ◀─ execution gateway
```

The tick-to-trade budget is spent, roughly in order:

1. **Wire → NIC (network)**: propagation through fiber/copper, switch forwarding, NIC PHY/MAC. Layer-1 switches cut this to ~4 ns; note that at 25GbE, forward error correction (FEC) alone adds ~100 ns, so ultra-low-latency shops run FEC-off over short links.
2. **NIC → application (host ingress)**: DMA, then either the kernel network stack (microseconds) or a kernel-bypass stack reading directly from user space (hundreds of nanoseconds).
3. **Decode & book build**: parsing the exchange protocol (e.g. ITCH over MoldUDP64) and updating an order book — pure CPU/cache work.
4. **Decision**: the strategy logic itself. In competitive systems this is nanoseconds — precomputed decisions, table lookups — not "run a model".
5. **Order out (host egress + network)**: serialize the order (e.g. OUCH over SoupBinTCP), through the NIC (cut-through transmit features like CTPIO help), back through the switch.

Each of the following sections attacks one layer of this budget. The expert insight is that **the layers interact**: e.g. a NUMA-wrong NIC placement poisons steps 2–5; an untamed kernel timer tick adds jitter to steps 3–4 no matter how good the code is.

**The three technology tiers** (2026 numbers):

| Tier | Tick-to-trade | Cost & flexibility |
|---|---|---|
| Tuned software (kernel bypass, C++/Rust) | ~1–2 µs | Cheapest, most flexible; where everyone starts |
| FPGA (hybrid CPU+FPGA) | ~50–500 ns; record 13.9 ns | Hardware team required; the mainstream competitive tier |
| ASIC / eFPGA | ~10–50 ns deterministic | Top-tier firms only; rigid, capital-intensive |

## 3. Level 1 — Measure first (the non-negotiable discipline) {#measure-first}

Every expert source agrees on the order of operations: **you cannot tune what you cannot measure**. Before changing a single BIOS flag:

- **Benchmark code** with [Google Benchmark](/summaries/software/#google-benchmark) and cycle-accurate timing ([the rdtsc methodology](/summaries/timing/#rdtsc-paper)); profile with `perf` and [Brendan Gregg's toolset](/summaries/linux-tuning/#gregg) — flame graphs show where time goes, eBPF/bpftrace shows why.
- **Measure OS jitter** with [cns-sysjitter](/summaries/tools/#sysjitter) or [cyclictest/rtla](/summaries/tools/#cyclictest): spin a thread on each core and record every interruption. This is the ground truth for whether your isolation tuning actually works.
- **Measure the machine's topology** with [core-to-core-latency or c2clat](/summaries/tools/#c2clat): a heatmap of communication cost between every core pair, which dictates thread placement.
- **Measure the wire** — production systems use passive taps with nanosecond timestamps ([Corvil/Pico](/summaries/tools/#corvil)); the industry's audited comparisons are the [STAC benchmarks](/summaries/tools/#stac).
- **Report percentiles**, never averages. The p99.9 and max are the numbers that decide whether you make or lose money.

A useful habit from the practitioners' guides: change one thing at a time, and keep a jitter measurement running while you do. Many "optimizations" trade mean for tail.

## 4. Level 2 — The machine: hardware selection & BIOS {#hardware}

*Full section references: [Server Hardware & CPUs](/#server-hardware--cpus).*

**CPU choice (2026).** The latency path wants *few, fast, cache-rich cores*, not many slow ones:

- **Overclocked boutique servers** (Blackcore, Hypertec CIARA) are the top software tier: liquid-cooled, binned, overclocked Xeon/Core parts at ~5+ GHz all-core, with pre-tuned BIOS profiles and burn-in. Overclocking helps more than clocks: it cuts RAM latency (~34%), cache latency (~30%) and PCIe latency. (HPE's old "Trade and Match" overclocked line is discontinued — this niche went boutique.)
- **AMD EPYC 9005 F-series** (e.g. 9575F: 64 cores, 5.0 GHz boost, 256 MB L3) is the designated high-frequency server SKU where standard iron is required; **EPYC 4004/4005 X3D** parts bring 3D V-Cache (128 MB L3) to cheap single-socket builds — huge L3 keeps the order book cache-resident (~49-cycle L3 vs ~350-cycle DRAM).
- **Intel Xeon 6** counters with Latency Optimized Mode (holds mesh/uncore frequency high — [~17% measured gain](/summaries/hardware/#phoronix-lom)), Efficiency Latency Control, and SST-BF (a subset of cores at guaranteed higher base frequency for your hot threads).
- **Topology beats brand**: Intel's mesh gives 50–80 ns core-to-core even across dies; AMD is ~8–25 ns inside a CCX but 150–180 ns across CCDs. Measure with c2clat and pin communicating threads accordingly.

**Settled architecture decisions:**

- **Single socket** for the latency path. Cross-socket hops cost 2–3.6× on core-to-core latency; put the NIC, its memory and the pinned cores on one socket/NUMA node. Dual-socket boxes are for throughput work (risk, backtesting).
- **BIOS canon** ([AMD guide](/summaries/hardware/#amd-9005-tuning), [Dell](/summaries/hardware/#dell-16g), [HPE](/summaries/hardware/#hpe-low-latency), [community checklist](/summaries/hardware/#hft-server-settings)): SMT/Hyper-Threading **off**; C-states beyond C1 **off** (or all off); maximum-performance/deterministic power profile; turbo on; uncore frequency fixed high; power/utilization-monitoring SMIs **off** (SMIs are invisible multi-microsecond stalls); memory at max rated speed, all channels populated; unused devices disabled.

## 5. Level 3 — The operating system: Linux tuning {#linux}

*Full section references: [Linux OS Tuning](/#linux-os-tuning).*

The goal: give your hot threads **dedicated, undisturbed cores**. Modern Linux can get microsecond-class jitter down to almost nothing, but only with deliberate configuration. The layered recipe, from the [Red Hat](/summaries/linux-tuning/#rhel-rt), [Rigtorp](/summaries/linux-tuning/#rigtorp-guide) and [SUSE](/summaries/linux-tuning/#suse-cpu-isolation) references:

1. **Partition the machine.** Housekeeping (kernel threads, IRQs, daemons) on a few cores; everything else isolated for the application. The `tuned` **cpu-partitioning** profile automates this and is Red Hat's recommended starting point.
2. **Isolate properly — `isolcpus` alone is not isolation.** It removes cores from the scheduler but timer ticks and RCU callbacks still fire ([a real case study: 200 µs spikes every 4 ms on "isolated" cores](/summaries/linux-tuning/#isolcpus-hft)). The full recipe is `isolcpus=` (or cgroup-v2 isolated partitions) **+ `nohz_full=`** (adaptive tickless — one residual tick/second) **+ `rcu_nocbs=`** (offload RCU callbacks) **+ `irqaffinity=`** (steer interrupts to housekeeping cores).
3. **Kill frequency and sleep-state transitions.** Performance governor; limit C-states via `cpu_dma_latency` PM-QoS (tuned's latency profiles hold it open) — waking from a deep C-state costs tens of microseconds.
4. **Tame memory.** Static hugepages (fewer TLB misses), `mlockall()` (no page faults), swap off, transparent hugepages off, automatic NUMA balancing off. Beware [TLB shootdowns](/summaries/linux-tuning/#tlb-shootdowns): `munmap`/`madvise` from any thread can interrupt every core in the process — preallocate and never free on the hot path.
5. **Decide on speculative-execution mitigations.** `mitigations=off` is standard on physically-secured, single-tenant trading hosts (historically 15–25% penalty when on; newer CPUs fix more in hardware). Do the risk assessment per host.
6. **Consider PREEMPT_RT.** Real-time preemption is **in mainline Linux since 6.12 (Nov 2024)** — no more out-of-tree patches — and `kernel-rt` ships with standard RHEL 9/10. Note RT optimizes *worst-case* scheduling latency, sometimes at a small throughput cost; many trading loads prefer a tuned standard kernel plus busy-polling. Red Hat's rule of thumb: standard-kernel tuning gets ~90% of the win.
7. **Verify.** `rtla timerlat`/`osnoise` (kernel 5.17+), sysjitter, cyclictest — after every change.

## 6. Level 4 — The network path {#network}

*Full section references: [Networking, NICs & Kernel Bypass](/#networking-nics--kernel-bypass).*

**Understand the kernel path first** ([CUBRID mental model](/summaries/networking/#cubrid), [packagecloud deep dive](/summaries/networking/#packagecloud), [consolidated modern guide](/summaries/networking/#ntk148v)). The kernel stack costs microseconds per packet (syscalls, copies, interrupts, softIRQs) and tops out around ~1M pps/core — but correct tuning (multi-queue NICs, RSS/RFS, IRQ affinity, NUMA-local everything, busy-polling) buys a lot: [1M+ pps without bypass](/summaries/networking/#cf-million-packets), [5× on a real server, every knob measured](/summaries/networking/#talawah). And there's a [good case for staying in the kernel](/summaries/networking/#cf-kernel-tcp) everywhere the last microsecond doesn't pay: you keep routing, firewalling, tooling and operability.

**Kernel bypass** is the standard on the trading hot path. The stack maps the NIC into user space; the app polls (spins) instead of taking interrupts:

- **AMD Solarflare**: [Onload](/summaries/networking/#openonload) transparently accelerates unmodified sockets apps (open source since 2021; runs on other NICs via AF_XDP); **ef_vi** is the raw layer-2 API at the latency floor; **TCPDirect** sits between. All in [UG1586](/summaries/networking/#onload-guide).
- **NVIDIA (Mellanox heritage)**: **XLIO** replaced VMA for socket acceleration; [Rivermax + NEIO FastSockets](/summaries/networking/#rivermax) (~0.25 µs serialization) is the 2025 market-data pitch; DPDK's mlx5 driver for full-custom datapaths.
- **DPDK** remains the gold standard when you own the entire packet path (feed handlers, gateways). **AF_XDP** is the "fast enough, operationally simpler" middle path; **io_uring** helps async I/O generally but is not a tick-to-trade tool.

**NICs (2026)**: AMD Solarflare **X4** (Oct 2025, custom ASIC, PCIe Gen5, CTPIO cut-through TX) leads the software-trading segment; **X3522**/X2522 widely deployed; [Cisco Nexus SmartNIC (ex-ExaNIC)](/summaries/networking/#exanic) still delivers 568 ns trigger-to-response but is in maintenance mode; NVIDIA ConnectX-7/8 common for market-data fan-out, capture and RoCE backends.

**Switches**: [Arista 7130](/summaries/networking/#arista-7130) owns the ultra-low-latency segment since Cisco end-of-saled the ex-Exablaze Nexus 3550 line (2023): layer-1 forwarding ~4 ns, MetaMux FPGA multiplexing ~39 ns, MetaWatch tapping with sub-ns timestamps. Conventional ULL switches (~150–250 ns) serve aggregation tiers.

**RDMA/InfiniBand**: role change since 2018 — exchange-facing paths are Ethernet UDP multicast; RDMA survives *inside* the firm as RoCEv2 fabrics (distribution, storage, capture, ~2–5 µs class). The [RDMA programming manual](/summaries/networking/#rdma-manual) is still the reference if you build those.

## 7. Level 5 — The code {#code}

*Full section references: [Programming Languages & Code](/#programming-languages--code).*

Language is the smallest choice; **mechanical sympathy** — writing code the CPU can execute predictably — is the real discipline:

- **Own the cache.** Data layout dominates: contiguous arrays over node-based containers, sorted vectors over maps, one cache line per writer (avoid false sharing — pad with `std::hardware_destructive_interference_size`). The order book is a data-structure problem before it is an algorithm problem ([Gross 2024](/summaries/software/#gross-2024)).
- **Nothing slow on the hot path.** No allocations, no syscalls, no exceptions thrown, no locks, no page faults. Preallocate everything; pin threads; busy-poll instead of sleeping. Keep the hot path *hot*: even branch predictors and instruction caches go cold if you run it rarely — hence **cache warming** with dummy traffic, one of the two biggest measured wins in the [Imperial College study](/summaries/software/#hft-paper) (the other: `constexpr` — compute at compile time).
- **Concurrency: message passing over sharing.** The canonical design is single-threaded business logic fed by lock-free SPSC queues ([build one from scratch](/summaries/software/#frasch-spsc), [rigtorp's implementations](/summaries/software/#rigtorp)). Learn the memory model properly ([Pikus on atomics](/summaries/software/#pikus-atomics), [Sutter's two-parter](/summaries/software/#sutter-lockfree), [Vyukov's 1024cores](/summaries/software/#vyukov-1024cores)) — and remember "lock-free" is about progress guarantees, not automatic speed.
- **C++** remains the industry default (the CppCon canon above, now led by Optiver's talks). **Rust** reached effective latency parity and is growing fast — [Databento's comparison](/summaries/software/#rust-vs-cpp) is the honest 2026 consensus; C++ keeps the ecosystem edge. **Java** works at low microseconds if you refuse to create garbage: [LMAX Disruptor, Chronicle Queue](/summaries/software/#disruptor-chronicle), [Aeron](/summaries/software/#aeron) for messaging/clustering, and pauseless GC via [Azul C4](/summaries/software/#azul-zing) or OpenJDK's generational ZGC (JDK 21+).
- **Know the protocols.** Real systems speak ITCH/OUCH over MoldUDP64/SoupBinTCP or exchange equivalents — [Databento's microstructure guides](/summaries/software/#databento-micro) are the approachable reference.
- **Architecture lesson from exchanges themselves** ([Thompson, Evolution of Financial Exchange Architectures](/summaries/software/#thompson-exchange)): deterministic single-threaded logic + sequenced event logs + replicated state machines beats distributed locking on both latency and correctness.

## 8. Level 6 — Hardware acceleration: FPGA & ASIC {#fpga}

*Full section references: [FPGA & Hardware Acceleration](/#fpga--hardware-acceleration).*

When software runs out of road (~1 µs), the datapath moves into hardware:

- **What an FPGA buys**: deterministic pipeline latency — parse, book-build, decide and emit the order entirely in gateware, unaffected by caches, schedulers or interrupts. Typical competitive tick-to-trade: tens to hundreds of nanoseconds ([an implementable ITCH/OUCH pipeline](/summaries/fpga/#dgway-itch-ouch)).
- **State of the art**: AMD's [Alveo UL3524](/summaries/fpga/#ul3524) (2023) was the first FPGA card purpose-built for trading (<3 ns transceivers); on it, Exegy + AMD set the [13.9 ns STAC-T0 record](/summaries/fpga/#stac-t0-record) (2024) using an asynchronous (clockless) critical path with ~200 ps jitter. The cheaper UL3422 (2024) pushed the tier down-market.
- **The usual architecture is hybrid**: FPGA handles the wire-speed hot path; software (kernel-bypass C++) handles strategy, risk and everything that changes daily. Vendors sell the building blocks — [Orthogone](/summaries/fpga/#orthogone) (17.1 ns MAC/PCS, 6.2 ns TX TCP offload), Exegy/Enyx, [Algo-Logic](/summaries/fpga/#algo-logic), Xelera (FPGA ML inference).
- **The frontier**: top firms burn deterministic functions into **ASICs** (10–50 ns, rigid, capital-intensive) or eFPGA-in-ASIC hybrids; the next battleground is inline ML inference on the critical path ([strategic overview](/summaries/fpga/#ateam-fpga)).

## 9. Time: synchronization & timestamping {#time}

*Full section references: [Time Synchronization & Timestamping](/#time-synchronization--timestamping).*

You cannot measure — or legally operate — a trading system without serious clocks:

- **Regulation sets the floor**: EU MiFID II RTS 25 requires HFT firms to stay within **100 µs of UTC with 1 µs timestamp granularity**, documented and traceable ([the requirement mapped to Linux](/summaries/timing/#mifid-rts25); note RTS 25's successor is being drafted in the ongoing MiFID review).
- **PTP (IEEE 1588-2019) with hardware timestamping** is the workhorse: [linuxptp](/summaries/timing/#linuxptp) (`ptp4l` + `phc2sys`) on a hardware-timestamping NIC reaches ~3 µs conservatively, ~10 ns on good hardware — but [NIC choice changes accuracy by 45×](/summaries/timing/#nic-ptp-accuracy), so measure. [Meta's deployment](/summaries/timing/#meta-ptp) (~436 ns worst-case, open-source stack) is the reference architecture.
- **Open hardware ate the grandmaster**: the [OCP Time Card](/summaries/timing/#time-card) (GNSS + atomic clock on a PCIe card) turns any server into a stratum-1 appliance.
- **Sub-nanosecond**: [White Rabbit](/summaries/timing/#white-rabbit) — CERN technology, now the PTP High Accuracy profile — is offered by [Deutsche Börse as a co-location service](/summaries/timing/#eurex-wr): you sync to the exchange's own clock.
- **Inside the box**: cycle-accurate timing via invariant TSC and the [rdtsc methodology](/summaries/timing/#rdtsc-paper).

## 10. The roadmap: beginner → expert {#roadmap}

**Stage 0 — Foundations (weeks).** Understand the domain and the machine. Read [tick-to-trade](/summaries/networking/#tick-to-trade); watch [Godbolt on CPUs](/summaries/software/#godbolt-cpu) and [Doumler's two-parter](/summaries/software/#doumler); learn the [kernel network path](/summaries/networking/#cubrid). Deliverable: you can explain where a microsecond goes.

**Stage 1 — Measure (weeks).** Set up Google Benchmark, `perf` + flame graphs, sysjitter and c2clat on a Linux box. Baseline everything. Deliverable: a latency/jitter report for stock hardware and kernel.

**Stage 2 — Tune the box (a month).** Apply the [BIOS canon](/summaries/hardware/#hft-server-settings) and the [Linux recipe](/summaries/linux-tuning/#rigtorp-guide): cpu-partitioning, `nohz_full`, IRQ steering, hugepages, governor. Re-measure after each change. Deliverable: isolated cores that sysjitter shows are actually quiet.

**Stage 3 — Tune the network (a month).** Multi-queue/RSS/IRQ-affinity/busy-poll in-kernel first ([Cloudflare](/summaries/networking/#cf-million-packets), [talawah](/summaries/networking/#talawah)); then kernel bypass with [Onload](/summaries/networking/#openonload) or XLIO; measure with [sockperf](/summaries/tools/#sockperf). Deliverable: a sub-2 µs echo path, with histograms to prove it.

**Stage 4 — Write hot-path code (months).** Build an SPSC queue [from scratch](/summaries/software/#frasch-spsc); learn [atomics](/summaries/software/#pikus-atomics) properly; write an ITCH parser and order book with cache-conscious layout; benchmark against the [Imperial study's findings](/summaries/software/#hft-paper). Watch the [Optiver talks](/summaries/software/#gross-2024) with code open. Deliverable: a feed-handler + book that never allocates or syscalls on the hot path.

**Stage 5 — Production-grade (ongoing).** PTP time sync with hardware timestamps; passive capture and percentile monitoring; deterministic replay-based architecture ([Thompson](/summaries/software/#thompson-exchange)); risk checks in the path. This is where most careers in the field live.

**Stage 6 — Hardware tier (specialist).** FPGA development against real protocols ([Design Gateway walkthrough](/summaries/fpga/#dgway-itch-ouch)), vendor IP, STAC-T0-style measurement. A different skill set — most firms buy before they build.

---

*Found an error or a missing resource? [Open an issue](https://github.com/datar/low-latency-system/issues). The full reference list with per-item summaries is on the [home page](/), and the research behind this 2026 update is in the [research notes](/research/).*
