# 任务二、三、四


## 具体实现：

参考代码：<https://github.com/dbydd/arceos_experiment/tree/task3_usb>

这里发现了xhci主机控制器的设备号：

```shell
arceos# ldr ffff0000fd508000
Value at address ffff0000fd508000: 0x34831106
```


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

[  0.321902 0 axruntime:126] Logging is enabled.
[  0.327632 0 axruntime:127] Primary CPU 0 started, dtb = 0x0.
[  0.334577 0 axruntime:129] Found physcial memory regions:
[  0.341263 0 axhal::mem:75] mem_regions
[  0.346300 0 axruntime:131]   [PA:0x80000, PA:0x96000) .text (READ | EXECUTE | RESERVED)
[  0.355589 0 axruntime:131]   [PA:0x96000, PA:0x9c000) .rodata (READ | RESERVED)
[  0.364184 0 axruntime:131]   [PA:0x9c000, PA:0xa0000) .data .tdata .tbss .percpu (READ | WRITE | RESERVED)
[  0.375124 0 axruntime:131]   [PA:0xa0000, PA:0xe0000) boot stack (READ | WRITE | RESERVED)
[  0.384675 0 axruntime:131]   [PA:0xe0000, PA:0x105000) .bss (READ | WRITE | RESERVED)
[  0.393791 0 axruntime:131]   [PA:0x0, PA:0x1000) spintable (READ | WRITE | RESERVED)
[  0.402821 0 axruntime:131]   [PA:0x105000, PA:0xfc000000) free memory (READ | WRITE | FREE)
[  0.412458 0 axruntime:131]   [PA:0xfe201000, PA:0xfe202000) mmio (READ | WRITE | DEVICE | RESERVED)
[  0.422790 0 axruntime:131]   [PA:0xff841000, PA:0xff849000) mmio (READ | WRITE | DEVICE | RESERVED)
[  0.433122 0 axruntime:131]   [PA:0xfe000000, PA:0xfe00c000) mmio (READ | WRITE | DEVICE | RESERVED)
[  0.443454 0 axruntime:131]   [PA:0xfd500000, PA:0xfd50a000) mmio (READ | WRITE | DEVICE | RESERVED)
[  0.453786 0 axruntime:131]   [PA:0x600000000, PA:0x600001000) mmio (READ | WRITE | DEVICE | RESERVED)
[  0.464291 0 axruntime:207] Initialize global memory allocator...
[  0.471584 0 axruntime:208]   use TLSF allocator.
[  0.477488 0 axhal::mem:75] mem_regions
[  0.482525 0 axruntime:214] allocating 105000
[  0.488081 0 axhal::mem:75] mem_regions
[  0.493118 0 axruntime:221] allocating 105000
[  0.498673 0 axalloc:213] initialize global allocator at: [0xffff000000105000, 0xffff0000fc000000)
[  0.509863 0 axhal::mem:75] mem_regions
[  0.513868 0 axruntime:145] Initialize kernel page table...
[  0.520644 0 axhal::mem:75] mem_regions
[  0.525676 0 axruntime:246] allocating 80000
[  0.531157 0 axruntime:246] allocating 96000
[  0.536615 0 axruntime:246] allocating 9c000
[  0.542085 0 axruntime:246] allocating a0000
[  0.547558 0 axruntime:246] allocating e0000
[  0.553027 0 axruntime:246] allocating 0
[  0.558147 0 axruntime:246] allocating 105000
[  0.563767 0 axruntime:246] allocating fe201000
[  0.569436 0 axruntime:246] allocating ff841000
[  0.575167 0 axruntime:246] allocating fe000000
[  0.580897 0 axruntime:246] allocating fd500000
[  0.586629 0 axruntime:246] allocating 600000000
[  0.592448 0 axruntime:149] Initialize platform devices...
[  0.599127 0 axhal::platform::aarch64_raspi:57] testtesttest
[  0.605986 0 axruntime:185] Primary CPU 0 init OK.
[  0.611978 0 brcm_pcie::bcm2711:309] assert bridge reset
[  0.618489 0 brcm_pcie::bcm2711:313] assert fundamental reset
[  0.625437 0 brcm_pcie::bcm2711:319] deassert bridge reset
[  0.632122 0 brcm_pcie::bcm2711:326] enable serdes
[  0.638113 0 brcm_pcie::bcm2711:333] hw_rev772
[  0.643754 0 brcm_pcie::bcm2711:339] disable and clear any pending interrupts
[  1.652193 0 brcm_pcie::bcm2711:408] PCIe link is ready
Available commands:
  exit
  help
  uname
  ldr
  str
  uart
  test
  move
  tud
  test_flag
arceos# tud
[  5.791442 0 brcm_pcie::bcm2711:309] assert bridge reset
[  5.795253 0 brcm_pcie::bcm2711:313] assert fundamental reset
[  5.802201 0 brcm_pcie::bcm2711:319] deassert bridge reset
[  5.808887 0 brcm_pcie::bcm2711:326] enable serdes
[  5.814878 0 brcm_pcie::bcm2711:333] hw_rev772
[  5.820519 0 brcm_pcie::bcm2711:339] disable and clear any pending interrupts
[  6.828958 0 brcm_pcie::bcm2711:408] PCIe link is ready
[  6.832512 0 axdriver:158] Initialize device drivers...
[  6.838935 0 axdriver:159]   device model: static
[  6.844839 0 axdriver::bus::pci:147] base_vaddr:ffff0000fd500000
[  6.852082 0 axdriver::bus::pci:164] PCI 00:01.0: 1106:3483 (class 0c.03, rev 01) Standard
[  6.861547 0 axdriver::bus::pci:64]   BAR 0: MEM [0x600000000, 0x600001000) 64bit
[  6.870191 0 axdriver::bus::pci:82] two ents
[  6.875713 0 axdriver::drivers:175] vl805 found! at DeviceFunction { bus: 0, device: 1, function: 0 }
[  6.886112 0 axdriver::drivers:182] enabling!
[  6.891637 0 driver_xhci::register_operations_init_xhci:48] xhci energizing!
[  6.899885 0 driver_xhci::register_operations_init_xhci:104] enable bridge!
[  6.908047 0 driver_xhci::register_operations_init_xhci:154] devfn:0
[  6.915600 0 driver_xhci::register_operations_init_xhci:159] case 2:address=ffff0000fd500000
[  6.925238 0 driver_xhci::register_operations_init_xhci:110] conf = ffff0000fd500000
[  6.934180 0 driver_xhci::register_operations_init_xhci:123] enable waiting:true:604001006040010,false:1
[  6.944859 0 driver_xhci::register_operations_init_xhci:127] check passed
[  6.952848 0 driver_xhci::register_operations_init_xhci:147] done
[  6.960140 0 driver_xhci::register_operations_init_xhci:202] get_tag
[  6.967694 0 driver_xhci::register_operations_init_xhci:243] get_tags,tag:[tag_id:1048576,buf_size:32,val_len:4]
[  6.979068 0 driver_xhci::register_operations_init_xhci:247] p_buffer:ffff000000400000
[  6.988184 0 driver_xhci::register_operations_init_xhci:263] n_buffer_address:c0400000
[  6.997300 0 driver_xhci::register_operations_init_xhci:315] wrte read:18446462601958260736
[  7.006852 0 driver_xhci::register_operations_init_xhci:357] waiting...1
[  7.731114 0 driver_xhci::register_operations_init_xhci:367] break! n_result & 0xf:8
[  7.737184 0 driver_xhci::register_operations_init_xhci:379] n_result:c0400008,and!0xf = c0400000
[  7.747256 0 driver_xhci::register_operations_init_xhci:58] Unsupported xHCI version ffff
[  7.756633 0 driver_xhci::register_operations_init_xhci:66] enable xhci!
[  7.764534 0 driver_xhci::register_operations_init_xhci:75] conf = ffff0000fd508000
[  7.773390 0 driver_xhci::register_operations_init_xhci:91] check passed
[  7.781322 0 driver_xhci:74] received address:ffff000600000000
[  7.788323 0 driver_xhci:45] mapping:ffff000600000000
[  7.794574 0 driver_xhci:45] mapping:ffff000600000002
[  7.800826 0 driver_xhci:45] mapping:ffff000600000004
[  7.807077 0 driver_xhci:45] mapping:ffff000600000008
[  7.813328 0 driver_xhci:45] mapping:ffff00060000000c
[  7.819579 0 driver_xhci:45] mapping:ffff000600000010
[  7.825831 0 driver_xhci:45] mapping:ffff000600000014
[  7.832082 0 driver_xhci:45] mapping:ffff000600000018
[  7.838333 0 driver_xhci:45] mapping:ffff00060000001c
[  7.844584 0 driver_xhci:45] mapping:ffff000600000020
[  7.850865 0 driver_xhci:45] mapping:ffff000600000100
[  7.857116 0 driver_xhci:45] mapping:ffff000600000020
[  7.863338 0 driver_xhci:45] mapping:ffff000600000024
[  7.869589 0 driver_xhci:45] mapping:ffff000600000028
[  7.875841 0 driver_xhci:45] mapping:ffff000600000034
[  7.882092 0 driver_xhci:45] mapping:ffff000600000038
[  7.888343 0 driver_xhci:45] mapping:ffff000600000050
[  7.894594 0 driver_xhci:45] mapping:ffff000600000058
[  7.900875 0 driver_xhci:45] mapping:ffff000600000420
[  7.907126 0 driver_xhci:45] mapping:ffff000600000200
[  7.913378 0 axdriver::bus::pci:181] registered a new XHCI device at 00:01.0: "xhci-controller"
[  7.923246 0 axdriver:166] number of NICs: 0
[  7.928716 0 axdriver:190] number of xhci devices: 1
[  7.934880 0 axdriver:193]   xhci device 0: "xhci-controller"
arceos# test_flag
true
```




