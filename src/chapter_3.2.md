# 任务二：PCIe检测设备，分配内存空间

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

[  0.111216 0 axruntime:126] Logging is enabled.
[  0.116945 0 axruntime:127] Primary CPU 0 started, dtb = 0x0.
[  0.123891 0 axruntime:129] Found physcial memory regions:
[  0.130578 0 axruntime:131]   [PA:0x80000, PA:0x91000) .text (READ | EXECUTE | RESERVED)
[  0.139866 0 axruntime:131]   [PA:0x91000, PA:0x96000) .rodata (READ | RESERVED)
[  0.148462 0 axruntime:131]   [PA:0x96000, PA:0x9a000) .data .tdata .tbss .percpu (READ | WRITE | RESERVED)
[  0.159402 0 axruntime:131]   [PA:0x9a000, PA:0xda000) boot stack (READ | WRITE | RESERVED)
[  0.168952 0 axruntime:131]   [PA:0xda000, PA:0xff000) .bss (READ | WRITE | RESERVED)
[  0.177982 0 axruntime:131]   [PA:0x0, PA:0x1000) spintable (READ | WRITE | RESERVED)
[  0.187012 0 axruntime:131]   [PA:0xff000, PA:0xfc000000) free memory (READ | WRITE | FREE)
[  0.196562 0 axruntime:131]   [PA:0xfe201000, PA:0xfe202000) mmio (READ | WRITE | DEVICE | RESERVED)
[  0.206894 0 axruntime:131]   [PA:0xff841000, PA:0xff849000) mmio (READ | WRITE | DEVICE | RESERVED)
[  0.217226 0 axruntime:131]   [PA:0xfd500000, PA:0xfd509310) mmio (READ | WRITE | DEVICE | RESERVED)
[  0.227558 0 axruntime:131]   [PA:0xf8000000, PA:0xfc000000) mmio (READ | WRITE | DEVICE | RESERVED)
[  0.237890 0 axruntime:149] Initialize platform devices...
[  0.244575 0 axruntime:185] Primary CPU 0 init OK.
[  0.250567 0 brcm_pcie::bcm2711:309] assert bridge reset
[  0.257078 0 brcm_pcie::bcm2711:313] assert fundamental reset
[  0.264026 0 brcm_pcie::bcm2711:319] deassert bridge reset
[  0.270711 0 brcm_pcie::bcm2711:326] enable serdes
[  0.276702 0 brcm_pcie::bcm2711:333] hw_rev772
[  0.282343 0 brcm_pcie::bcm2711:339] disable and clear any pending interrupts
[  1.290781 0 brcm_pcie::bcm2711:408] PCIe link is ready
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
arceos# tud
[  3.236644 0 brcm_pcie::bcm2711:309] assert bridge reset
[  3.240455 0 brcm_pcie::bcm2711:313] assert fundamental reset
[  3.247403 0 brcm_pcie::bcm2711:319] deassert bridge reset
[  3.254088 0 brcm_pcie::bcm2711:326] enable serdes
[  3.260079 0 brcm_pcie::bcm2711:333] hw_rev772
[  3.265720 0 brcm_pcie::bcm2711:339] disable and clear any pending interrupts
[  4.274160 0 brcm_pcie::bcm2711:408] PCIe link is ready
[  4.277714 0 axdriver:158] Initialize device drivers...
[  4.284137 0 axdriver:159]   device model: static
[  4.290041 0 axdriver::bus::pci:144] base_vaddr:ffff0000fd500000
[  4.297248 0 axdriver::bus::pci:155] iter 0
[  4.302666 0 axdriver::bus::pci:160] PCI 00:01.0: 1106:3483 (class 0c.03, rev 01) Standard
[  4.312094 0 axdriver::bus::pci:162] in!
[  4.317250 0 axdriver::bus::pci:37] type 64
[  4.322632 0 axdriver::bus::pci:61]   BAR 0: MEM [0x600000000, 0x600001000) 64bit
[  4.331282 0 axdriver::bus::pci:79] two ents
[  4.336805 0 axdriver::drivers:168] vl805 found! at DeviceFunction { bus: 0, device: 1, function: 0 }
[  4.347203 0 axdriver::drivers:171] Memory space at 0x600000000, size 4096, type Width64, prefetchable false
[  4.358227 0 axdriver::drivers:181] status:10
[  4.363754 0 axdriver::drivers:182] command:7
[  4.369311 0 axdriver::bus::pci:177] registered a new XHCI device at 00:01.0: "xhci-controller"
[  4.379209 0 axdriver:166] number of NICs: 0
[  4.384679 0 axdriver:190] number of xhci devices: 1
[  4.390843 0 axdriver:193]   xhci device 0: "xhci-controller"
arceos#
```
