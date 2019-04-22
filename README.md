# Low Latency System

A collection of everything about low latency system.

The goal is to collect resouces to help people to get better performance in their system.

## Level a message
If you have anything about this project, please submit a [GitHub issue here](https://github.com/datar/low-latency-system/issues).

## Linux OS tuning

- [Redhat Linux Performance Tuning Guide](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html-single/performance_tuning_guide/index)
**A basic manual of tuning Linux OS.** It works with RHEL and Centos. Please select the right version number for your system.

- [Red Hat Enterprise Linux Network Performance Tuning Guide](https://access.redhat.com/articles/1391433) How to tune network stack in kernel. A low latency system should use kernel bypass instead of kernel stack to improve the performance.

 
## System tuning guides from server vendors

- [Configuring and tuning HP ProLiant Servers for low-latency applications](https://support.hpe.com/hpsc/doc/public/display?docLocale=en_US&docId=emr_na-c01804533)
This document will be updated months. The latest version is 201710.

- [HPE Trade and Match Server: HPE ProLiant XL1x0r Gen9 servers using Intel Xeon E5-1680 v3 processors](https://www.hpe.com/h20195/v2/getpdf.aspx/4AA6-6103ENW.pdf?ver=4.0) A solution which uses 1 overclocking CPU to get best performance. 

- [HPE Gen10 Servers Intelligent System
Tuning](https://support.hpe.com/hpsc/doc/public/display?docId=a00018313en_us) HP's new tech to remove jitters in G10 system.

- [HP Gen8 technologies for low latency, high performance trading and exchanges](http://www.flaggmgmt.com/hpc/ppt/session-9_patrickgreene.pdf)

- [Supermicro HFT-Optimized Servers and the ExaNIC](https://exablaze.com/media/blog-supermicro-latency)

- [Configuring Low-Latency Environments on Dell PowerEdge Servers](http://www.dell.com/downloads/global/products/pedge/en/configuring_dell_powerEdge_servers_for_low_latency_12132010_final.pdf)

- [Optimizing the High Frequency Trading GatiRT* Application on the latest Intel® Architecture Server](https://software.intel.com/en-us/articles/optimizing-the-high-frequency-trading-gatirt-application-on-the-latest-intel-architecture)


- [HP Gen8 technologies for low latency, high performance trading and exchanges](http://www.flaggmgmt.com/hpc/ppt/session-9_patrickgreene.pdf)

- [Reducing OS-Jitter, (Frame Latency, Latency, Stutter)](http://www.tomshardware.com/faq/id-2477508/reducing-jitter-frame-latency-latency-stutter.html)

- [Performance Analysis and Tuning of Red Hat Enterprise Linux](https://github.com/major/redhat-summit-2015-notes/blob/master/2015-06-24/Performance%20Analysis%20and%20Tuning%20of%20Red%20Hat%20Enterprise%20Linux.md)

- [Optimizing Linux performance on HP Integrity Superdome X](http://www8.hp.com/h20195/v2/GetPDF.aspx/4AA5-9698ENW.pdf)

- [Optimizing for low latency](https://software.intel.com/sites/default/files/managed/07/f3/low_latency_presentation.pdf)
- [Blog from a guy working in LMAX](http://epickrram.blogspot.co.uk/)
    - [Reducing system jitters part 1](http://epickrram.blogspot.co.uk/2015/09/reducing-system-jitter.html)
    - [Reducing system jitters part 2](http://epickrram.blogspot.co.uk/2015/11/reducing-system-jitter-part-2.html)

- [Linux Performance and Tuning Guidelines](https://lenovopress.com/redp4285)
    - [中文版](https://www.gitbook.com/book/lihz1990/transoflptg/details)

- [Network tuning and performance](https://calomel.org/network_performance.html)

- [Solarflare Server Adapter User Guide:Performance Tuning on Linux](http://solarflare.com)

## Benchmarks and tools

- STAC
  - [SUT ID: SFC170206 STAC-Network IO UDP over 10GbE using OpenOnload on RHEL 6.6 with Solarflare SFN 8522-PLUS Adapters on HPE ProLiant XL170r Gen9 Trade & Match Servers](https://stacresearch.com/SFC170206) This test discloses the configurations. 

## Tools

- Sockperf

- iperf

## Faster than 10G network
- [Extreme Ethernet Server Networking: 10GbE, 25GbE, 40GbE & 50GbE](http://10gbe.blogspot.com/)

- [A lecture talk about Linux kernel networking](http://www.haifux.org/lectures/172/netLec.pdf)

- [A great blog from cloudflare](https://blog.cloudflare.com)

    - [How to receive a million packets](https://blog.cloudflare.com/how-to-receive-a-million-packets/)
    - [Why we use the Linux kernel's TCP stack](https://blog.cloudflare.com/why-we-use-the-linux-kernels-tcp-stack/)

- [Multi-queue network interfaces with SMP on Linux](https://greenhost.nl/2013/04/10/multi-queue-network-interfaces-with-smp-on-linux/)
  - [中译本：linux下基于SMP架构的多队列网卡的调优](http://blog.csdn.net/vah101/article/details/38615795)

## Low Level API

- IB verbs

- ef_vi

- libexanic

## Kernel bypass

## Solarflare
- [OpenOnload Google Talk](http://www.openonload.org/openonload-google-talk.pdf)
Compared different accelate technology
approach that solarflare used in openonload
compared latency, bandwidth by using openonload and kernel

- [ASICs argued with FPGAs](https://www.hotchips.org/wp-content/uploads/hc_archives/hc24/HC24-2-Fabric/HC24.28.220-FPGA-ASICs-Riddoch-Solarflare.pdf)

- [TCP on FPGA made practical and fast](https://www.brighttalk.com/webcast/11615/176711)

## Infiniband, RMDA

- [THE GEEK IN THE CORNER Programming odds and ends — InfiniBand, RDMA, and low-latency networking for now.](https://thegeekinthecorner.wordpress.com)

  A blog for RDMA programming and concept

  - [An introduction of infiniband and RDMA](https://thegeekinthecorner.wordpress.com/category/infiniband-verbs-rdma/)

- [RDMA programming concepts](https://www.openfabrics.org/images/eventpresos/workshops2013/IBUG/2013_UserDay_Thur_1400_Bob-Russell-programming-concepts.pdf)

- [InfiniBand Technology Overview](http://www.snia.org/sites/default/education/tutorials/2008/spring/networking/Goldenberg-D_InfiniBand_Technology_Overview.pdf) An advanced introduction to IB

- [introduction to Infiniband](http://services.geant.net/edupert/Resources/Documents/IntroductiontoInfiniband_Hussein.pdf) just a simple introduction. Only advantage is there are many diagram.

- [Introduction to InfiniBand](http://www.mellanox.com/pdf/whitepapers/IB_Intro_WP_190.pdf) 
 An introduction to IB from Mellanox whitepaper.


- [RDMA aware programming user manual](http://www.mellanox.com/related-docs/prod_software/RDMA_Aware_Programming_user_manual.pdf)

- [OpenOnload NIC / Infiniband RDMA](http://www.cnblogs.com/slime/archive/2010/10/20/1857134.html)

- [Designing Cloud and Grid Computing Systems with InfiniBand and High-Speed Ethernet](http://www.ics.uci.edu/~ccgrid11/files/ccgrid11-ib-hse_last.pdf)

- [Get Started with Infiniband](http://people.redhat.com/dledford/infiniband_get_started.html)

- [PF_RING](http://www.ntop.org/products/packet-capture/pf_ring/pf_ring-zc-zero-copy/)

## Zero Copy

- [Zero Copy I: User-Mode Perspective](http://www.linuxjournal.com/article/6345)

## Mellanox
- Performance Tuning Guide for Mellanox Network Adapters

  - [out date](http://www.mellanox.com/related-docs/prod_software/Performance_Tuning_Guide_for_Mellanox_Network_Adapters.pdf)
  - [the latest](https://community.mellanox.com/docs/DOC-2489)

How to tune libvma and other tools from mellanox to improve network performance.

- [VMA Performance Tuning Guide](https://community.mellanox.com/docs/DOC-2797)

- [Mellanox AcademyIntroduction to InfiniBand](https://rmacc.org/sites/default/files/RMACC%208-10-15.pdf)


## Time tech

- [IEEE 1588 & PTP USING EMBEDDED LINUX SYSTEMS](http://events.linuxfoundation.org/sites/events/files/slides/elc_insop_2015.pdf)

- [How to Benchmark Code Execution Times in Intel IA-32 and IA-64 Instruction Set Architectures](https://www.intel.com/content/www/us/en/embedded/training/ia-32-ia-64-benchmark-code-execution-paper.html)

## Jitter detection

- [sysjitter from solarflare](http://www.openonload.org/download.html) 

  A tool that measures the jitter experienced by user-level threads running on each CPU core of a computer. Such jitter may be caused by interrupts, OS threads, system management interrupts or other processes on the system.
  
  All openOnload download is here [http://www.openonload.org/download/](http://www.openonload.org/download/)
  
- [Erez's hiccup tools](https://github.com/erez-strauss/hiccups)
  
  A tool used for measure OS hiccup.
  
- [HP-timetest](mailto:low.latency@hp.com)
  I don't get this tool now. Need help.
- [Google Group: Mechanical-Sympathy]() 
    - [A Toolkit to Measure Basic System Performance & OS Jitter](https://groups.google.com/d/msg/mechanical-sympathy/GuYJZIifrZE/SbF5dQs8dR0J)
    - [Systemic to reduce Linux OS jitter](https://groups.google.com/forum/#!topic/mechanical-sympathy/DWlziVmyW-w)



## Unsort

- [Ultra Low Latency Trading Systems: A blog about techniques used to build www.SubMicroTrading.com](submicro.blogspot.com)
- [Performance Analysis](https://wiki.mikejung.biz/Performance_Analysis)
- [A Low-Latency Solution for High Frequecy Trading From IBM and Solarflare](http://www.moderntech.com.hk/sites/default/files/whitepaper/V10_A%20Low-Latency_Solution_for_High-Frequency_Trading_from_IBM_and_Solarflare_0.pdf)
It's a very old solution that looks like system has not been tuned well.
- [Low Latency in Linux kernel](http://www.linuxdevcenter.com/pub/a/linux/2000/11/17/low_latency.html)
 An article from 2000. Very very old.
- [Optimizing System Performance](http://linux-sunxi.org/Optimizing_system_performance)

- [Design Best Practices for Latency Optimization](https://www.cisco.com/application/pdf/en/us/guest/netsol/ns407/c654/ccmigration_09186a008091d542.pdf)
- [Low Latency Optimization](https://community.dev.hpe.com/t5/tkb/articleprintpage/tkb-id/bigdata_wiki_vertica/article-id/73)

- [Intel i350 NIC datasheet](http://www.intel.com/content/dam/www/public/us/en/documents/datasheets/ethernet-controller-i350-datasheet.pdf)

- [Network in Linux kernel](https://wiki.openwrt.org/doc/networking/praxis)

- [Understanding TCP/IP network stack and writing network programming](http://www.cubrid.org/blog/dev-platform/understanding-tcp-ip-network-stack/)

- [Trading floor architecture](http://www.cisco.com/c/en/us/td/docs/solutions/Verticals/Trading_Floor_Architecture-E.html)

- [Latency in high performance trading system](https://cs.uwaterloo.ca/~mkarsten/cs856-W10/lec05-abridged.pdf)

- [How does an algorithmic trading system architecture look like](https://www.quora.com/How-does-an-algorithmic-trading-system-architecture-look-like)

- [Evolution and practice: Low-latency Distributed Applications in Finance](https://www.quora.com/How-does-an-algorithmic-trading-system-architecture-look-like)

- [Technology in banking – a problem in scale and complexity](http://web.stanford.edu/class/ee380/Abstracts/110511-slides.pdf)

- [Low-latency networks for trading infrastructure Fujitsu](https://www.fujitsu.com/us/Images/LowlatencyTrading.pdf
) A introduction of Fujitsu's optical infrastruction.

- [Topics in High Performance Messaging](https://www.informatica.com/downloads/1568_high_perf_messaging_wp/Topics-in-High-Performance-Messaging.htm)

## Report, Whitepaper

- [Tolly Test Report:Mellanox Spectrum vs. Broadcom StrataXGS Tomahawk 25GbE & 100GbE Performance Evaluation](http://www.mellanox.com/related-docs/products/tolly-report-performance-evaluation-2016-march.pdf)
- [The Next Frontiers for Low Latency Programmers](http://www.mellanox.com/blog/2016/07/the-next-frontiers-for-low-latency-programmers/)
- [Webinar from Mellanox: The latest Trend of Low Latency Programming](http://www.mellanox.com/webinars/2016/trend-low-latency/MLNX-LowLatencyWebinar-072816.mp4)
- [Verbs Programming, Raw Ethernet](https://community.mellanox.com/docs/DOC-2517) 
- [Low Latency Optimization](https://community.dev.hpe.com/t5/tkb/articleprintpage/tkb-id/bigdata_wiki_vertica/article-id/73)
- [Tolly Test Report:Mellanox Spectrum vs. Broadcom StrataXGS Tomahawk 25GbE & 100GbE Performance Evaluation](http://www.mellanox.com/related-docs/products/tolly-report-performance-evaluation-2016-march.pdf)
- [The Next Frontiers for Low Latency Programmers](http://www.mellanox.com/blog/2016/07/the-next-frontiers-for-low-latency-programmers/)
- [Webinar from Mellanox: The latest Trend of Low Latency Programming](http://www.mellanox.com/webinars/2016/trend-low-latency/MLNX-LowLatencyWebinar-072816.mp4)
- [Verbs Programming, Raw Ethernet](https://community.mellanox.com/docs/DOC-2517) 

## FPGA

- [algo-logic](http://algo-logic.com/)

- [A Low Latency Library in FPGA Hardware for High Frequency Trading (HFT)](http://www.hoti.org/hoti20/slides/Lockwood_AlgoLogic.pdf) An article from alog-logic. Here is the external link. The origin link on algo-logic.com is 404 error.

- [Efficient Event Processing through Reconfigurable Hardware for Algorithmic Trading](http://msrg.org/publications/pdf_files/2010/moTrading-Efficient_Event_Processing_through_.pdf)

- [NetFPGA-10G Information](http://netfpga.org/10G_specs.html) A FPGA solution in 2012. Should update to latest news.

- [FPGA networking processing card](http://www.nallatech.com/solutions/fpga-network-processing/)

- [FPGA smart NIC card](http://meanderful.blogspot.com/2013/01/fpga-smart-nic-cards.html) A list of FPGA solution of NIC. A 2013 article, it's a little expired. 

- [FPGA options for ultra low latency HFT with stunningly convincing Youtube video with Algo Logic with some technical papers](http://quantlabs.net/blog/2013/02/fpga-options-for-ultra-low-latency-hft-with-stunningly-convincing-youtube-video-with-algo-logic-with-some-technical-papers/) A 2013 article also.

- [Xeon+FPGA platform for data center](https://www.ece.cmu.edu/~calcm/carl/lib/exe/fetch.php?media=carl15-gupta.pdf)

- [Intel hybrid CPU+FPGA](http://www.embeddedintel.com/commentary.php?article=2143)

- [FPGA developer](http://www.fpgadeveloper.com/) A FPGA site that focus on tech not finance solution.

- [How are HFT systems implemented on FPGA nowadays](http://quant.stackexchange.com/questions/10519/how-are-hft-systems-implemented-on-fpga-nowadays) A QA on stackexchange


## resource

- [intelligent trading technology from A-Team](http://intelligenttradingtechnology.com/)

- [nextplatform](http://www.nextplatform.com)

## About

- [My LinkedIn Page](https://www.linkedin.com/in/masterchen/)
- [chenxing@live.com](mailto:chenxing@live.com)
- 微信 Wechat: trador
