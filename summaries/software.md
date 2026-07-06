---
title: "Summaries — Software Engineering & Languages"
description: "Plain-language summaries of the programming-language, lock-free and community references in the collection."
permalink: /summaries/software/
translation: /zh/summaries/software/
nav: home
---

# Reference Summaries: Software Engineering & Languages

[← Back to the reference list](/#programming-languages--code) · Each entry below summarizes one reference from the collection, so you can decide whether it is worth your reading time.

---

## When Nanoseconds Matter — David Gross (CppCon 2024 keynote) {#gross-2024}

**Source:** [YouTube](https://www.youtube.com/watch?v=sX2nF1fW7kI) · <span class="lvl lvl-a">advanced</span> · 2024

The current definitive conference talk on HFT C++, delivered as a CppCon 2024 keynote by Optiver's options tech lead. Gross walks through building an ultrafast trading system end to end: order-book data structures (why node-based containers lose, why sorted vectors with `lower_bound` win), concurrent queue design, and measuring what actually matters with hardware counters. It updates the folklore from the 2017-era talks with nanosecond-scale techniques used in production. Essential viewing as the successor to Carl Cook's classic.

## Trading at Light Speed — David Gross (Meeting C++ 2022) {#gross-2022}

**Source:** [YouTube](https://www.youtube.com/watch?v=8uAW5FQtcvE) · <span class="lvl lvl-a">advanced</span> · 2022

Gross's earlier full-system talk on designing a low-latency trading system from scratch: translating CPU/hardware knowledge into C++, data modeling for performance, exploiting multiple cores while managing concurrency, C-state/P-state tuning, and shared last-level-cache effects. Complements the 2024 keynote with more emphasis on system tuning and measurement methodology — a good bridge between architecture-level and code-level optimization.

## When a Microsecond Is an Eternity — Carl Cook (CppCon 2017) {#carl-cook}

**Source:** [YouTube](https://www.youtube.com/watch?v=NH1Tta7purM) · <span class="lvl lvl-i">intermediate</span> · 2017

The classic and still one of CppCon's most-watched talks, by an Optiver engineer. Cook demonstrates concrete coding techniques for shaving time off the hot path — branch elimination, cache warming, avoiding system calls and allocations — targeting ~2.5 µs wire-to-wire latency. Some specifics have aged, but the mindset and most techniques remain the canonical introduction to HFT C++. Pair with David Gross's 2024 keynote for the current state of the art.

## What is Low Latency C++? — Timur Doumler (C++Now 2023, 2 parts) {#doumler}

**Source:** [Part 1](https://www.youtube.com/watch?v=EzmNeAhWqVs) · [Part 2](https://www.youtube.com/watch?v=5uIsadq-nyk) · <span class="lvl lvl-i">intermediate</span> · 2023

A two-part survey of what "low latency" means across trading, audio and games, and why C++ dominates these domains. Part 1 covers efficiency fundamentals — avoiding allocations, exceptions and system calls on the hot path; Part 2 goes deep on hardware effects: caches, branch prediction, low-latency multithreading and wait-free techniques. The best structured, modern (C++20-era) pedagogical treatment of the whole topic, from an ISO C++ committee member. The ideal first serious deep dive before the firm-specific HFT talks.

## SPSC Lock-free FIFO From the Ground Up — Charles Frasch (CppCon 2023) {#frasch-spsc}

**Source:** [YouTube](https://www.youtube.com/watch?v=K3P_Lmq6pw0) · <span class="lvl lvl-i">intermediate</span> · 2023

A step-by-step construction of the workhorse data structure of trading systems: the single-producer single-consumer lock-free queue. Frasch starts from a naive mutex version and iteratively applies memory ordering, cache-line alignment against false sharing, and index caching — benchmarking each step to show exactly where the nanoseconds go. Effectively a video companion to rigtorp's `SPSCQueue`. One of the best hands-on introductions to reasoning about C++ atomics with real performance data.

## C++ Design Patterns for Low-latency Applications (arXiv) {#hft-paper}

**Source:** [arxiv.org](https://arxiv.org/abs/2309.04259) · <span class="lvl lvl-a">advanced</span> · 2023

An Imperial College paper (Bilokon & Gunduz) that benchmarks the low-latency C++ folklore — cache warming, `constexpr`, branch reduction, loop unrolling, prefetching, a C++ Disruptor — with statistical rigor. Cache warming and `constexpr` delivered the largest measured latency wins. Ships a companion repository ([github.com/0burak/imperial_hft](https://github.com/0burak/imperial_hft)) with all benchmark code. Useful because it turns conference-talk advice into measured, reproducible numbers — one of the few citable academic treatments of HFT software engineering.

## Google Benchmark {#google-benchmark}

**Source:** [GitHub](https://github.com/google/benchmark) · <span class="lvl lvl-b">beginner</span> · actively maintained (v1.9.5, Jan 2026)

The de-facto standard C++ microbenchmarking library, used throughout the low-latency community to validate optimization claims. Provides statistically robust timing, automatic iteration-count selection, `DoNotOptimize`/`ClobberMemory` barriers to defeat the optimizer, and perf-counter integration. Any latency claim you read in a talk or blog post should be reproducible with this plus Linux `perf`.

## What Every Programmer Should Know about How CPUs Work — Matt Godbolt (GOTO 2024) {#godbolt-cpu}

**Source:** [YouTube](https://www.youtube.com/watch?v=-HNpim5x-IE) · <span class="lvl lvl-b">beginner</span> · 2024

Godbolt — Compiler Explorer creator and former HFT engineer — explains modern CPU pipelines, branch prediction, out-of-order execution and caches, and how compilers exploit them, in 43 minutes. The best current "mechanical sympathy" primer: everything the classic Martin Thompson blog taught, updated for 2024 hardware and illustrated with live tooling. The ideal on-ramp before the HFT-specific talks.

## C++ Atomics, From Basic to Advanced — Fedor Pikus (CppCon 2017) {#pikus-atomics}

**Source:** [YouTube](https://www.youtube.com/watch?v=ZQFzMfHIxng) · <span class="lvl lvl-a">advanced</span> · 2017 (still canonical)

Pikus demystifies `std::atomic`: what atomicity and memory ordering actually guarantee, what the hardware does underneath, and the real cost of sequential consistency versus acquire/release. Full of counterintuitive benchmark results — when a mutex beats an atomic, and why "lock-free" does not mean "faster". Still the reference talk for correctly using atomics in queue and seqlock implementations; Pikus continues teaching this material in his CppCon classes and his book *The Art of Writing Efficient Programs*.

## Lock-Free Programming, Parts I & II — Herb Sutter (CppCon 2014) {#sutter-lockfree}

**Source:** [Part I](https://www.youtube.com/watch?v=c1gO9aB9nbs) · [Part II](https://www.youtube.com/watch?v=CmxkPChOcvw) · <span class="lvl lvl-a">advanced</span> · 2014 (still canonical)

Sutter's two-part masterclass on lock-free fundamentals: exchange-based patterns, the ABA problem, and building a lock-free singly linked list correctly. Part I establishes the mental model and when *not* to go lock-free; Part II works through the hard cases and hazard-pointer-style memory reclamation. A decade old, but nothing has replaced it as the conceptual foundation — modern talks assume you have absorbed it. (The Part II link circulating on old pages, including the previous version of this site, had a truncated video ID; the link above is correct.)

## Erik Rigtorp — Low-latency C++ Essays and Libraries {#rigtorp}

**Source:** [rigtorp.se](https://rigtorp.se/) · <span class="lvl lvl-a">advanced</span> · 2020–2021, still canonical

The most-cited modern collection of low-latency engineering write-ups: "Low latency tuning guide", "Correctly implementing a spinlock in C++", "Latency implications of virtual memory", "Optimizing a ring buffer for throughput". The companion MIT-licensed libraries — `SPSCQueue`, `MPMCQueue`, `Seqlock` on [github.com/rigtorp](https://github.com/rigtorp) — are production-quality reference implementations with careful false-sharing padding and index caching. Also home of the curated [awesome-lockfree](https://github.com/rigtorp/awesome-lockfree) list.

## 1024cores — Dmitry Vyukov on Lock-free Algorithms {#vyukov-1024cores}

**Source:** [sites.google.com/site/1024cores](https://sites.google.com/site/1024cores/) · <span class="lvl lvl-a">advanced</span> · ~2010–2013 (relocated)

Vyukov's classic reference on lock-free/wait-free algorithms, memory models, producer-consumer queue taxonomies, and his famous MPSC queue design used inside Go's scheduler and countless trading systems. **Warning:** the old custom domain `1024cores.net` is dead and hijacked (it now serves spam) — the Google Sites URL above is the surviving official location. The content is 2010-era but the algorithms and the "first things first" methodology remain foundational.

## Aeron — Low-latency Messaging and Clustering {#aeron}

**Source:** [GitHub (aeron-io)](https://github.com/aeron-io/aeron) · [docs](https://aeron.io/docs/) · <span class="lvl lvl-a">advanced</span> · actively developed through 2026

Martin Thompson's high-throughput, low-latency message transport (UDP unicast/multicast and shared-memory IPC), plus Aeron Archive and Aeron Cluster for fault-tolerant, exchange-style replicated state machines. Real Logic was acquired by Adaptive Financial Consulting in February 2022 and the repository moved from `real-logic` to the `aeron-io` GitHub organization. It is the reference open-source architecture for building a matching engine in Java, with C/C++ and .NET clients too.

## Evolution of Financial Exchange Architectures — Martin Thompson (QCon 2020) {#thompson-exchange}

**Source:** [YouTube](https://www.youtube.com/watch?v=qDhTjE0XmkE) · <span class="lvl lvl-i">intermediate</span> · 2020

The Disruptor/Aeron creator surveys 25 years of exchange architecture: from mainframes through LMAX-style in-memory event sourcing to replicated state machines with Raft consensus (Aeron Cluster). Explains why deterministic single-threaded business logic plus sequenced logs beats distributed locking for both latency and correctness. The best single talk connecting software design patterns to exchange-grade requirements like fault tolerance and replay.

## LMAX Disruptor & Chronicle Queue — the GC-free Java Toolkit {#disruptor-chronicle}

**Source:** [Disruptor](https://github.com/LMAX-Exchange/disruptor) · [Chronicle Queue](https://github.com/OpenHFT/Chronicle-Queue) · <span class="lvl lvl-i">intermediate</span> · maintained through 2026

The Disruptor is the ring-buffer inter-thread messaging pattern that started the mechanical-sympathy movement; still maintained (v4.x), and its [user guide](https://lmax-exchange.github.io/disruptor/) remains the definitive read. Chronicle Queue (Peter Lawrey's OpenHFT) provides microsecond persisted messaging via memory-mapped files — broker-less, everything on disk — and is used across major investment banks; the Chronicle 25 series supports Java 21. Together they define GC-free Java design: preallocated, flyweight, off-heap.

## Azul Platform Prime (Zing) and the C4 Pauseless Collector {#azul-zing}

**Source:** [docs.azul.com](https://docs.azul.com/prime/c4-garbage-collection.html) · <span class="lvl lvl-i">intermediate</span> · current docs

Azul's Zing JVM — now packaged as "Azul Zing Builds of OpenJDK" within Azul Platform Prime — remains the commercial option for pauseless Java, via the C4 (Continuously Concurrent Compacting Collector) and the Falcon JIT. This page documents how C4 achieves generational, fully concurrent compaction without stop-the-world pauses. Weigh it against OpenJDK's free **generational ZGC** (default since JDK 21, matured through JDK 25), which has closed much of the gap for latency-sensitive workloads.

## Rust vs C++ for Trading Systems — Databento {#rust-vs-cpp}

**Source:** [databento.com](https://databento.com/blog/rust-vs-cpp) · <span class="lvl lvl-i">intermediate</span> · August 2025

The most grounded public comparison of the two languages for trading, written by a market-data vendor that runs both in production. C++ still wins on ecosystem maturity, exchange-connectivity libraries and ultra-tuned hot paths; Rust delivers near-identical latency with compile-time elimination of memory bugs, and incremental adoption works well via C FFI. Reflects the actual mid-2026 industry consensus rather than language advocacy.

## hftbacktest — Rust HFT Backtesting Framework {#hftbacktest}

**Source:** [GitHub](https://github.com/nkaz001/hftbacktest) · <span class="lvl lvl-a">advanced</span> · active (Dec 2025 release)

The flagship open-source Rust project in HFT: a tick-level backtesting framework and live trading bot that models limit-order queue position, order-book reconstruction and feed/order latencies — the details naive backtesters ignore. Written mostly in Rust with Python bindings (4k+ stars). Demonstrates idiomatic low-latency Rust (zero-copy event processing, preallocated structures) and doubles as a study codebase for market-microstructure simulation.

## Mechanical Sympathy Google Group {#mech-sympathy}

**Source:** [groups.google.com](https://groups.google.com/g/mechanical-sympathy) · <span class="lvl lvl-i">intermediate</span> · 2012–present

The discussion group founded by Martin Thompson in 2012 for "coding sympathetically for the underlying platform". Traffic is far below its 2012–2018 peak but it remains alive (threads as recent as May 2025), and its archive is an unmatched Q&A corpus with answers from Thompson, Gil Tene and Peter Lawrey. Thompson's companion blog ([mechanical-sympathy.blogspot.com](https://mechanical-sympathy.blogspot.com/)) is dormant but still the origin of the movement's key essays.

## Databento Microstructure Guides (ITCH, OUCH, MoldUDP64) {#databento-micro}

**Source:** [databento.com](https://databento.com/microstructure/itch) · <span class="lvl lvl-b">beginner</span> · 2023–2026, actively maintained

A free, modern reference series explaining the protocols every HFT engineer must know: Nasdaq TotalView-ITCH market-by-order feeds over MoldUDP64, OUCH order entry over SoupBinTCP, and concepts like tick-to-trade latency. Far more approachable than the raw exchange PDFs while linking to them. Databento's engineering blog is arguably the most actively maintained public HFT-engineering publication of 2024–2026.

## Ultra Low-Latency Trading Systems blog (SubMicroTrading) {#submicro}

**Source:** [submicro.blogspot.com](https://submicro.blogspot.com) · [code on GitHub](https://github.com/gsitgithub/SubMicroTrading) · <span class="lvl lvl-a">advanced</span> · 2015 (historical)

Richard Rose's blog about the techniques used to build SubMicroTrading, a sub-microsecond Java trading framework. Dormant since 2015, but the entire framework has since been open-sourced, so you can read the actual code behind the blog posts. Kept as a worked example of extreme GC-free Java engineering.

## Mark Price — epickrram (LMAX) blog {#epickrram}

**Source:** [epickrram.blogspot.com](https://epickrram.blogspot.com/) · Jitter posts: [part 1](https://epickrram.blogspot.com/2015/09/reducing-system-jitter.html), [part 2](https://epickrram.blogspot.com/2015/11/reducing-system-jitter-part-2.html) · <span class="lvl lvl-a">advanced</span> · 2015–2020 (dormant)

A blog by an LMAX Exchange engineer, best known for the two-part "Reducing system jitter" series: a methodical hunt for OS-induced latency spikes using isolation, tracing and measurement, on a real trading workload. Dormant since 2020 but the jitter posts remain an excellent worked example of the diagnose-then-tune loop that separates engineering from checklist-copying.
