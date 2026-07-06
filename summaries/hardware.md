---
title: "Summaries — Server Hardware & CPUs"
description: "Plain-language summaries of the server hardware and CPU tuning references in the collection."
permalink: /summaries/hardware/
translation: /zh/summaries/hardware/
nav: home
---

# Reference Summaries: Server Hardware & CPUs

[← Back to the reference list](/#server-hardware--cpus) · Each entry below summarizes one reference from the collection, so you can decide whether it is worth your reading time.

---

## AMD EPYC 9005 BIOS & Workload Tuning Guide (Pub 58467) {#amd-9005-tuning}

**Source:** [docs.amd.com](https://docs.amd.com/v/u/en-US/58467_amd-epyc-9005-tg-bios-and-workload) · <span class="lvl lvl-a">advanced</span> · rev 2.2, June 2026

AMD's official tuning guide for 5th-gen EPYC "Turin" (9005-series) servers — the successor to the retired 2018 developer.amd.com tuning PDF this site used to link. It walks through every latency-relevant BIOS knob on the SP5 platform: power/performance determinism, C-state control, SMT, NUMA-per-socket (NPS) modes, Infinity Fabric and memory-clock configuration. Workload-specific recommendation tables map settings directly to a low-latency trading build on chips like the EPYC 9575F. The current canonical AMD server tuning document.

## Dell PowerEdge 16G BIOS Tuning Guide — Low Latency {#dell-16g}

**Source:** [infohub.delltechnologies.com](https://infohub.delltechnologies.com/en-us/l/poweredge-bios-performance-and-workload-tuning-guide-for-16g/low-latency-17/) · <span class="lvl lvl-i">intermediate</span> · 2023–2024, maintained

Dell's modern replacement for its classic 2010 "Configuring Low-Latency Environments on PowerEdge Servers" whitepaper (now dead). The low-latency chapter explains which System Profile to select and how it disables deep C-states, locks the CPU governor and tunes memory frequency for deterministic response; surrounding chapters give the full BIOS option reference so you understand the throughput/power/latency trade-off each setting makes. Directly actionable for trading workloads on commodity Dell servers.

## Configuring HPE ProLiant Servers for Low-Latency Applications {#hpe-low-latency}

**Source:** [hpe.com/psnow](https://www.hpe.com/psnow/doc/c01804533) · <span class="lvl lvl-i">intermediate</span> · updated through ProLiant Gen11

HPE's long-running low-latency whitepaper — the same document ID this site linked since 2018, now living at a new URL after HPE retired its old support portal. It covers BIOS workload profiles, disabling power-management SMIs (a classic source of multi-microsecond jitter on HPE boxes), jitter-smoothing options, and OS-level recommendations. Note that HPE's overclocked "Trade and Match" Apollo servers are discontinued; boutique vendors now fill that niche, but this guide is still the reference for tuning standard ProLiants.

## Intel Xeon 6 Performance & Power Profiles (incl. Latency Optimized Mode) {#intel-xeon6-profiles}

**Source:** [builders.intel.com PDF](https://builders.intel.com/docs/networkbuilders/intel-xeon-6-processors-performance-and-power-profiles-1746785299.pdf) · <span class="lvl lvl-a">advanced</span> · April 2025

Intel's own technical article documenting Xeon 6 (Granite Rapids) performance and power profiles: the out-of-box default, **Latency Optimized Mode** (holds uncore/mesh frequency high instead of downclocking) and Efficiency Latency Control (ELC). Explains what each profile does to core/uncore frequency behavior and when to choose latency-optimized settings over performance-per-watt defaults. The closest modern successor to Intel's retired "Optimizing Computer Applications for Latency" article series.

## Phoronix — Xeon 6 Latency Optimized Mode Benchmarks {#phoronix-lom}

**Source:** [phoronix.com](https://www.phoronix.com/review/intel-latency-optimized-mode) · <span class="lvl lvl-i">intermediate</span> · early 2025

An independent benchmark study of Intel's Latency Optimized Mode, quantifying a setting low-latency practitioners previously approximated by manually pinning uncore frequency: roughly 17% geometric-mean performance improvement across a broad suite, with acceptable power cost. Also explains how the feature interacts with ELC policies and RAPL/TDP limits. Essential reading when specifying or tuning Xeon 6 trading servers.

## Xeon 6 High-Priority Cores (Intel SST-BF) Explained {#sth-sst}

**Source:** [servethehome.com](https://www.servethehome.com/intel-xeon-6-high-priority-and-low-priority-cores-explained/) · <span class="lvl lvl-b">beginner</span> · 2024–2025

A clear explainer of Intel Speed Select Base Frequency (SST-BF), which lets a subset of "high-priority" cores run at a guaranteed higher base frequency while the rest run lower. For latency-sensitive systems this means hot trading threads can be pinned to guaranteed-fast cores for deterministic clocks regardless of load. A good conceptual primer on why frequency asymmetry became a necessary tool as core counts inflated.

## Chips and Cheese — Xeon 6 Memory Subsystem Deep Dive {#chips-cheese-xeon6}

**Source:** [chipsandcheese.com](https://chipsandcheese.com/p/a-look-into-intel-xeon-6s-memory) · <span class="lvl lvl-a">advanced</span> · September 2025

A microarchitectural analysis measuring exactly the numbers that drive CPU selection for trading: Xeon 6 L3 latency (~33 ns), local DRAM latency (~131 ns), and core-to-core latency (50–80 ns, even across EMIB-stitched compute dies). Directly compares Intel's mesh against AMD EPYC's CCD/Infinity Fabric design: Intel wins on cross-die cache coherence (core-to-core messaging), AMD wins on raw local memory latency. Illustrates why core-to-core latency has become a first-class hardware selection metric.

## core-to-core-latency — Measure Your CPU's Topology {#c2c-latency}

**Source:** [GitHub](https://github.com/nviennot/core-to-core-latency) · <span class="lvl lvl-i">intermediate</span> · 2022, still the reference dataset

An open-source Rust tool that measures inter-core communication latency via cache-coherence operations, with a large community results database across Intel, AMD, ARM and Apple CPUs. The heatmaps visualize topology effects that matter enormously in trading systems: ~8–25 ns within a CCX, 85–180 ns across CCD boundaries, 2–3.6× penalties across sockets. Trading firms and overclocked-server vendors use this class of tool (alongside rigtorp's `c2clat`) to validate thread-pinning maps and shortlist CPUs.

## Blackcore — Why Overclocked Servers Still Win in Trading {#blackcore}

**Source:** [blackcoretech.com](https://www.blackcoretech.com/knowledge/maximize-your-trading-potential-embrace-overclocking-to-stay-ahead-of-the-curve) · <span class="lvl lvl-b">beginner</span> · 2023–2025 (vendor content)

A vendor perspective from the leading maker of overclocked trading servers on why overclocking still wins: beyond raw clock speed, tuned systems cut RAM latency (~34% claimed), cache access time (~30%) and PCIe latency versus stock servers. The wider Knowledge Center covers binning, liquid cooling, 16-hour burn-in and validation tooling (Prime95, Intel MLC, pcie-lat, c2clat). Vendor bias acknowledged — but it paints a concrete picture of what differentiates a purpose-built HFT server from an enterprise box, a niche HPE abandoned when it retired the Apollo "Trade and Match" line.

## hft-server-settings — Community Checklist {#hft-server-settings}

**Source:** [GitHub](https://github.com/twanas/hft-server-settings) · <span class="lvl lvl-i">intermediate</span> · ~2020–2024

A community-maintained checklist of concrete server settings used in real HFT deployments: BIOS items (disable hyper-threading and deep C-states, static high-performance profile, NUMA guidance), kernel boot parameters, IRQ/NIC/socket alignment and CPU-pinning conventions. Its value is brevity — it condenses the vendor whitepapers into the settings practitioners actually change. A good quick-reference companion to the fuller Rigtorp and Red Hat guides.

## Optimizing Computer Applications for Latency — Intel (parts 1 & 2) {#intel-latency-articles}

**Source:** [Part 1: hardware](https://www.intel.com/content/www/us/en/developer/articles/technical/optimizing-computer-applications-for-latency-part-1-configuring-the-hardware.html) · [Part 2: applications](https://www.intel.com/content/www/us/en/developer/articles/technical/optimizing-computer-applications-for-latency-part-2-tuning-applications.html) · <span class="lvl lvl-i">intermediate</span> · ~2017 (historical)

Intel's classic two-part article series, relocated after software.intel.com was retired (the old URLs this site carried are dead). Part 1 covers hardware configuration — C-states, Turbo, memory population, BIOS settings; Part 2 covers application tuning — profiling with VTune, cache optimization and threading. The concepts still hold, but for current silicon read them alongside the Xeon 6 profile documents above.

## Lenovo/IBM Linux Performance and Tuning Guidelines {#lenovo-redp}

**Source:** [lenovopress.lenovo.com](https://lenovopress.lenovo.com/redp4285) · <span class="lvl lvl-b">beginner</span> · 2008 (historical — superseded)

The old IBM Redpaper this site linked for years, still online at Lenovo Press but frozen at April 2008 (IBM System x, kernel 2.6). Kept only as a historical curiosity: its structure (understand, measure, then tune CPU/memory/disk/network) shaped a generation of tuning guides, but every specific recommendation is obsolete. Use the RHEL 9 documentation and the vendor guides above instead.
