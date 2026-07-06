---
title: "Research Notes — the July 2026 Update"
description: "How this site's 2026 content refresh was researched: process, key findings, and the full link audit."
permalink: /research/
translation: /zh/
nav: research
---

# Research Notes: the July 2026 Update

This site was originally curated around 2016–2018. In July 2026 the entire collection was re-researched and rebuilt. This page documents **how** the research was done and **what** it found, so readers can judge the update's reliability. The file-by-file changelog with rationale is in [CHANGES.md](https://github.com/datar/low-latency-system/blob/master/CHANGES.md) in the repository.

## Process {#process}

1. **Inventory.** Every reference on the old one-page site (~85 links across 20 sections) was extracted and grouped into five research areas: server hardware & CPUs; networking/NICs/kernel bypass; Linux OS tuning; FPGA, time sync & measurement tools; software engineering & languages.
2. **Link audit.** Each legacy URL was fetched and classified *alive / moved (redirect) / dead / hijacked*. For every dead or moved link we searched for the canonical current location, an official successor document, or — for classics with no successor — a Wayback Machine capture.
3. **State-of-the-art survey.** For each area, web research (vendor documentation, conference talks, benchmark disclosures, practitioner blogs, kernel changelogs) established what changed between ~2018 and mid-2026, with dates and numbers.
4. **Reference selection.** New references were admitted only if (a) the URL was verified reachable, (b) the content is authoritative or uniquely instructive, and (c) it earns its place for a stated audience level (beginner/intermediate/advanced). Every admitted reference got a 3–5 sentence summary on a [summaries page](/summaries/networking/), linked beside its source link.
5. **Reorganization.** The flat link list became: a curated reference list by topic ([home](/)), per-topic summary pages, and a single [beginner-to-expert guide](/guide/) that gives the whole field a narrative structure.

*Caveat: a few vendor pages (docs.redhat.com, intel.com, cisco.com, hpe.com, STAC) block automated fetchers and were verified via search indexing and mirrors; they load normally in browsers.*

## Key findings by area {#findings}

### The vendor map was redrawn by acquisitions

- **Solarflare → Xilinx (2019) → AMD (2022).** The dominant trading-NIC franchise now ships as AMD Solarflare; the current flagship is the **X4 series** (Oct 2025). OpenOnload was open-sourced in 2021 at `github.com/Xilinx-CNS/onload`; openonload.org and support.solarflare.com are gone.
- **Mellanox → NVIDIA (2020).** All mellanox.com documentation links died; docs moved to NVIDIA portals. The VMA kernel-bypass library was superseded by **XLIO**; Rivermax/NEIO FastSockets is the 2025 finance offering.
- **Exablaze → Cisco (2019), then exit.** ExaNIC became Cisco Nexus SmartNIC (maintenance mode); Cisco end-of-saled the entire ex-Exablaze ULL switch line in April 2023, leaving **Arista 7130** (ex-Metamako) as the de facto layer-1/ULL switching platform.
- **HPE retired its overclocked "Trade and Match" servers**; boutique vendors (Blackcore, Hypertec CIARA) now own the overclocked-server niche.
- Smaller moves: Real Logic (Aeron) acquired by Adaptive (2022), repo moved to `aeron-io`; Enyx acquired by Exegy (2022); Corvil acquired by Pico (2019); STAC acquired by ISG (2020).

### Technology state of the art (mid-2026)

- **Tick-to-trade tiers:** tuned software with kernel bypass ~1–2 µs; FPGA tens–hundreds of ns, with the **STAC-T0 record at 13.9 ns** (Exegy + AMD Alveo UL3524, 2024); ASICs at the deterministic frontier.
- **PREEMPT_RT merged into mainline Linux 6.12 (Nov 2024)** after ~20 years out-of-tree; `kernel-rt` is included in standard RHEL since 9.0. Modern isolation practice is `nohz_full` + `rcu_nocbs` + IRQ steering (bare `isolcpus` is insufficient); tuned's `cpu-partitioning` profile automates it; `rtla` (kernel 5.17+) is the new jitter-diagnosis tool; observability is eBPF-first.
- **CPUs:** AMD EPYC 9005 F-series (9575F) and X3D parts with 3D V-Cache; Intel Xeon 6 with Latency Optimized Mode (+17% measured) and SST-BF guaranteed-fast cores; single-socket settled as the latency architecture; core-to-core latency is now a first-class selection metric.
- **Time sync:** IEEE 1588-2019 (PTP v2.1) is the baseline; White Rabbit became its High Accuracy profile and is offered by Deutsche Börse in co-location; Meta open-sourced its PTP stack and the OCP Time Card; MiFID II RTS 25 (100 µs / 1 µs granularity) still drives requirements, with its successor RTS in draft.
- **Languages:** C++ canon refreshed (Optiver's David Gross: Meeting C++ 2022, CppCon 2024 keynote); Rust reached effective latency parity with a maturing ecosystem (Databento, 2025); Java consolidated on Disruptor/Chronicle/Aeron with pauseless GC now free via generational ZGC (JDK 21+).

### Link audit results

Of the ~85 legacy references: roughly **one third were dead or effectively dead** (404, retired portals, empty JS shells), several more silently redirected to marketing homepages, and one — Dmitry Vyukov's `1024cores.net` — was **domain-hijacked and now serves spam** (content survives at the official Google Sites mirror, which we now link). Notable dead links and their replacements:

| Legacy link | Fate | Replacement |
|---|---|---|
| RHEL 7 Performance Tuning Guide | RHEL 7 EOL 2024; URL redirects | RHEL 9 performance & real-time guides |
| Red Hat low-latency & network tuning articles | paywalled / flagged obsolete | RHEL 9 docs + public RHEL 7 PDF (historical) |
| Mellanox tuning PDF, VMA guide, RDMA manual | mellanox.com retired | NVIDIA support article, XLIO docs, networking-docs.nvidia.com |
| openonload.org (talk PDF, sysjitter) | domain broken | github.com/Xilinx-CNS/onload, cns-sysjitter; talk on Wayback |
| support.solarflare.com portal | redirects to generic AMD portal | AMD UG1586 Onload User Guide |
| Dell 2010 low-latency whitepaper | dead | Dell PowerEdge 16G BIOS tuning guide |
| HP hpsc document viewer links | empty JS shell | hpe.com/psnow/doc/c01804533 |
| Intel software.intel.com latency articles | 403 | relocated intel.com developer-articles URLs |
| AMD 2018 tuning PDF (developer.amd.com) | 404 | EPYC 9005 tuning guide (docs.amd.com, 2026) |
| exablaze.com blog | domain dead | Cisco SmartNIC datasheet; Wayback |
| Intel rdtsc benchmarking paper | removed by Intel | university mirror + Wayback |
| Lockwood Algo-Logic HOTI slides | hoti.org 404 | Wayback capture |
| cubrid.org TCP/IP stack article | 404 | same article at cubrid.org/blog/3826497 |
| Herb Sutter Lock-Free Part II | video ID truncated on old page | corrected YouTube link |
| 1024cores.net | **hijacked (spam)** | sites.google.com/site/1024cores |
| Mechanical Sympathy group, Cloudflare posts, calomel, Linux Journal zero-copy, algo-logic.com, hiccups | alive | kept (some marked historical/unmaintained) |

### What was removed

Some legacy entries were removed rather than replaced, with reasons recorded in [CHANGES.md](https://github.com/datar/low-latency-system/blob/master/CHANGES.md): content-farm or Q&A links that no longer add value next to better sources (Quora architecture threads, Tom's Hardware jitter FAQ, quantlabs posts), 2012-era FPGA vendor pages that no longer exist or describe long-obsolete products (NetFPGA-10G, Nallatech, Xeon+FPGA prototypes), superseded tuning posts (2013 multi-queue SMP article, OpenBSD-centric calomel), and marketing pages with no durable technical content (Fujitsu optical brochure, IBM/Solarflare 2011 whitepaper, "faster than 10G" blogspot). The scrubbed Meanderful blog (Matt Hurd) is remembered via the Wayback Machine in CHANGES.md only.

## Methodology notes {#methodology}

Research was performed with parallel web-research agents (one per area), each instructed to verify every recommended URL by fetching it and to return link status, dated state-of-the-art facts, and candidate references with summaries and difficulty levels. Their raw findings were then cross-checked where areas overlapped (e.g. Rigtorp's guide, STAC records and core-to-core-latency tools surfaced independently in multiple areas — a good sign of canonicity) and edited into the pages you see. Facts carry dates in the text so future readers can judge staleness; this page itself should be re-verified around **mid-2027**.
