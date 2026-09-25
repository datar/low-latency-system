---
title: "Summaries — Linux OS Tuning"
description: "Plain-language summaries of every Linux OS tuning reference in the collection."
permalink: /summaries/linux-tuning/
translation: /zh/summaries/linux-tuning/
nav: home
---

# Reference Summaries: Linux OS Tuning

[← Back to the reference list](/#linux-os-tuning) · Each entry below summarizes one reference from the collection, so you can decide whether it is worth your reading time.

---

## Optimizing RHEL for Real Time for Low Latency Operation (RHEL 9/10) {#rhel-rt}

**Source:** [docs.redhat.com (RHEL 10)](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux_for_real_time/10/html-single/optimizing_rhel_for_real_time_for_low_latency_operation/index) · [RHEL 9 edition](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux_for_real_time/9/html-single/optimizing_rhel_9_for_real_time_for_low_latency_operation/index) · <span class="lvl lvl-i">intermediate</span> · continuously updated (2022–2026)

Red Hat's current canonical guide for tuning a RHEL system to consistently low, predictable latency — the direct successor of the old RHEL 7 low-latency tuning article that this site linked for years. It covers TuneD real-time profiles, scheduler policies and priorities, CPU isolation and IRQ binding, memory locking and hugepages, avoiding TCP-stack and journaling latency, and running hardware/firmware latency tests with `hwlatdetect` and `rteval`. Since RHEL 9, the real-time kernel (`kernel-rt`) is included with standard RHEL — no separate product needed. Essential baseline reading for anyone deploying trading systems on RHEL 9/10.

## Monitoring and Managing System Status and Performance (RHEL 9/10) {#rhel-perf}

**Source:** [docs.redhat.com (RHEL 10)](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/monitoring_and_managing_system_status_and_performance/index) · [RHEL 9 edition](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html-single/monitoring_and_managing_system_status_and_performance/index) · <span class="lvl lvl-b">beginner</span> · continuously updated

The modern replacement for the retired RHEL 7 *Performance Tuning Guide*. It introduces the concepts and tools of system performance work on a current RHEL: TuneD profiles, PCP/perf monitoring, network, memory, disk and CPU tuning chapters. Concepts transfer to CentOS Stream, Rocky, Alma and — with different tooling — other distributions. Read this first if you are new to Linux performance tuning; then go deeper with the real-time guide above.

## Erik Rigtorp — Low Latency Tuning Guide {#rigtorp-guide}

**Source:** [rigtorp.se](https://rigtorp.se/low-latency-guide/) · <span class="lvl lvl-i">intermediate</span> · 2020

The single most cited practitioner's checklist for tuning x86_64 Linux for HFT-style workloads, written by a trading-systems engineer. It walks through BIOS and power settings, SMT, turbo, the performance governor, core isolation with `isolcpus`/`nohz_full`, IRQ affinity, disabling swap/THP/NUMA-balancing/mitigations, cache partitioning (Intel CAT), `mlockall`, hugepages, and TLB-shootdown avoidance. Each recommendation explains the jitter mechanism it eliminates, and the companion `hiccups` tool lets you measure the resulting OS jitter. Written in 2020 but still overwhelmingly current.

## The Realtime Preemption End Game (LWN) {#lwn-preempt-rt}

**Source:** [lwn.net](https://lwn.net/Articles/989212/) · <span class="lvl lvl-i">intermediate</span> · September 2024

LWN's definitive account of PREEMPT_RT finally merging into mainline Linux 6.12, ending roughly twenty years of out-of-tree real-time patches. It explains what real-time preemption does (sleeping spinlocks, threaded interrupts), why the `printk` rewrite was the last blocker, and what mainlining means in practice: any stock 6.12+ kernel on x86/ARM64/RISC-V (later also LoongArch in 6.13 and 32-bit ARM in 7.1) can be built with full RT preemption, with in-tree drivers and no patch maintenance. Context every trading-system builder should have when choosing a kernel in 2026.

## SUSE Labs — CPU Isolation Series (Frederic Weisbecker) {#suse-cpu-isolation}

**Source:** [suse.com](https://www.suse.com/c/cpu-isolation-practical-example-part-5/) · <span class="lvl lvl-a">advanced</span> · 2021–2022

A six-part deep dive by the kernel maintainer of `nohz_full` into what CPU isolation actually does inside the kernel: full-dynticks internals, the residual 1 Hz tick, housekeeping CPUs, `rcu_nocbs` offloading, and a hands-on worked example of configuring and verifying a fully isolated CPU, including TSC/clocksource troubleshooting. This is the authoritative explanation behind the kernel parameters that every low-latency deployment copies from checklists. Written by the same engineer whose CPU-isolation documentation was merged into the mainline kernel admin guide in 2026.

## CPU Isolation for HFT: What `isolcpus` Doesn't Do {#isolcpus-hft}

**Source:** [nikhilpadala.com](https://nikhilpadala.com/blog/cpu-optimization-linux-latency/) · <span class="lvl lvl-i">intermediate</span> · December 2024

A recent practitioner article debunking the belief that `isolcpus` alone isolates a core: it removes the core from scheduler load balancing but does not stop timer ticks or RCU callbacks, which still inject jitter. It uses a real trading-infrastructure case study — unexplained 200 µs spikes every 4 ms on "isolated" cores — to motivate the full `isolcpus` + `nohz_full` + `rcu_nocbs` recipe, with step-by-step configuration and verification commands, and an honest discussion of the trade-offs (lost cores, harder debugging).

## TuneD — the Linux Tuning Daemon {#tuned}

**Source:** [tuned-project.org](https://tuned-project.org/) · [GitHub](https://github.com/redhat-performance/tuned) · <span class="lvl lvl-b">beginner</span> · actively maintained

The profile-based tuning service used by RHEL, Fedora and derivatives, and the mechanism behind the `latency-performance`, `network-latency`, `realtime` and `cpu-partitioning` profiles. The `cpu-partitioning` profile is now Red Hat's recommended starting point for latency-sensitive workloads because it automates core isolation, `nohz_full`, IRQ affinity and housekeeping-CPU assignment. Reading the profile sources on GitHub is the fastest way to absorb Red Hat's accumulated low-latency defaults instead of hand-copying sysctl snippets.

## RHEL Performance Guide (Marko Myllynen) {#myllynen}

**Source:** [GitHub](https://github.com/myllynen/rhel-performance-guide) · <span class="lvl lvl-i">intermediate</span> · maintained 2021–2025

A concise, opinionated, community-maintained guide to RHEL 9-era performance tuning covering CPU (governors, C-states, isolation, boot parameters), memory (hugepages, NUMA, swap), I/O and network tuning, with monitoring commands for each area. Valuable because it aggregates the scattered official documentation into one actionable page and explicitly warns which tunables *not* to apply blindly. A good bridge between beginner checklists and the full Red Hat documentation set.

## Brendan Gregg — Linux Performance {#gregg}

**Source:** [brendangregg.com](https://www.brendangregg.com/linuxperf.html) · <span class="lvl lvl-b">beginner</span> → <span class="lvl lvl-a">advanced</span> · updated through 2026

The central reference hub for Linux performance observability: tool maps for `perf`, ftrace and eBPF/bpftrace, flame graphs, the USE method, off-CPU analysis, and "Linux Performance Analysis in 60 Seconds". It links Gregg's two standard textbooks — *Systems Performance* (2nd ed., 2020) and *BPF Performance Tools* (2019) — which define modern practice for measuring where latency actually comes from before tuning anything. eBPF-based tracing has replaced guesswork as the standard way trading firms diagnose scheduling jitter and network-stack delays.

## TLB Shootdowns: How to Deter or Disarm Them (JabPerf) {#tlb-shootdowns}

**Source:** [jabperf.com](https://www.jabperf.com/how-to-deter-or-disarm-tlb-shootdowns/) · <span class="lvl lvl-a">advanced</span> · October 2021

Explains a latency source most tuning checklists miss: TLB-shootdown inter-processor interrupts triggered by `munmap`/`madvise`/`mprotect` in multithreaded processes, which can freeze even "isolated" cores. Benchmarks show a workload exploding from 342 ms to 16.4 s as thread count grows, and a trading-infrastructure war story shows kernel upgrades plus automatic NUMA balancing causing mysterious production jitter. Remedies: memory pre-allocation, allocator choice (jemalloc), and Intel's hardware Remote Action Request feature.

## Getting Started with PREEMPT_RT (Realtime Linux project) {#preempt-rt-guide}

**Source:** [realtime-linux.org](https://realtime-linux.org/getting-started-with-preempt_rt-guide/) · <span class="lvl lvl-b">beginner</span> · 2024–2025

The upstream community's onboarding guide for the now-mainline PREEMPT_RT kernel: enabling `CONFIG_PREEMPT_RT`, what changes under RT (threaded IRQs, sleeping locks, priority inheritance), writing and scheduling real-time application threads (`SCHED_FIFO`, priorities, `mlockall`), and measuring latency with `cyclictest` and `rtla`. The natural first read after learning that RT is in mainline 6.12+, before committing to RHEL-RT or a custom kernel build.

## Red Hat Summit 2018 — Performance Analysis and Tuning (slides) {#rh-summit}

**Source:** [PDF](https://www.redhat.com/files/summit/session-assets/2018/Performance-analysis-and-tuning-of-Red-Hat-Enterprise-Linux-Part-1.pdf) · <span class="lvl lvl-i">intermediate</span> · 2018 (historical)

The slide deck from Red Hat's long-running Summit talk series on performance analysis and tuning. The original videos have been taken offline, but the slides survive and still show how Red Hat's performance engineers think: measure first, use tuned profiles as a baseline, then attack NUMA placement, hugepages and IRQ placement. Kept as a historical reference; for current material, use the RHEL 10 (or RHEL 9) documentation above.

## Low Latency Performance Tuning for RHEL 7 (legacy PDF) {#rhel7-legacy}

**Source:** [PDF](https://access.redhat.com/sites/default/files/attachments/201501-perf-brief-low-latency-tuning-rhel7-v2.1.pdf) · <span class="lvl lvl-i">intermediate</span> · 2015–2017 (historical — RHEL 7 is EOL)

The freely accessible PDF version of the now-paywalled knowledge-base article this site originally linked. It covers tuned profiles, C-state and P-state control, NUMA pinning with `numactl`, IRQ affinity, kernel timer-tick options and network tunables as practiced on RHEL 7. Kept as a clearly-labeled historical reference: the techniques are recognizably the ancestors of today's practice, but RHEL 7 reached end of maintenance in June 2024 — build new systems against the RHEL 9/10 guides.
