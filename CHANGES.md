# Changes — July 2026 Site Rebuild

This file records every change made in the July 2026 update and why. The research process and findings behind these changes are on the site at [/research/](https://lowlatencysystem.com/research/) (`research.md`).

## 1. Why this update

The site's content dated from ~2016–2018. A full link audit found roughly a third of the ~85 references dead (retired vendor portals, 404s, empty JS shells), one domain hijacked and serving spam (1024cores.net), and the industry landscape substantially changed (Solarflare→AMD, Mellanox→NVIDIA, Exablaze→Cisco→exit, PREEMPT_RT mainlined, FPGA records, Rust adoption). The site also had no summaries, no learning path, and no Chinese version.

## 2. Structural changes

| Change | Why |
|---|---|
| **New layout** (`_layouts/default.html`): responsive typography, light/dark theme (auto + toggle), sticky nav, footer | Old layout was a bare 650px column with a 2013-era CSS snippet; "beautify web UI" was a goal of this update |
| **Language switcher + full Chinese mirror under `/zh/`** | Requested feature; nav labels localize per page `lang`; every page's front matter carries a `translation:` URL |
| **Per-topic summary pages under `/summaries/`** (7 pages: linux-tuning, hardware, networking, fpga, timing, tools, software) | Every reference now has a `summary` badge beside the source link, so readers can decide before clicking; anchors are stable per reference |
| **New `/guide/` page** — one-page overview organizing the whole topic from beginner to expert | Requested feature; the flat link list had no narrative or ordering for newcomers |
| **New `/research/` page** | Documents the research process/results behind this update, per the update's ground rules |
| **`index.md` rewritten** — sections reorganized (Start Here → OS → hardware → network → FPGA → time → measurement → code), level badges (beginner/intermediate/advanced) on every entry | Old page mixed topics ("Other" three times), had orphan stubs (bare "Sockperf", "IB verbs", empty "Kernel bypass" heading), and no difficulty signals |
| **404 page rewritten** (`404.md`) | Old page said only "Whatever!"; now links home/guide/zh and the issue tracker |
| **`_config.yml`**: site title/description/url added; layout default applies to all pages | Old config only styled index.md; new pages need the layout too |
| **Google Analytics snippet updated UA → GA4 (gtag.js), ID commented out** | The old `UA-75987842-3` property stopped processing data when Google sunset Universal Analytics (July 2023). The owner must create a GA4 property and set `google_analytics: G-XXXXXXXXXX` to re-enable analytics |
| **AdSense include modernized** | Old page-level-ads API is deprecated; replaced with the current async auto-ads script (same publisher ID) |
| **README.md updated** | Points to the new pages and this changelog |

## 3. Content changes by section

### Linux OS tuning
- **Replaced** RHEL 7 Performance Tuning Guide (EOL June 2024) → RHEL 9 *Monitoring and managing system status and performance*.
- **Replaced** paywalled/obsolete Red Hat KB articles (1323793, 1391433) → RHEL 9 *Optimizing RHEL for Real Time for low latency operation*; kept the public RHEL 7 PDF as a labeled historical item.
- **Replaced** dead Red Hat Summit 2018 videos → surviving slide PDF, labeled historical.
- **Added** (all new since the last update): Rigtorp low-latency guide, LWN on PREEMPT_RT mainlining (6.12, Nov 2024), SUSE CPU-isolation series, isolcpus-for-HFT case study, TuneD project, Myllynen RHEL performance guide, Brendan Gregg hub, JabPerf TLB-shootdown article, realtime-linux.org PREEMPT_RT starter.
- **Removed**: haifux 2008 kernel-networking lecture (superseded by packagecloud/ntk148v), greenhost 2013 multi-queue post (superseded), calomel.org (BSD-centric, marginal for this site), linux-sunxi & openwrt wiki pages and linuxdevcenter 2000 article (obsolete), wiki.mikejung.biz (gone/low value).

### Server hardware & CPUs (was "System tuning guides from server vendors" + "Intel and AMD")
- **Rewrote the section intro**: the old text recommended Core i9-7980XE-era overclocking; now covers Blackcore/Hypertec boutique servers, EPYC 9005 F-series, EPYC 4004/4005 X3D, Xeon 6 latency features, single-socket consensus, core-to-core latency as a selection metric. HPE "Trade and Match" noted as discontinued.
- **Replaced** dead HP hpsc links → hpe.com/psnow/doc/c01804533 (updated through Gen11); dropped the Gen8 conference PDF and Superdome doc (dead, obsolete products).
- **Replaced** dead Dell 2010 whitepaper → Dell PowerEdge 16G BIOS tuning guide (low-latency chapter).
- **Replaced** dead AMD 2018 tuning PDF → AMD EPYC 9005 BIOS & Workload Tuning Guide (June 2026).
- **Moved** relocated Intel latency articles to their new intel.com URLs, labeled historical.
- **Kept** Lenovo/IBM redp4285 at its new lenovopress.lenovo.com URL but labeled clearly as 2008-era historical.
- **Added**: Intel Xeon 6 performance/power profiles PDF, Phoronix Latency Optimized Mode benchmarks, ServeTheHome SST-BF explainer, Chips and Cheese Xeon 6 memory analysis, nviennot/core-to-core-latency, Blackcore knowledge article, twanas/hft-server-settings checklist.
- **Removed**: Supermicro/ExaNIC blog (exablaze.com dead), Intel GatiRT article (dead), Tom's Hardware jitter FAQ (consumer content).

### Networking (was "Network Tuning Guides", "Faster than 10G", "Low Level API", "Kernel bypass", "Infiniband, RMDA", "Zero Copy")
- **Consolidated six scattered sections into one** with subsections: kernel path → bypass stacks → NICs → switches → RDMA. The old "Kernel bypass" heading had no content; "Low Level API" was three bare names.
- **Replaced** dead Solarflare/Mellanox links → AMD UG1586 Onload guide, Xilinx-CNS/onload GitHub, NVIDIA adapter-tuning article, XLIO docs (VMA's successor), networking-docs RDMA manual.
- **Added**: AMD Solarflare X4 launch (2025), Rivermax/NEIO FastSockets (2025), Arista 7130 hub + 4 ns measurement brief, Cisco SmartNIC datasheet (with maintenance-mode caveat), Cloudflare kernel-bypass intro, talawah.io extreme tuning, ntk148v consolidated guide, CUBRID article at its new URL, Databento tick-to-trade.
- **Kept**: both Cloudflare classics (verified alive), Linux Journal Zero Copy (2003 classic).
- **Removed**: five IB introduction PDFs/slide decks (mostly dead; era 2008–2013; replaced by the maintained RDMA manual + a note that InfiniBand left the exchange-facing path), thegeekinthecorner blog (dormant, unverified), PF_RING entry (niche today; covered inside the kernel-bypass references), 10gbe.blogspot, cnblogs repost, "Additional Resources" grab-bag (Quora threads, 2000-era articles, brochures — no durable value).

### FPGA
- **Rewrote entirely**: the old section was 2012–2013 vintage (NetFPGA-10G, Nallatech, quantlabs). New anchor facts: AMD Alveo UL3524 (2023) purpose-built trading FPGA, Exegy+AMD 13.9 ns STAC-T0 record (2024), hybrid CPU+FPGA as mainstream architecture, ASIC frontier.
- **Kept**: Algo-Logic (still active); Lockwood slides preserved via Wayback (hoti.org dead).
- **Added**: A-Team strategic overview, Orthogone stack, Design Gateway ITCH/OUCH pipeline walkthrough.
- **Removed**: NetFPGA-10G page (dead/obsolete), Nallatech (company renamed/absorbed), quantlabs blog posts (content farm), meanderful FPGA-NIC list (blog scrubbed — archived at web.archive.org/web/2019/https://meanderful.blogspot.com/), embeddedintel commentary and CMU Xeon+FPGA slides (dead; the hybrid concept is now covered by current references), fpgadeveloper.com (generic, off-topic), StackExchange QA (2014-era answers).

### Time & measurement (was "Time tech" + "Jitter detection" + "Benchmarks and tools")
- **Split into two sections**: *Time synchronization & timestamping* and *Measurement, benchmarks & jitter detection* — timing is now a first-class topic (MiFID II, PTP, White Rabbit).
- **Replaced**: dead ELC 2015 PTP slides → linuxptp docs, Meta PTP deployment, OCP Time Card, White Rabbit, Eurex WR service, Red Hat RTS 25 article, scottstuff NIC PTP measurements. Dead Intel rdtsc paper → university mirror + Wayback. Dead openonload.org sysjitter → Xilinx-CNS/cns-sysjitter. Old stacresearch.com/SFC170206 → docs.stacresearch.com (+ the 2024 AMD240422 record).
- **Kept**: hiccups (marked unmaintained), sockperf (now with its GitHub URL instead of a bare name).
- **Added**: cyclictest/rt-tests + rtla, Corvil/Pico, c2clat, Google Benchmark.
- **Removed**: "HP-timetest" mailto entry (was a request for help, not a resource); iperf bare name (folded into sockperf summary as companion tools).

### Programming languages & code
- **Restructured** into C++ / concurrency & lock-free / Java / Rust / communities & protocols, with a Start Here section up top.
- **Fixed** the broken Herb Sutter Part II link (video ID was truncated: `CmxkPChOcv` → `CmxkPChOcvw`).
- **Re-pointed** 1024cores to the official Google Sites mirror — **the .net domain is hijacked and serves spam; do not link or visit it.**
- **Updated** epickrram blog to .com URL (old .co.uk redirects); Azul entry now points at the C4 docs and notes the Platform Prime rebranding and generational ZGC as the free alternative; Aeron points at the new aeron-io org (Adaptive acquired Real Logic in 2022).
- **Added**: David Gross CppCon 2024 keynote + Meeting C++ 2022 talk, Carl Cook 2017 classic (was missing!), Doumler C++Now 2023 two-parter, Frasch SPSC talk, Imperial College arXiv paper, Google Benchmark, Godbolt GOTO 2024 CPU talk, rigtorp essays/libraries, Disruptor + Chronicle Queue, Thompson exchange-architectures talk, Databento Rust-vs-C++ and microstructure guides, hftbacktest, Mechanical Sympathy group (verified still active).
- **Kept as historical**: SubMicroTrading blog (now with the open-sourced code repo), epickrram jitter posts.

## 4. Editorial conventions introduced

- Every reference: `[Title](source)` + `[summary]` badge → anchored entry on a summaries page + level tag (beginner/intermediate/advanced) + one-line description.
- Facts carry dates in the text ("Oct 2025", "kernel 6.12, Nov 2024") so future staleness is visible.
- Dead-but-classic resources are labeled *Historical* rather than silently kept or dropped.
- Next full re-verification recommended around mid-2027.
