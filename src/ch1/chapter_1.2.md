# 第一步：PCIe 总线

## 总线

**总线**是连接各个部件的信息传输线，是各个部件共享的传输介质。总线充当了计算机内各个硬件组件之间的通信媒介，使得中央处理器（CPU）、内存、输入输出设备等能够协同工作。

总线分类：

* **片内总线：** 片内总线是指在芯片内部连接各个元件的总线。这些元件可以包括内部的寄存器、缓存、算术单元等。片内总线用于在芯片内部传递数据和控制信号，以协调不同部件的工作。

* **系统总线：** 系统总线是连接计算机主要部件的总线。连接了中央处理器、内存、输入输出控制器等主要组件。系统总线用于在这些主要组件之间传递地址、数据和控制信号，协调它们之间的工作。

* **通信总线：** 通信总线，外部总线，是计算机系统之间或计算机系统与其他系统之间的通信通道。它包括串行传输和并行传输。

## PCIe总线

**PCIe**（Peripheral Component Interconnect Express）是一种计算机总线标准，用于连接内部硬件设备，例如图形卡、存储设备、网卡等。PCIe 是 PCI 标准的一种现代化、高速、并行的演进版本，提供更高的数据传输速率和更强大的性能。

PCIe 具有以下特点：

* **点对点连接：** PCIe 采用点对点（point-to-point）连接架构，每个 PCIe 设备直接连接到主板上的 PCIe 控制器，而不是共享总线。这种点对点连接架构消除了传统总线结构中的冲突和瓶颈，提高了系统性能。

* **高速串行传输：** PCIe 使用高速的差分串行通信，相较于传统的并行总线，提供更高的数据传输速率。这使得 PCIe 能够支持更大的带宽需求，适应了现代高性能计算和通信的要求。

* **可伸缩性：** PCIe 提供了可伸缩性，支持不同数量和配置的通道，例如 PCIe x1、PCIe x4、PCIe x8 和 PCIe x16。这使得 PCIe 能够适应各种设备和应用的需求，从网络适配器到高性能图形卡。

* **热插拔支持：** PCIe 支持热插拔功能，允许用户在系统运行时插入或拔出 PCIe 设备，而无需关闭系统。这增加了系统的灵活性和可维护性。


### 背景知识

ISA（Industry Standard Architecture）总线是一种计算机总线标准，它的前身可以追溯到1981年IBM推出的个人电脑（PC）。当时，IBM为了连接主板上的微处理器和外部设备（如显卡、声卡等），设计了一个内部通信接口。这个早期的总线被称作PC总线。随着时间的发展，IBM在其1984年推出的IBM PC/AT（Advanced Technology）中引入了一个新版本的总线，这就是ISA总线的前身。它最初是16位的，并且比之前的8位PC总线提供了更高的数据传输速率。ISA总线不仅能兼容新的16位卡，而且还向后兼容之前的8位扩展卡。尽管ISA总线曾经非常流行，但它基于并行通信原理，导致传输速度相对较慢且存在长度限制，这使得它难以满足日益增长的数据传输需求。

英特尔在1992年首次提出了 PCI（Peripheral Component Interconnect） 总线的概念，旨在解决 ISA 总线的瓶颈问题。PCI 是一种并行总线标准，最初被设计为用于连接外部设备，如网卡、显卡、磁盘控制器等。之后，PCI 很快成为一种开放标准，得到了多家硬件制造商的支持。这使得各种设备能够在不同厂商的计算机系统中互通性更好。

随着计算机技术的发展，PCI总线标准逐渐显露出一些限制，如带宽瓶颈和对未来高性能设备的支持不足。因此，产业界开始寻找一种更先进的替代方案。英特尔在2003年首次提出了 PCI Express （Peripheral Component Interconnect Express）的概念，作为对传统 PCI 标准的更新和改进。PCIe 最初被设计为一种高性能、灵活性更强的串行总线标准，以适应未来计算机的需求。

### 发展历程

1. **PCIe 1.0：** PCIe 1.0 标准于2003年发布。它引入了高速串行连接，每个通道的数据速率为2.5 GT/s（gigatransfers per second）。PCIe 1.0 提供了比传统的并行总线更高的性能和可扩展性。
2. **PCIe 2.0：** PCIe 2.0 标准于2007年发布。它在每个通道上将数据速率提高到5 GT/s，从而提供了更大的总线带宽。PCIe 2.0 向下兼容 PCIe 1.0，并为图形卡和其他高性能设备提供了更多的带宽。
3. **PCIe 3.0：** PCIe 3.0 标准于2010年发布。它将每个通道的数据速率进一步提高到8 GT/s，进一步增加了总线带宽。PCIe 3.0 的发布支持更高分辨率的图形、更快速的存储设备和更多高带宽应用。
4. **PCIe 4.0：** PCIe 4.0 标准于2017年发布。它将每个通道的数据速率提高到16 GT/s，实现了比 PCIe 3.0 更高的带宽。PCIe 4.0 的推出对于数据中心、高性能计算和其他需要更高带宽的应用具有重要意义。
5. **PCIe 5.0：** PCIe 5.0 标准于2019年发布。它将每个通道的数据速率提高到32 GT/s，再次增加了总线带宽。PCIe 5.0 的推出进一步满足了对更高性能和更大带宽的需求。
6. **PCIe 6.0：** PCIe 6.0 标准于2022年发布。它将每个通道的数据速率提高到64 GT/s，为未来计算机系统提供更大的带宽和性能。PCIe 6.0 的推出标志着 PCIe 标准的不断创新和发展。

<table>         
    <thead>            
        <tr>                 
            <th rowspan="2">PCIe Specification(版本)</th>                 
            <th rowspan="2">Specification ratification year(发布时间)</th>                 
            <th rowspan="2">Encoding(有效数据)</th>                 
            <th rowspan="2">Data Rate per lane(传输速率)</th>                 
            <th colspan="5">Throughput(吞吐量)</th>             
        </tr>             
        <tr>                 
            <th>x1</th>                 
            <th>x2</th>                 
            <th>x4</th>                 
            <th>x8</th>                 
            <th>x16</th>             
        </tr>         
    </thead>         
    <tbody>                         
        <tr>                 
            <td>1.0</td>                 
            <td>2003</td>                 
            <td>8b/10b</td>                 
            <td>2.5GT/s</td>                 
            <td>250MB/s</td>                 
            <td>0.500GB/s</td>                 
            <td>1.000GB/s</td>                 
            <td>2.000GB/s</td>                 
            <td>4.000GB/s</td>             
        </tr>             
        <tr>                 
            <td>2.0</td>                 
            <td>2007</td>                 
            <td>8b/10b</td>                 
            <td>5.0GT/s</td>                 
            <td>500MB/s</td>                 
            <td>1.000GB/s</td>                 
            <td>2.000GB/s</td>                 
            <td>4.000GB/s</td>                 
            <td>8.000GB/s</td>             
        </tr>             
        <tr>                 
            <td>3.0</td>                 
            <td>2010</td>                 
            <td>128b/130b</td>                 
            <td>8.0GT/s</td>                 
            <td>985MB/s</td>                 
            <td>1.969GB/s</td>                 
            <td>3.938GB/s</td>                 
            <td>7.877GB/s</td>                 
            <td>15.754GB/s</td>             
        </tr>             
        <tr>                 
            <td>4.0</td>                 
            <td>2017</td>                 
            <td>128b/130b</td>                 
            <td>16.0GT/s</td>                 
            <td>1.969GB/s</td>                 
            <td>3.938GB/s</td>                 
            <td>7.877GB/s</td>                 
            <td>15.754GB/s</td>                 
            <td>31.508GB/s</td>             
        </tr>             
        <tr>                 
            <td>5.0</td>                 
            <td>2019</td>                 
            <td>128b/130b</td>                 
            <td>32.0GT/s</td>                 
            <td>3.938GB/s</td>                 
            <td>7.877GB/s</td>                 
            <td>15.754GB/s</td>                 
            <td>31.508GB/s</td>                 
            <td>63.015GB/s</td>             
        </tr>             
        <tr>                 
            <td>6.0</td>                 
            <td>2022</td>                 
            <td>1b/1b</td>                 
            <td>64.0GT/s</td>                 
            <td>7.563GB/s</td>                 
            <td>15.125GB/s</td>                 
            <td>30.250GB/s</td>                 
            <td>60.500GB/s</td>                 
            <td>121.000GB/s</td>             
        </tr>             
    </tbody>    
</table>


### PCIe链路

PCIe Link表示两个组件之间的全双工通信信道，是PCIe总线中两个设备之间的通信连接，采用点对点的通信架构，一条链路可以包含多个通道(lane)，可增加通道个数来满足更高的带宽要求。

![image-20231226120533938](https://github.com/apengaaa/usb-doc/blob/master/src/assert/1.21-1.png)



Lane，是指一组差分信号的组合，包括发送和接收。一个发送方向的差分信号包括TX+和TX-两条线，接收亦然。所以一条lane有四条物理连线。

![image-20231226143119817](https://github.com/apengaaa/usb-doc/blob/master/src/assert/1.21-2.png)



### PCIe拓扑结构

![image-20231226120936484](https://github.com/apengaaa/usb-doc/blob/master/src/assert/1.21-3.png)

1. **点对点连接：** PCIe总线采用点对点的通信方式，每个设备都直接连接到根复杂器（Root Complex）或其他中继设备。这种结构消除了共享总线的瓶颈，提供了更高的性能。
2. **树状结构：** PCIe的拓扑结构通常呈现为树状结构。根复杂器是总线的起点，连接到根复杂器的设备称为端点（Endpoint）。可以通过 Switch 进一步连接到其他设备，形成一个树状的拓扑结构。
3. **根复杂器 Root Complex（RC）：** RC是PCIe总线的起点，负责初始化和管理总线。RC可以连接到CPU、北桥芯片组或其他I/O控制器。
4. **PCIe Bridge To PCI/PCI-X**： 桥接设备，可以连接不同的PCIe域，例如连接其他的PCI总线、PCI-X、PCIe总线。
5. **Switch**： PCIe的转接器设备（PCIe扩展端口），为挂载设备提供路由以及转发服务；Switch的内部结构可以看作由PCI桥组成，在Switch中，每个端口对应的PCIe设备号是确定的，Switch设备会记录下游PCIe端口连接设备分配到的PCIe地址，在接收到TLP包时，通过比较目的地址和下游设备的地址，来判断是否转发以及转发到哪个PCIe端口。
6. **端点设备 PCIe Endponit（EP）：** 端点是直接连接到PCIe总线的终端设备，如显卡、网卡等。每个设备都有自己的设备标识和唯一的设备ID。
7. **链路：** 链路是连接PCIe设备的通信通道，可以根据需要进行配置。链路的宽度（Link Width）和速度（Link Speed）是PCIe连接的关键参数，决定了数据传输的带宽和速率。
8. **多域拓扑：** PCIe支持多域拓扑，允许将PCIe总线划分为不同的域，每个域内有独立的根复杂器和设备。

### PCIe架构

PCIe架构包括四个主要层次：物理层（Physical Layer）、数据链路层（Data Link Layer）、传输层（Transaction Layer）、应用层（Application Layer）。

#### 1. 物理层（Physical Layer）：

- **电气特性：** 定义了PCIe信号的电气特性，包括电压水平、时钟频率等。PCIe使用差分信号传输，以提高抗干扰性和传输速率。
- **连接器和插槽：** 规定了物理连接的标准，包括插槽的形状和尺寸，以及连接器的设计。常见的物理连接形式包括x1、x4、x8、x16等。
- **数据传输：** 负责将数据以数据包的形式传输到数据链路层。物理层还管理时序、流控和数据逐字节的传输。

#### 2. 数据链路层（Data Link Layer）：

- **数据包组装：** 将数据划分为适当大小的数据包，并在每个数据包上添加控制信息，包括起始和终止标志、错误检测和纠错码等。
- **流量控制：** 确保数据在物理层上的稳定传输，避免因接收方处理能力不足而导致数据丢失。
- **错误检测和纠错：** 负责检测并在可能的情况下纠正传输中的错误，以确保数据的可靠性。

#### 3. 传输层（Transaction Layer）：

- **传输事务：** 负责将数据包分解为传输层事务，并在设备之间传递这些事务。每个事务包含有关数据的信息、目标设备的地址等。
- **路由：** 确定数据包的路径，使其能够正确地传递到目标设备。传输层支持多通道和多队列，以提高并行性和性能。
- **可靠性：** 确保数据传输的可靠性，包括重传机制、流控等。

#### 4. 应用层（Application Layer）：

- **与操作系统和应用程序的接口：** 提供PCIe设备与主机系统之间的通信接口。在这一层，操作系统通过相应的驱动程序与PCIe设备进行交互。
- **配置和管理：** 应用层处理PCIe设备的配置信息，包括设备ID、中断分配等。此外，它还负责设备的启动和关闭。

### 树莓派Linux中关于 PCIe 部分

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

  

## 关于 PCIe 总线的初始化


### 相关寄存器介绍


### 具体实例（树莓派Linux系统）

```shell
pi@yahboom:~$ lspci
00:00.0 PCI bridge: Broadcom Inc. and subsidiaries BCM2711 PCIe Bridge (rev 20)
01:00.0 USB controller: VIA Technologies, Inc. VL805 USB 3.0 Host Controller (rev 01)
pi@raspberrypi:~ $ sudo lspci -xxx -s 00:00.0
00:00.0 PCI bridge: Broadcom Limited Device 2711 (rev 20)
00: e4 14 11 27 46 01 10 00 20 00 04 06 10 00 01 00
10: 00 00 00 00 00 00 00 00 00 01 01 00 00 00 00 00
20: 00 c0 00 c0 f1 ff 01 00 00 00 00 00 00 00 00 00
30: 00 00 00 00 48 00 00 00 00 00 00 00 3e 01 03 00
40: 00 00 00 00 00 00 00 00 01 ac 13 48 08 20 00 00
50: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
60: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
70: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
80: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
90: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
a0: 00 00 00 00 00 00 00 00 00 00 00 00 10 00 42 00
b0: 02 80 00 00 10 2c 00 00 12 5c 65 00 00 00 12 90
c0: 00 00 00 00 00 00 40 00 18 00 01 00 00 00 00 00
d0: 1f 08 08 00 00 00 00 00 06 00 00 80 02 00 00 00
e0: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
f0: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
```



  

### 代码实现

```rust
#![allow(unused)]
fn main() {
//相关寄存器地址及配置
register_bitfields![
    u32,
    //  Broadcom STB PCIe Register Offsets
    // 0x0188
    RC_CFG_VENDOR_VENDOR_SPECIFIC_REG1 [
        LITTLE_ENDIAN OFFSET(0) NUMBITS(1) [],
        ENDIAN_MODE_BAR2 OFFSET(0xC) NUMBITS(1) [],
    ],

    // 0x043c
    RC_CFG_PRIV1_ID_VAL3 [
        CLASS_ID  OFFSET(0) NUMBITS(24) [
            pcie_pcie_bridge = 0x060400
        ],
    ],
    // 0x04dc
    // PCIE_RC_CFG_PRIV1_LINK_CAPABILITY [],
    // 0x1100
    // RC_DL_MDIO_ADDR [],
    // 0x1104
    // RC_DL_MDIO_WR_DATA [],
    // 0x1108
    // RC_DL_MDIO_RD_DATA
    // 0x4008
    MISC_MISC_CTRL [
        SCB_ACCESS_EN OFFSET(12) NUMBITS(1) [],
        CFG_READ_UR_MODE OFFSET(13) NUMBITS(1) [],
        MAX_BURST_SIZE OFFSET(20) NUMBITS(2) [],
        SCB0_SIZE OFFSET(27) NUMBITS(5) [
            init_val = 0x17,
        ],
        SCB1_SIZE OFFSET(22) NUMBITS(5) [],
        SCB2_SIZE OFFSET(0) NUMBITS(5) [],
    ],
    // 0x400c
    MISC_CPU_2_PCIE_MEM_WIN0_LO [
        MEM_WIN0_LO OFFSET(0) NUMBITS(32) [
            // TODO
            init_val = 0x0000_0000
        ],
    ],
    // 0x4010
    MISC_CPU_2_PCIE_MEM_WIN0_HI [
        MEM_WIN0_HI OFFSET(0) NUMBITS(32) [
            init_val = 0x0000_0006
        ],
    ],
    // 0x4204
    // 0x402C
    MISC_RC_BAR1_CONFIG_LO [
        MEM_WIN OFFSET(0) NUMBITS(5)[]
    ],
    // 0x4034
    MISC_RC_BAR2_CONFIG_LO [
        VALUE_LO OFFSET(0) NUMBITS(32)[
            init_val = 0x11,
        ]
    ],
    // 0x4038
    MISC_RC_BAR2_CONFIG_HI [
        VALUE_HI OFFSET(0) NUMBITS(32)[
            init_val = 0x4,
        ]
    ],
    // 0x403C
    MISC_RC_BAR3_CONFIG_LO [
        MEM_WIN OFFSET(0) NUMBITS(5)[]
    ],
    // 0x4044
    // MISC_MSI_BAR_CONFIG_LO
    // 0x4048
    // MISC_MSI_BAR_CONFIG_HI
    // 0x404c
    // MISC_MSI_DATA_CONFIG
    // 0x4060
    // MISC_EOI_CTRL
    // 0x4064
    // MISC_PCIE_CTRL
    // 0x4068
    MISC_PCIE_STATUS [
        CHECK_BITS OFFSET(4) NUMBITS(2)[],
        RC_MODE OFFSET(7) NUMBITS(1)[],
    ],
    // 0x406c
    MISC_REVISION [
        MISC_REVISION OFFSET(0) NUMBITS(32)[]
    ],

    // 0x4070
    MISC_CPU_2_PCIE_MEM_WIN0_BASE_LIMIT [
        MEM_WIN0_BASE_LIMIT OFFSET(0) NUMBITS(32)[
            // TODO
            init_val = 0
        ]
    ],
    // 0x4080
    MISC_CPU_2_PCIE_MEM_WIN0_BASE_HI [
        MEM_WIN0_BASE_HI OFFSET(0) NUMBITS(32)[
            init_val = 6
        ]
    ],
    // 0x4084
    MISC_CPU_2_PCIE_MEM_WIN0_LIMIT_HI [
        MEM_WIN0_LIMIT_HI OFFSET(0) NUMBITS(32)[
            init_val = 6
        ]
    ],
    // 0x4204
    MISC_HARD_PCIE_HARD_DEBUG [
        CLKREQ_DEBUG_ENABLE OFFSET(0) NUMBITS(1) [],
        CLKREQ_L1SS_ENABLE OFFSET(21) NUMBITS(1) [],
        SERDES_IDDQ OFFSET(27) NUMBITS(1) [],
    ],

    // 0x4300 INTR2_CPU_BASE
    INTR2_CPU_STATUS [
        INTR_STATUS OFFSET(0) NUMBITS(32) [],
    ],
    // 0x4304 0x4300 + 0x4
    INTR2_CPU_SET [
        INTR_SET OFFSET(0) NUMBITS(32) [],
    ],
    // 0x4308 0x4300 + 0x8
    INTR2_CPU_CLR [
        INTR_CLR OFFSET(0) NUMBITS(32) []
    ],
    // 0x430c 0x4300 + 0x0c
    INTR2_CPU_MASK_STATUS [
        INTR_MASK_STATUS OFFSET(0) NUMBITS(32) []
    ],
    // 0x4310 0x4300 + 0x10
    INTR2_CPU_MASK_SET [
        INTR_MASK_SET OFFSET(0) NUMBITS(32) []
    ],
    // 0x4314 0x4500 + 0x14
    INTR2_CPU_MASK_CLR [
        INTR_MASK_CLR OFFSET(0) NUMBITS(32) []
    ],
    // 0x4500 MSI_INTR2_BASE
    MSI_INTR2_STATUS [
        INTR_STATUS OFFSET(0) NUMBITS(32) [],
    ],
    // 0x4504 0x4500 + 0x4
    MSI_INTR2_SET [
        INTR_SET OFFSET(0) NUMBITS(32) [],
    ],
    // 0x4508 0x4500 + 0x8
    MSI_INTR2_CLR [
        INTR_CLR OFFSET(0) NUMBITS(32) []
    ],
    // 0x450c 0x4500 + 0x0c
    MSI_INTR2_MASK_STATUS [
        INTR_MASK_STATUS OFFSET(0) NUMBITS(32) []
    ],
    // 0x4510 0x4500 + 0x10
    MSI_INTR2_MASK_SET [
        INTR_MASK_SET OFFSET(0) NUMBITS(32) []
    ],
    // 0x4514 0x4500 + 0x14
    MSI_INTR2_MASK_CLR [
        INTR_MASK_CLR OFFSET(0) NUMBITS(32) []
    ],
    // 0x8000
    // EXT_CFG_DATA
    // 0x9000
    // EXT_CFG_INDEX
    // 0x9210
    RGR1_SW_INIT_1 [
        PCIE_RGR1_SW_INTI_1_PERST OFFSET(0) NUMBITS(1) [],
        RGR1_SW_INTI_1_GENERIC OFFSET(1) NUMBITS(1) [],
    ],
];

register_structs! {
    /// Pl011 registers.
    BCM2711PCIeHostBridgeRegs {
        (0x00 => _rsvd1),
        (0x0188 => rc_cfg_vendor_vendor_specific_reg1),
        (0x043c => rc_cfg_priv1_id_val3: ReadWrite<u32,RC_CFG_PRIV1_ID_VAL3::Register>),
        (0x0440 => _rsvdd2),
        (0x1100 => rc_dl_mdio_addr),
        (0x1104 => rc_dl_mdio_wr_data),
        (0x1108 => rc_dl_mdio_rd_data),
        (0x4008 => misc_misc_ctrl: ReadWrite<u32, MISC_MISC_CTRL::Register>),
        (0x400C => misc_cpu_2_pcie_mem_win0_lo: ReadWrite<u32,MISC_CPU_2_PCIE_MEM_WIN0_LO::Register>),
        (0x4010 => misc_cpu_2_pcie_mem_win0_hi: ReadWrite<u32,MISC_CPU_2_PCIE_MEM_WIN0_HI::Register>),
        (0x4014 => _rsvd22),
        (0x4028 => _rsvd2),
        (0x402C => misc_rc_bar1_config_lo: ReadWrite<u32,MISC_RC_BAR1_CONFIG_LO::Register>),
        (0x4030 => _rsvdd),
        (0x4034 => misc_rc_bar2_config_lo: ReadWrite<u32,MISC_RC_BAR2_CONFIG_LO::Register>),
        (0x4038 => misc_rc_bar2_config_hi: ReadWrite<u32,MISC_RC_BAR2_CONFIG_HI::Register>),
        (0x403C => misc_rc_bar3_config_lo: ReadWrite<u32,MISC_RC_BAR3_CONFIG_LO::Register>),
        (0x4040 => _rsvddd),
        (0x4044 => misc_msi_bar_config_lo),
        (0x4048 => misc_msi_bar_config_hi),
        (0x404c => misc_msi_data_config	),
        (0x4060 => misc_eoi_ctrl),
        (0x4064 => misc_pcie_ctrl),
        (0x4068 => misc_pcie_status: ReadOnly<u32,MISC_PCIE_STATUS::Register>),
        (0x406C => misc_revision: ReadWrite<u32,MISC_REVISION::Register>),
        (0x4070 => misc_cpu_2_pcie_mem_win0_base_limit: ReadWrite<u32, MISC_CPU_2_PCIE_MEM_WIN0_BASE_LIMIT::Register>),
        (0x4074 => hole),
        (0x4080 => misc_cpu_2_pcie_mem_win0_base_hi: ReadWrite<u32,MISC_CPU_2_PCIE_MEM_WIN0_BASE_HI::Register>),
        (0x4084 => misc_cpu_2_pcie_mem_win0_limit_hi: ReadWrite<u32,MISC_CPU_2_PCIE_MEM_WIN0_LIMIT_HI::Register>),
        (0x4088 => hole2),
        (0x4204 => misc_hard_pcie_hard_debug: ReadWrite<u32,MISC_HARD_PCIE_HARD_DEBUG::Register>),
        (0x4208 => _rsvd3),
        /// cpu intr
        (0x4300 => intr2_cpu_status:        ReadWrite<u32,INTR2_CPU_STATUS::Register>),
        (0x4304 => intr2_cpu_set:           ReadWrite<u32,INTR2_CPU_SET::Register>),
        (0x4308 => intr2_cpu_clr:           ReadWrite<u32,INTR2_CPU_CLR::Register>),
        (0x430C => intr2_cpu_mask_status:   ReadWrite<u32,INTR2_CPU_MASK_STATUS::Register>),
        (0x4310 => intr2_cpu_mask_set:      ReadWrite<u32,INTR2_CPU_MASK_SET::Register>),
        (0x4314 => intr2_cpu_mask_clr:      ReadWrite<u32,INTR2_CPU_MASK_CLR::Register>),
        (0x4318 => hole3),
        /// msi intr
        (0x4500 => msi_intr2_status:        ReadWrite<u32,MSI_INTR2_STATUS::Register>),
        (0x4504 => msi_intr2_set:           ReadWrite<u32,MSI_INTR2_SET::Register>),
        (0x4508 => msi_intr2_clr:           ReadWrite<u32,MSI_INTR2_CLR::Register>),
        (0x450C => msi_intr2_mask_status:   ReadWrite<u32,MSI_INTR2_MASK_STATUS::Register>),
        (0x4510 => msi_intr2_mask_set:      ReadWrite<u32,MSI_INTR2_MASK_SET::Register>),
        (0x4514 => msi_intr2_mask_clr:      ReadWrite<u32,MSI_INTR2_MASK_CLR::Register>),
        (0x4518 => hole4),
        /// Interrupt Clear Register.
        (0x9210 => rgr1_sw_init: ReadWrite<u32,RGR1_SW_INIT_1::Register>),
        (0x9214 => _rsvd4),
        (0x9310 => @END),
    }
}

impl BCM2711PCIeHostBridgeRegs {
    // 设置桥接器软初始化标志
    fn bridge_sw_init_set(&self, bit: u32) {
        // 如果 bit 为 1，将 RGR1_SW_INTI_1_GENERIC 置位
        if bit == 1 {
            self.rgr1_sw_init
                .modify(RGR1_SW_INIT_1::RGR1_SW_INTI_1_GENERIC::SET);
        }
        // 如果 bit 为 0，清除 RGR1_SW_INTI_1_GENERIC 位
        if bit == 0 {
            self.rgr1_sw_init
                .modify(RGR1_SW_INIT_1::RGR1_SW_INTI_1_GENERIC::CLEAR);
        }
    }

    // 设置 PERST（复位）标志
    fn perst_set(&self, bit: u32) {
        // 如果 bit 为 1，将 PCIE_RGR1_SW_INTI_1_PERST 置位
        if bit == 1 {
            self.rgr1_sw_init
                .modify(RGR1_SW_INIT_1::PCIE_RGR1_SW_INTI_1_PERST::SET);
        }
        // 如果 bit 为 0，清除 PCIE_RGR1_SW_INTI_1_PERST 位
        if bit == 0 {
            self.rgr1_sw_init
                .modify(RGR1_SW_INIT_1::PCIE_RGR1_SW_INTI_1_PERST::CLEAR);
        }
    }
}

 pub fn setup(&self) {
    let regs = self.regs();

    // 断言桥复位
    // 确保 PCIe 控制器处于已知状态,实际上是将 PCIe 控制器的状态置于一个初始值
    regs.bridge_sw_init_set(1);
    log::debug!("assert bridge reset");

    // 断言基本复位
    // 将整个 PCIe 控制器或者相关模块复位到初始状态，以确保系统在初始化开始时处于一种可控制和已知状态
    regs.perst_set(1);
    log::debug!("assert fundamental reset");

    H::sleep(core::time::Duration::from_micros(2));

    // 解除桥复位
    //标志 PCIe 控制器已经完成了一系列的初始化步骤，并且认为 PCIe 控制器已经准备好正常工作
    regs.bridge_sw_init_set(0);
    log::debug!("deassert bridge reset");

    H::sleep(core::time::Duration::from_micros(2));

    // 启用 SerDes,确保 PCIe 控制器能够正常进行数据传输
    regs.misc_hard_pcie_hard_debug
        .modify(MISC_HARD_PCIE_HARD_DEBUG::SERDES_IDDQ::CLEAR);
    log::debug!("enable serdes");

    H::sleep(core::time::Duration::from_micros(2));

    // 获取硬件版本
    let hw_rev = regs.misc_revision.read(MISC_REVISION::MISC_REVISION) & 0xFFFF;
    log::debug!("hw_rev: {}", hw_rev);

    // 禁用和清除任何挂起的中断,保在 PCIe 控制器初始化的过程中，系统处于一个可控的状态
    regs.msi_intr2_clr.write(MSI_INTR2_CLR::INTR_CLR::SET);
    regs.msi_intr2_mask_set
        .write(MSI_INTR2_MASK_SET::INTR_MASK_SET::SET);
    log::debug!("disable and clear any pending interrupts");

    // 初始化设置 SCB_MAX_BURST_SIZE 0x0, CFG_READ_UR_MODE, SCB_ACCESS_EN
    regs.misc_misc_ctrl
        .modify(MISC_MISC_CTRL::SCB_ACCESS_EN::SET);
    regs.misc_misc_ctrl
        .modify(MISC_MISC_CTRL::CFG_READ_UR_MODE::SET);
    regs.misc_misc_ctrl
        .modify(MISC_MISC_CTRL::MAX_BURST_SIZE::CLEAR);

    // 设置入站内存视图
    regs.misc_rc_bar2_config_lo
        .write(MISC_RC_BAR2_CONFIG_LO::VALUE_LO::init_val);
    regs.misc_rc_bar2_config_hi
        .write(MISC_RC_BAR2_CONFIG_HI::VALUE_HI::init_val);

    regs.misc_misc_ctrl
        .modify(MISC_MISC_CTRL::SCB0_SIZE::init_val);

    // 禁用 PCIe->GISB 内存窗口和 PCIe->SCB 内存窗口
    regs.misc_rc_bar1_config_lo
        .modify(MISC_RC_BAR1_CONFIG_LO::MEM_WIN::CLEAR);
    regs.misc_rc_bar3_config_lo
        .modify(MISC_RC_BAR3_CONFIG_LO::MEM_WIN::CLEAR);

    // 设置 MSIs，清除中断，屏蔽中断
    // CPU::MMIOWrite32(pcieBase + MSI_BAR_CONFIG_LO, (MSI_TARGET_ADDR & 0xFFFFFFFFu) | 1);
    // CPU::MMIOWrite32(pcieBase + MSI_BAR_CONFIG_HI, MSI_TARGET_ADDR >> 32);
    // CPU::MMIOWrite32(pcieBase + MSI_DATA_CONFIG, hwRev >= HW_REV_33 ? 0xffe06540 : 0xFFF86540);
    // TODO: 在此注册 MSI 处理程序

  

    // 解除基本复位
    // 在确认初始化步骤完成后，将 PCIe 控制器从基本复位状态恢复到正常工作状态
    regs.perst_set(0);

    // 检查 status 值 
    for _ in 0..20 {
        let val = regs.misc_pcie_status.read(MISC_PCIE_STATUS::CHECK_BITS);
        log::trace!("val: {}", val);
        if val == 0x3 {
            break;
        }
        H::sleep(core::time::Duration::from_micros(5000));
    }

    // 检查 status 值
    {
        let val = regs.misc_pcie_status.read(MISC_PCIE_STATUS::CHECK_BITS);
        if val != 0x3 {
            panic!("PCIe link is down");
        }
    }

    // 检查控制器是否运行在根复杂模式
    {
        let val = regs.misc_pcie_status.read(MISC_PCIE_STATUS::RC_MODE);
        if val != 0x1 {
            panic!("PCIe controller is not running in root complex mode");
        }
    }

    log::debug!("PCIe link is ready");

    // 定义 PCIe 设备可以访问的一块内存区域，使 PCIe 设备可以读取或写入这个内存区域中的数据
    regs.misc_cpu_2_pcie_mem_win0_lo
        .write(MISC_CPU_2_PCIE_MEM_WIN0_LO::MEM_WIN0_LO::init_val);
    regs.misc_cpu_2_pcie_mem_win0_hi
        .write(MISC_CPU_2_PCIE_MEM_WIN0_HI::MEM_WIN0_HI::init_val);
    regs.misc_cpu_2_pcie_mem_win0_base_limit
        .write(MISC_CPU_2_PCIE_MEM_WIN0_BASE_LIMIT::MEM_WIN0_BASE_LIMIT::init_val);
    regs.misc_cpu_2_pcie_mem_win0_base_hi
        .write(MISC_CPU_2_PCIE_MEM_WIN0_BASE_HI::MEM_WIN0_BASE_HI::init_val);
    regs.misc_cpu_2_pcie_mem_win0_limit_hi
        .write(MISC_CPU_2_PCIE_MEM_WIN0_LIMIT_HI::MEM_WIN0_LIMIT_HI::init_val);

    // 设置正确的 Class ID
    regs.rc_cfg_priv1_id_val3
        .modify(RC_CFG_PRIV1_ID_VAL3::CLASS_ID::pcie_pcie_bridge);

   }
}

```



