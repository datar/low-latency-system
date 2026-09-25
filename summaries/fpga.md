---
title: "Summaries — FPGA & Hardware Acceleration"
description: "Plain-language summaries of the FPGA and hardware-acceleration references in the collection."
permalink: /summaries/fpga/
translation: /zh/summaries/fpga/
nav: home
---

# Reference Summaries: FPGA & Hardware Acceleration

[← Back to the reference list](/#fpga--hardware-acceleration) · Each entry below summarizes one reference from the collection, so you can decide whether it is worth your reading time.

---

## AMD Alveo UL3524 — Product Brief {#ul3524}

**Source:** [PDF](https://www.xilinx.com/content/dam/xilinx/publications/product-briefs/2233051_Product_Brief_UL3524_Alveo_Accelerator_Card.pdf) · <span class="lvl lvl-i">intermediate</span> · September 2023

Official product brief for the first FPGA card purpose-built for electronic trading: a custom Virtex UltraScale+ VU2P with a redesigned transceiver path achieving **under 3 ns** wire-to-wire latency — about 7× faster than the previous FPGA generation. Details the form factor, 64 GTF transceivers and reference designs. The concrete spec anchor for any discussion of 2023+ trading FPGAs; the cheaper Alveo UL3422 (Oct 2024) brings the same transceivers to a slimmer card.

## Exegy + AMD: the 13.9 ns STAC-T0 Record {#stac-t0-record}

**Source:** [exegy.com](https://www.exegy.com/exegy-amd-new-record/) · audited report: [STAC AMD240422](https://docs.stacresearch.com/news/AMD240422) · <span class="lvl lvl-i">intermediate</span> · June 2024

The fastest published tick-to-trade result: 13.9 ns minimum actionable latency in the STAC-T0 benchmark, 49% below the previous 24.2 ns record, with jitter cut to ~200 ps. Achieved with an off-the-shelf Alveo UL3524 plus Exegy's nxFramework, using an asynchronous (clockless) implementation of the critical path. Establishes the 2024–2026 state of the art for the wire-to-wire floor. (STAC pages require free registration.)

## Where Next for Ultra-Low Latency Trading? (A-Team Insight) {#ateam-fpga}

**Source:** [a-teaminsight.com](https://a-teaminsight.com/blog/as-the-latest-fpga-technology-from-amd-sets-the-gold-standard-where-next-for-ultra-low-latency-trading/) · <span class="lvl lvl-b">beginner</span> · November 2024

Industry analysis of the latency arms race after the 13.9 ns record. Compares the three implementation tiers — software, FPGA, ASIC — explaining why ASICs are the endgame for top-tier firms but rigid and capital-intensive, while FPGAs occupy the deterministic-but-reprogrammable middle ground. Also covers the budget-friendly UL3422 and the coming role of AI/ML inference on the critical path. Good strategic framing for deciding where to invest engineering effort.

## Orthogone — a Modern FPGA Trading Stack {#orthogone}

**Source:** [orthogone.com](https://orthogone.com/industries/high-frequency-trading-finance/) · <span class="lvl lvl-i">intermediate</span> · 2025

A current vendor view of a full FPGA trading stack: ultra-low-latency IP cores including a 17.1 ns wire-to-wire Ethernet MAC/PCS and a 6.2 ns TX TCP/UDP offload engine, plus kernel-bypass PCIe DMA. Shows how modern hybrid CPU+FPGA systems are partitioned — data path in FPGA, strategy iteration in software — with concrete numbers like 585 ns FPGA-to-host round trip on overclocked Blackcore servers. Representative of the 2025 vendor ecosystem beyond Exegy (which acquired Enyx in 2022 and NovaSparks in January 2026) and AMD.

## FPGA Trading with Nasdaq ITCH & OUCH (Design Gateway) {#dgway-itch-ouch}

**Source:** [dgway.com](https://dgway.com/blog_E/2025/12/18/ultra-low-latency-fpga-trading-with-nasdaq-developed-itch-ouch/) · <span class="lvl lvl-i">intermediate</span> · December 2025

A practical walkthrough of building an all-hardware trading pipeline for Nasdaq protocols: ITCH market data over MoldUDP64 in, order-book building, and OUCH order entry over SoupBinTCP out — entirely in FPGA logic on an AMD Alveo X3522. Shows which protocol layers exist in a real tick-to-trade path and how commercial IP cores compose into a deterministic pipeline. A good bridge from concepts to implementable design.

## Algo-Logic — FPGA Trading Solutions {#algo-logic}

**Source:** [algo-logic.com](https://www.algo-logic.com/) · <span class="lvl lvl-b">beginner</span> · active through 2026

One of the longest-running FPGA-for-finance vendors, still active: tick-to-trade systems, pre-built order-book and protocol-parsing gateware, and FPGA-as-a-service offerings, now marketed alongside AI-driven hardware-accelerated trading. This site has linked Algo-Logic since 2018; John Lockwood's classic HOT Interconnects slides on their low-latency library are preserved on the [Wayback Machine](https://web.archive.org/web/20181221085356/http://www.hoti.org/hoti20/slides/Lockwood_AlgoLogic.pdf) (the original hoti.org link is dead).
