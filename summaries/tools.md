---
title: "Summaries — Measurement, Benchmarks & Jitter Detection"
description: "Plain-language summaries of the measurement and benchmarking references in the collection."
permalink: /summaries/tools/
translation: /zh/summaries/tools/
nav: home
---

# Reference Summaries: Measurement, Benchmarks & Jitter Detection

[← Back to the reference list](/#measurement-benchmarks--jitter-detection) · Each entry below summarizes one reference from the collection, so you can decide whether it is worth your reading time.

---

## STAC Benchmarks (STAC-T0, STAC-N1, STAC-TS) {#stac}

**Source:** [docs.stacresearch.com](https://docs.stacresearch.com/) · current record: [AMD240422](https://docs.stacresearch.com/news/AMD240422) · <span class="lvl lvl-i">intermediate</span> · ongoing (STAC acquired by n2 Group, parent of NAG, July 2024)

The industry's audited benchmark authority for trading technology: STAC-T0 measures tick-to-trade network-stack round trip (current record 13.9 ns, Exegy/AMD 2024), STAC-N1 measures host network stacks under market-data workloads, and STAC-TS covers timestamping/clock-sync compliance tooling for MiFID II. Reports disclose full hardware/software configurations, which makes them valuable tuning references even if you never run the benchmarks. Free registration required; the old `stacresearch.com/SFC170206` link this site carried now lives at `docs.stacresearch.com/SFC170206`.

## cns-sysjitter — Measure OS-induced Jitter {#sysjitter}

**Source:** [GitHub (Xilinx-CNS)](https://github.com/Xilinx-CNS/cns-sysjitter) · <span class="lvl lvl-a">advanced</span> · tool ~2010s, repo current under AMD/Xilinx

The open-source home of Solarflare's classic `sysjitter` tool (v1.4, by David Riddoch): it spins a thread on each isolated core and records every interval where the thread was knocked off-CPU, producing per-core interruption counts and histograms. This is the ground-truth check after applying `isolcpus`/`nohz_full` tuning — did the ticks, IRQs and SMIs actually go away? Successor URL to the dead openonload.org download page this site previously linked.

## hiccups — Erez Strauss's OS Hiccup Meter {#hiccups}

**Source:** [GitHub](https://github.com/erez-strauss/hiccups) · <span class="lvl lvl-a">advanced</span> · dormant

A small tool measuring OS "hiccups" — scheduling interruptions experienced by a busy-spinning thread — in the same spirit as sysjitter. Still online but unmaintained; kept because it's simple enough to read in one sitting and adapt. For maintained alternatives use cns-sysjitter above, or `cyclictest`/`rtla` from the kernel's rt-tests ecosystem.

## cyclictest & rt-tests {#cyclictest}

**Source:** [man page](https://man.archlinux.org/man/cyclictest.8.en) · [rt-tests at kernel.org](https://git.kernel.org/pub/scm/utils/rt-tests/rt-tests.git) · <span class="lvl lvl-i">intermediate</span> · actively maintained

The standard real-time latency measurement suite: `cyclictest` measures timer-wakeup latency distributions (the canonical RT kernel benchmark), `hwlatdetect` isolates firmware/SMI-induced latency, and the companion `rteval` tool (a separate project) wraps them for whole-system evaluation under load. On modern kernels (5.17+), the in-kernel `rtla` tool with its `timerlat`/`osnoise` tracers is the recommended companion — it tells you not just *that* jitter happened but *which* kernel path caused it.

## sockperf — Socket-level Latency Benchmarking {#sockperf}

**Source:** [GitHub (Mellanox)](https://github.com/Mellanox/sockperf) · <span class="lvl lvl-b">beginner</span> · v3.10, 2022

The go-to network latency microbenchmark from Mellanox/NVIDIA: ping-pong, under-load and throughput modes over TCP/UDP with full percentile histograms (not just averages — in latency work the tail is the story). Works with kernel sockets and kernel-bypass stacks (VMA/XLIO, Onload), which makes it the standard tool for before/after comparisons of NIC and stack tuning. Pair with `iperf3`/`netperf` for throughput and MoonGen for hardware-rate packet generation.

## Corvil (Pico) — Production Latency Analytics {#corvil}

**Source:** [pico.net](https://www.pico.net/products/corvil-analytics/) · <span class="lvl lvl-b">beginner</span> · current (2025 platform)

The dominant commercial platform for passive, packet-level latency measurement in trading: nanosecond timestamping, 200 Gbps sustained capture with real-time analytics (Corvil 12000), 600+ trading-protocol decoders, and MiFID II/RegNMS/CAT compliance reporting with traceable time. Shows what production-grade latency monitoring looks like beyond DIY tools — no agents, no application changes, every order measured from market-data tick to order response. Corvil was acquired by Pico in 2019.

## c2clat & core-to-core-latency {#c2clat}

**Source:** [rigtorp/c2clat](https://github.com/rigtorp/c2clat) · [nviennot/core-to-core-latency](https://github.com/nviennot/core-to-core-latency) · <span class="lvl lvl-i">intermediate</span> · 2020/2022, still the reference

Two open-source tools that measure cache-coherence latency between every pair of CPU cores and render the result as a matrix/heatmap. They expose the topology effects that decide thread-placement strategy: ~8–25 ns within a CCX or between SMT siblings, 85–180 ns across chiplet boundaries, several-fold penalties across sockets. Running one of these is now a standard characterization step for any new trading server — vendors use them in QA too. (Also summarized in the [hardware section](/summaries/hardware/#c2c-latency).)

## Google Benchmark {#google-benchmark-tools}

**Source:** [GitHub](https://github.com/google/benchmark) · <span class="lvl lvl-b">beginner</span> · v1.9.5, January 2026

The standard C++ microbenchmarking harness — statistically robust timing, automatic iteration counts, `DoNotOptimize` barriers and perf-counter integration. Listed here as the code-level complement to the system-level tools above; see the [software section](/summaries/software/#google-benchmark) for the full summary.
