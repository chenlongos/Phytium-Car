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


### 具体寄存器配置

  

### 代码分析

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

    // 等待 [0xfd504068] 的位 4 和 5 被设置，每隔 5000 微秒检查一次
    for _ in 0..20 {
        let val = regs.misc_pcie_status.read(MISC_PCIE_STATUS::CHECK_BITS);
        log::trace!("val: {}", val);
        if val == 0x3 {
            break;
        }
        H::sleep(core::time::Duration::from_micros(5000));
    }

    // 检查链路是否正常
    {
        let val = regs.misc_pcie_status.read(MISC_PCIE_STATUS::CHECK_BITS);
        if val != 0x3 {
            panic!("PCIe link is down");
        }
    }

    // 检查控制器是否运行在根复杂模式。如果位 7 未设置，则发生错误
    {
        let val = regs.misc_pcie_status.read(MISC_PCIE_STATUS::RC_MODE);
        if val != 0x1 {
            panic!("PCIe controller is not running in root complex mode");
        }
    }

    log::debug!("PCIe link is ready");

    // 配置出站内存
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
  

  

  





