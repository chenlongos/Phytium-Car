# 任务一：PCIe 主桥的初始化

# PCIe总览

## 概述

### 何为PCIe

PCIe（Peripheral Component Interconnect Express），是一种高速串行计算机扩展总线标准，用于连接计算机内部的硬件设备。PCIe是PCI的后继者，旨在提供更高的带宽和性能。

关键特点：

1. **高速串行连接：** 与传统的PCI总线使用并行连接不同，PCIe采用高速串行连接。这使得数据能够以更高的速率进行传输，提供更大的带宽。
2. **多通道架构：** PCIe采用多通道（lanes）的结构，每个通道都能独立传输数据。每个通道的带宽是独立分配的，通常以“x1”、“x4”、“x8”和“x16”等倍数来表示。例如，PCIe x16插槽提供比PCIe x1插槽更大的带宽。
3. **点对点连接：** 每个PCIe设备都与主板上的PCIe插槽之间建立一个点对点的连接。这种连接方式与传统PCI总线的共享连接方式相比，更有效地支持高带宽需求。
4. **热插拔支持：** PCIe支持热插拔，允许用户在计算机运行时插入或拔出PCIe设备，而无需重新启动计算机。
5. **兼容性：** PCIe是一种通用的总线标准，广泛应用于各种设备，包括显卡、网卡、固态硬盘、扩展卡等。

### 树莓派中Linux实现

在树莓派Linux系统中，输入```cat /proc/iomem```，得到如下输出：

```shell
pi@raspberrypi:~ $ sudo cat /proc/iomem
00000000-37ffffff : System RAM
  00008000-00ffffff : Kernel code
  01200000-01434083 : Kernel data
40000000-fbffffff : System RAM
fd500000-fd50930f : fd500000.pcie pcie@7d500000
fd580000-fd58ffff : fd580000.ethernet ethernet@7d580000
  fd580e14-fd580e1c : unimac-mdio.-19
fe007000-fe007aff : fe007000.dma dma@7e007000
fe007b00-fe007eff : fe007b00.dma dma@7e007b00
fe00a000-fe00a023 : fe100000.watchdog watchdog@7e100000
fe00b840-fe00b87b : fe00b840.mailbox mailbox@7e00b840
fe00b880-fe00b8bf : fe00b880.mailbox mailbox@7e00b880
fe100000-fe100113 : fe100000.watchdog watchdog@7e100000
fe101000-fe102fff : fe101000.cprman cprman@7e101000
fe104000-fe104027 : fe104000.rng rng@7e104000
fe200000-fe2000b3 : fe200000.gpio gpio@7e200000
fe201000-fe2011ff : serial@7e201000
  fe201000-fe2011ff : fe201000.serial serial@7e201000
fe215000-fe215007 : fe215000.aux aux@7e215000
fe300000-fe3000ff : fe300000.mmcnr mmcnr@7e300000
fe340000-fe3400ff : fe340000.mmc mmc@7e340000
fe600000-fe6000ff : fe600000.firmwarekms firmwarekms@7e600000
fec00000-fec03fff : fec00000.v3d hub
fec04000-fec07fff : fec00000.v3d core0
fec11000-fec1101f : fe100000.watchdog watchdog@7e100000
600000000-63fffffff : pcie@7d500000
  600000000-6000fffff : PCI Bus 0000:01
    600000000-600000fff : 0000:01:00.0
      600000000-600000fff : xhci-hcd
```

* 由此```fd500000-fd50930f : fd500000.pcie pcie@7d500000```可以知道，

  `fd500000` 是 PCIe 控制器的基地址，`pcie@7d500000` 是 PCIe 控制器的设备节点。这个地址范围包含了 PCIe 控制器的寄存器，用来配置和控制 PCIe 总线。

* 由此 可以知道，

  ```shell
  600000000-63fffffff : pcie@7d500000
    600000000-6000fffff : PCI Bus 0000:01
      600000000-600000fff : 0000:01:00.0
        600000000-600000fff : xhci-hcd
  ```

  `600000000` 是 PCIe 总线的基地址，`pcie@7d500000` 是总线的设备节点。这个地址范围包含了连接到 PCIe 总线上的所有设备的地址范围。

  * **`600000000-63fffffff : pcie@7d500000`：**
    * PCIe 控制器在物理地址空间中的基地址范围。
    * PCIe 控制器负责管理 PCIe 总线上的所有连接设备，包括配置总线上的设备、处理中断、以及协调数据传输等操作。

  * **`600000000-6000fffff : PCI Bus 0000:01`：**
    * 表示 PCI Express 总线上的一个具体的子总线（Bus 0000:01）。PCI Express 总线通常包含多个子总线，每个子总线可能连接一个或多个设备。
    * 子总线的地址范围是从 `600000000` 到 `6000fffff`。

  * **`600000000-600000fff : 0000:01:00.0`：**
    * 表示具体的 PCIe 设备，这里是一个 xHCI 控制器，其设备地址是 `0000:01:00.0`。
    * xhci（eXtensible Host Controller Interface）是用于支持 USB 3.0 和 USB 3.1 标准的主机控制器接口。在这个地址范围内，xhci 控制器有可能包含寄存器、缓冲区等。

  * **`600000000-600000fff : xhci-hcd`：**
    * 这是 xchi 控制器的具体地址范围，表示 xchi 控制器在物理地址空间中的寄存器范围。`xhci-hcd` 是 Linux 中对 xchi 控制器的驱动程序的命名。

  

  ## 关于 PCIe总线 的初始化

  

   ### 相关寄存器介绍

  

  ### 具体寄存器配置

  

  ### 代码分析

  

  

  





