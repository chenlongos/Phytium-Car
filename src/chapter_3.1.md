# 任务一：PCIe主桥初始化

## 何为PCIe

PCIe（Peripheral Component Interconnect Express），是一种高速串行计算机扩展总线标准，用于连接计算机内部的硬件设备。PCIe是PCI的后继者，旨在提供更高的带宽和性能。

关键特点：

1. **高速串行连接：** 与传统的PCI总线使用并行连接不同，PCIe采用高速串行连接。这使得数据能够以更高的速率进行传输，提供更大的带宽。

2. **多通道架构：** PCIe采用多通道（lanes）的结构，每个通道都能独立传输数据。每个通道的带宽是独立分配的，通常以“x1”、“x4”、“x8”和“x16”等倍数来表示。例如，PCIe x16插槽提供比PCIe x1插槽更大的带宽。

3. **点对点连接：** 每个PCIe设备都与主板上的PCIe插槽之间建立一个点对点的连接。这种连接方式与传统PCI总线的共享连接方式相比，更有效地支持高带宽需求。

4. **热插拔支持：** PCIe支持热插拔，允许用户在计算机运行时插入或拔出PCIe设备，而无需重新启动计算机。

5. **兼容性：** PCIe是一种通用的总线标准，广泛应用于各种设备，包括显卡、网卡、固态硬盘、扩展卡等。


## 初始化实现

参考代码：<https://github.com/apengaaa/arceos/tree/raspi4>

运行结果：

```shell
       d8888                            .d88888b.   .d8888b.
      d88888                           d88P" "Y88b d88P  Y88b
     d88P888                           888     888 Y88b.
    d88P 888 888d888  .d8888b  .d88b.  888     888  "Y888b.
   d88P  888 888P"   d88P"    d8P  Y8b 888     888     "Y88b.
  d88P   888 888     888      88888888 888     888       "888
 d8888888888 888     Y88b.    Y8b.     Y88b. .d88P Y88b  d88P
d88P     888 888      "Y8888P  "Y8888   "Y88888P"   "Y8888P"

arch = aarch64
platform = aarch64-raspi4
target = aarch64-unknown-none-softfloat
smp = 1
build_mode = release
log_level = debug

[  0.108847 0 axruntime:126] Logging is enabled.
[  0.114577 0 axruntime:127] Primary CPU 0 started, dtb = 0x0.
[  0.121522 0 axruntime:129] Found physcial memory regions:
[  0.128209 0 axruntime:131]   [PA:0x80000, PA:0x89000) .text (READ | EXECUTE | RESERVED)
[  0.137498 0 axruntime:131]   [PA:0x89000, PA:0x8c000) .rodata (READ | RESERVED)
[  0.146093 0 axruntime:131]   [PA:0x8c000, PA:0x90000) .data .tdata .tbss .percpu (READ | WRITE | RESERVED)
[  0.157033 0 axruntime:131]   [PA:0x90000, PA:0xd0000) boot stack (READ | WRITE | RESERVED)
[  0.166584 0 axruntime:131]   [PA:0xd0000, PA:0xd1000) .bss (READ | WRITE | RESERVED)
[  0.175613 0 axruntime:131]   [PA:0x0, PA:0x1000) spintable (READ | WRITE | RESERVED)
[  0.184643 0 axruntime:131]   [PA:0xd1000, PA:0xfc000000) free memory (READ | WRITE | FREE)
[  0.194193 0 axruntime:131]   [PA:0xfe201000, PA:0xfe202000) mmio (READ | WRITE | DEVICE | RESERVED)
[  0.204525 0 axruntime:131]   [PA:0xff841000, PA:0xff849000) mmio (READ | WRITE | DEVICE | RESERVED)
[  0.214857 0 axruntime:131]   [PA:0xfd500000, PA:0xfd509310) mmio (READ | WRITE | DEVICE | RESERVED)
[  0.225189 0 axruntime:149] Initialize platform devices...
[  0.231874 0 axruntime:185] Primary CPU 0 init OK.
Available commands:
  exit
  help
  uname
  ldr
  str
  uart
  test
arceos# [  0.245158 0 brcm_pcie::bcm2711:309] assert bridge reset
[  0.251670 0 brcm_pcie::bcm2711:313] assert fundamental reset
[  0.258618 0 brcm_pcie::bcm2711:319] deassert bridge reset
[  0.265304 0 brcm_pcie::bcm2711:326] enable serdes
[  0.271294 0 brcm_pcie::bcm2711:333] hw_rev772
[  0.276935 0 brcm_pcie::bcm2711:339] disable and clear any pending interrupts
[  0.340273 0 brcm_pcie::bcm2711:406] PCIe link is ready

arceos# ldr ffff0000fd508000
Value at address ffff0000fd508000: 0x34831106
```

## 代码分析

```rust
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

    ...
    
    RGR1_SW_INIT_1 [
        PCIE_RGR1_SW_INTI_1_PERST OFFSET(0) NUMBITS(1) [],
        RGR1_SW_INTI_1_GENERIC OFFSET(1) NUMBITS(1) [],
    ],

];

...

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
```
