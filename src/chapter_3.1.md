# 任务一：PCIe总线初始化

## 何为PCIe
PCIe（Peripheral Component Interconnect Express），是一种高速串行计算机扩展总线标准，用于连接计算机内部的硬件设备。PCIe是PCI的后继者，旨在提供更高的带宽和性能。

关键特点：

1. **高速串行连接：** 与传统的PCI总线使用并行连接不同，PCIe采用高速串行连接。这使得数据能够以更高的速率进行传输，提供更大的带宽。

2. **多通道架构：** PCIe采用多通道（lanes）的结构，每个通道都能独立传输数据。每个通道的带宽是独立分配的，通常以“x1”、“x4”、“x8”和“x16”等倍数来表示。例如，PCIe x16插槽提供比PCIe x1插槽更大的带宽。

3. **点对点连接：** 每个PCIe设备都与主板上的PCIe插槽之间建立一个点对点的连接。这种连接方式与传统PCI总线的共享连接方式相比，更有效地支持高带宽需求。

4. **热插拔支持：** PCIe支持热插拔，允许用户在计算机运行时插入或拔出PCIe设备，而无需重新启动计算机。

5. **兼容性：** PCIe是一种通用的总线标准，广泛应用于各种设备，包括显卡、网卡、固态硬盘、扩展卡等。












参考代码：<https://github.com/Axsl666/arceos>

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
