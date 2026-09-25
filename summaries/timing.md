---
title: "Summaries — Time Synchronization & Timestamping"
description: "Plain-language summaries of the time-sync and timestamping references in the collection."
permalink: /summaries/timing/
translation: /zh/summaries/timing/
nav: home
---

# Reference Summaries: Time Synchronization & Timestamping

[← Back to the reference list](/#time-synchronization--timestamping) · Each entry below summarizes one reference from the collection, so you can decide whether it is worth your reading time.

---

## How PTP Is Deployed at Meta {#meta-ptp}

**Source:** [engineering.fb.com](https://engineering.fb.com/2022/11/21/production-engineering/precision-time-protocol-at-meta/) · <span class="lvl lvl-i">intermediate</span> · November 2022 (follow-ups: [SPTP, Feb 2024](https://engineering.fb.com/2024/02/07/production-engineering/simple-precision-time-protocol-sptp-meta/); leap seconds, Feb 2025)

Meta's canonical write-up of replacing NTP with PTP at hyperscale, achieving tens-of-nanosecond synchronization (~436 ns worst-case Window of Uncertainty) — roughly 100× better than their tuned NTP. Covers the full architecture: Time Card-based open time servers, hardware-timestamping NICs, and the open-source stack (`ptp4u` serving 1M+ clients, `fbclock`, `ptpcheck`, `oscillatord`, and the newer SPTP client). The best public case study of production PTP outside finance, directly applicable to trading infrastructure.

## OCP Time Appliances Project — Time Card {#time-card}

**Source:** [GitHub](https://github.com/Time-Appliances-Project/Time-Card) · <span class="lvl lvl-a">advanced</span> · 2021–2024 (release Dec 2024)

The open-source hardware design that turns any x86 server with a hardware-timestamping NIC into a stratum-1 time appliance: a PCIe card combining a GNSS receiver and a miniature atomic clock for holdover, exposed to Linux as standard `/dev/ptpX` and `/dev/ppsY` devices consumable by chrony and ptp4l. The design is being standardized as IEEE P3335, and commercial off-the-shelf versions exist. What used to require proprietary grandmaster appliances is now open hardware.

## White Rabbit — Sub-nanosecond Time from CERN {#white-rabbit}

**Source:** [white-rabbit.web.cern.ch](https://white-rabbit.web.cern.ch/) · <span class="lvl lvl-i">intermediate</span> · ongoing; Collaboration launched 2024

The sub-nanosecond timing technology that extends PTP with Synchronous Ethernet and phase measurement, achieving sub-ns accuracy over km-scale fiber. White Rabbit was folded into IEEE 1588-2019 as the High Accuracy profile; hardware, firmware and software are fully open source. CERN launched a formal White Rabbit Collaboration in 2024 with four finance-sector members — the reference point for "beyond PTP" timing in trading.

## Deutsche Börse / Eurex High Precision Time (White Rabbit) {#eurex-wr}

**Source:** [eurex.com](https://www.eurex.com/ex-en/support/initiatives/archive/high-precision-time-white-rabbit-pilot) · current service page: [Deutsche Börse Time Services](https://www.deutsche-boerse.com/dbg-en/markets-services/ps-technology/ps-7-market-technology/ps-n7/ps-connectivity-services-time-services) · <span class="lvl lvl-b">beginner</span> · pilot 2018–2019, permanent service since April 2019

How an actual exchange distributes time to trading participants: co-location customers synchronize to the exact clock Deutsche Börse uses to generate its own high-precision timestamps, delivered over dedicated fiber cross-connects with sub-ns accuracy. Explains why exchange-anchored timing matters for latency measurement and fairness analysis. Deutsche Börse remains a White Rabbit Collaboration member.

## linuxptp Documentation (ptp4l, phc2sys, ts2phc) {#linuxptp}

**Source:** [linuxptp.nwtime.org](https://linuxptp.nwtime.org/documentation/) · <span class="lvl lvl-i">intermediate</span> · continuously maintained

Reference documentation for the standard Linux PTP stack: `ptp4l` (ordinary/boundary/transparent clock), `phc2sys` (sync the system clock to the NIC's PTP hardware clock), `ts2phc`, `pmc` and `timemaster` (combine PTP and NTP sources). Includes worked example configurations for telecom profiles, unicast, gPTP and authentication. This is the software that any MiFID II-compliant Linux timestamping deployment is built on.

## MiFID II RTS 25 and Time Sync on Linux (Red Hat) {#mifid-rts25}

**Source:** [redhat.com](https://www.redhat.com/en/blog/mifid-ii-rts-25-and-time-synchronisation-red-hat-enterprise-linux-and-red-hat-virtualization) · <span class="lvl lvl-b">beginner</span> · 2017, fundamentals still accurate

Maps the EU regulatory requirement — ≤100 µs divergence from UTC with 1 µs timestamp granularity for HFT firms under RTS 25 — to concrete Linux capability numbers: ~30 µs for software-timestamped NTP/PTP, ~10 µs for hardware-timestamped NTP, ~3 µs or better for hardware-timestamped PTP. Explains why hardware-timestamping NICs are effectively mandatory for compliance. **Update (2026):** in the EU, RTS 25 (Delegated Regulation 2017/574) was repealed and replaced from 2 March 2026 by [Commission Delegated Regulation (EU) 2025/1155](https://eur-lex.europa.eu/eli/reg_del/2025/1155/oj/eng), now under MiFIR Art. 22c. The 100 µs maximum divergence for HFT and for the fastest venues is unchanged, the tightest timestamp granularity moved from 1 µs to 0.1 µs, and scope widened to systematic internalisers, APAs and consolidated-tape providers. The UK keeps its onshored RTS 25. The article's Linux technical guidance still applies.

## Real-world PTP Accuracy Across NICs (scottstuff.net) {#nic-ptp-accuracy}

**Source:** [scottstuff.net](https://scottstuff.net/posts/2025/05/07/connectx-4-and-linuxptp/) · <span class="lvl lvl-a">advanced</span> · May 2025

A meticulous field measurement of real PTP accuracy across NIC generations (ConnectX-3 through -6, Intel x710/E810), showing hardware matters enormously: Intel E810-class parts hold ~10 ns to the PHC while a ConnectX-4 running ptp4l inflated errors 45× and spiked to tens of microseconds. Also documents a subtle production pitfall — hardware-timestamped chrony and ptp4l sharing one PHC couple their errors — plus workarounds. Exactly the measured, current data missing from vendor datasheets.

## Intel rdtsc Benchmarking Whitepaper (Paoloni) {#rdtsc-paper}

**Source:** [mirror PDF](https://cis.temple.edu/~qzeng/cis3207-spring18/files/ia-32-ia-64-benchmark-code-execution-paper.pdf) · [archived Intel original](https://web.archive.org/web/20230817174418/https://www.intel.com/content/dam/www/public/us/en/documents/white-papers/ia-32-ia-64-benchmark-code-execution-paper.pdf) · <span class="lvl lvl-a">advanced</span> · 2010 (classic)

Gabriele Paoloni's classic Intel whitepaper on cycle-accurate code timing with `rdtsc`/`rdtscp`: serializing instructions, fencing, and the statistical methodology for repeatable measurements. Intel removed the original page (the link this site carried is dead), but the paper remains the foundation cited by nearly every benchmarking framework. Modern practice adds invariant TSC, isolated cores, and the jitter tools in the measurement section.
