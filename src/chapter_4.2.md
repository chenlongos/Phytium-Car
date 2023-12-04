# 任务二、三、四


## 具体实现：

参考代码：https://github.com/dbydd/arceos_experiment/tree/task3_usb

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

## 代码分析

* 位置：modules/axdriver/src/lib.rs

```rust
...

//启用 xhci 特性，那么将导出 AxXHciDevice 结构体
#[cfg(feature = "xhci")]
pub use self::structs::AxXHciDevice;

/// A structure that contains all device drivers, organized by their category.
#[derive(Default)]
pub struct AllDevices {
    
    ...
    
    #[cfg(feature = "xhci")]
    pub xhci: AxDeviceContainer<AxXHciDevice>,
}

impl AllDevices {
    /// Returns the device model used, either `dyn` or `static`.
    ///
    /// See the [crate-level documentation](crate) for more details.
    pub const fn device_model() -> &'static str {
        if cfg!(feature = "dyn") {
            "dyn"
        } else {
            "static"
        }
    }

    /// Probes all supported devices.
    fn probe(&mut self) {
        for_each_drivers!(type Driver, {
            if let Some(dev) = Driver::probe_global() {
                info!(
                    "registered a new {:?} device: {:?}",
                    dev.device_type(),
                    dev.device_name(),
                );
                self.add_device(dev);
            }
        });

        self.probe_bus_devices();
    }

    /// Adds one device into the corresponding container, according to its device category.
    #[allow(dead_code)]
    fn add_device(&mut self, dev: AxDeviceEnum) {
        match dev {
            
            ...
            //如果检测到一个 XHCI 类型的设备，就将它添加到 xhci 字段指定的容器中
            #[cfg(feature = "xhci")]
            AxDeviceEnum::XHCI(dev) => self.xhci.push(dev),
        }
    }
}

/// Probes and initializes all device drivers, returns the [`AllDevices`] struct.
pub fn init_drivers() -> AllDevices {
    info!("Initialize device drivers...");
    info!("  device model: {}", AllDevices::device_model());

    let mut all_devs = AllDevices::default();
    all_devs.probe();

    ...
    
    #[cfg(feature = "xhci")]
    {
        debug!("number of xhci devices: {}", all_devs.xhci.len());
        for (i, dev) in all_devs.xhci.iter().enumerate() {
            assert_eq!(dev.device_type(), DeviceType::XHCI);
            debug!("  xhci device {}: {:?}", i, dev.device_name());
        }
    }

    all_devs
}
```

* 位置：crates/driver_xhci/src/libs.rs:

```rust
...

pub struct XhciController {
    pub controller: Option<Registers<MemoryMapper>>,
}

// VL805 xhci 控制器的供应商和设备 ID
pub const VL805_VENDOR_ID: u16 = 0x1106;
pub const VL805_DEVICE_ID: u16 = 0x3483;

pub mod register_operations_init_xhci;

/// The information of the graphics device.
#[derive(Debug, Clone, Copy)]
pub struct XhciInfo {}

// xhci 内存映射器的结构体
#[derive(Clone)]
struct MemoryMapper;
impl Mapper for MemoryMapper {
    // 映射物理内存范围
    unsafe fn map(&mut self, phys_base: usize, bytes: usize) -> NonZeroUsize {
        info!("mapping:{:x}", phys_base);

        return NonZeroUsize::new_unchecked(phys_base);
    }
    // 解除映射虚拟内存范围
    fn unmap(&mut self, virt_base: usize, bytes: usize) {}
}

impl XhciController {
    // 初始化 xhci 控制器实例的方法
    pub fn init(add: usize) -> XhciController {

        info!("received address:{:x}", add);
        XhciController {
            controller: Some(unsafe { xhci::Registers::new(add, MemoryMapper {}) }),
        }
    }
}
```

* 位置：crates/driver_xhci/src/register_operations_init_xhci.rs:

```rust
// 寄存器偏移量
const PCI_CLASS_REVISION: u64 = 0x08;
...
const XHCI_PCIE_FUNC: usize = 0;

// 一个延时函数
fn delay(seconds: u64) {
    for i in 1..seconds + 1 {
        fn fibonacci_recursive(n: u64) -> u64 {
            if n == 0 {
                return 0;
            }
            if n == 1 {
                return 1;
            }
            return fibonacci_recursive(n - 1) + fibonacci_recursive(n - 2);
        }
        fibonacci_recursive(36 + (i % 2));
    }
}

/// 启用 xHCI 控制器
/// 返回 xHCI 的 MMIO 空间的地址
pub fn enable_xhci(bus: u8, dfn: u8, address: usize) -> usize {
    info!("xhci energizing!");
    enable_bridge(bus, dfn, address);

    let a = get_tag();
    if a {
        panic!("state sync fail!");
    }

    // 检查版本
    let us_version: u16 =
        unsafe { *((MAPPED_XHCI_BASE + XHCI_REG_CAP_HCIVERSION) as *const u16) };
    if us_version != 0x100 {
        info!("Unsupported xHCI version {:x}", us_version);
    }

    enable_device(address);

    ARM_XHCI_BASE
}

// 启用 xHCI 设备
fn enable_device(address: usize) {
    info!("enable xhci!");
    const SLOT: usize = XHCI_PCIE_SLOT;
    const FUNC: usize = XHCI_PCIE_FUNC;
    const CLASS_CODE: u64 = 0xC0330;

    let conf: u64 = pcie_map_conf(
        1,
        ((SLOT & 0x1f) << 3 | (FUNC & 0x07)) as u8,
        0,
        address,
    );
    if conf == 0 {
        panic!("enable failed 1");
    } else {
        info!("conf = {:x}", conf);
    }

    info!("check passed");

    unsafe {
        *((conf + PCI_CACHE_LINE_SIZE) as *mut u8) = 64 / 4;

        *((conf + 0x10) as *mut u32) = (MEM_PCIE_RANGE_PCIE_START) as u32 | 0x04;
        *((conf + 0x14) as *mut u32) = (MEM_PCIE_RANGE_PCIE_START >> 32) as u32;
        *((conf + 0x04) as *mut u16) = 0x2 | 0x4 | 0x40 | 0x100;
    }
}

const PCI_HEADER_TYPE_BRIDGE: usize = 1;

// 启用桥接器
fn enable_bridge(bus: u8, dfn: u8, address: usize) {
    info!("enable bridge!");
    //获取 PCIe 配置地址
    let conf: u64 = pcie_map_conf(bus, dfn, 0, address);
    if conf == 0 {
        panic!("enable failed 1");
    } else {
        info!("conf = {:x}", conf);
    }

    unsafe {
        loop {
            let val1 = *((conf + PCI_CLASS_REVISION) as *const u64);
            let val2 = *((conf + PCI_HEADER_TYPE) as *const u8);
            let cond1 = val1 >> 8 != 0x060400;
            let cond2 = val2 as usize != PCI_HEADER_TYPE_BRIDGE;
            if !(cond1 || cond2) {
                break;
            } else {
                info!(
                    "enable waiting:{}:{:x},{}:{:x}",
                    cond1, val1, cond2, val2
                );
                break; // todo: remove it
            }
        }

        info!("check passed");

        *((conf + PCI_CACHE_LINE_SIZE) as *mut u8) = 64 / 4;

        *((conf + PCI_SECONDARY_BUS) as *mut u8) = 1;
        *((conf + PCI_SUBORDINATE_BUS) as *mut u8) = 1;

        *((conf + PCI_MEMORY_BASE) as *mut u16) = (MEM_PCIE_RANGE_PCIE_START >> 16) as u16;
        *((conf + PCI_MEMORY_LIMIT) as *mut u16) = (MEM_PCIE_RANGE_PCIE_START >> 16) as u16;

        *((conf + PCI_BRIDGE_CONTROL) as *mut u8) = PCI_BRIDGE_CTL_PARITY;

        assert_eq!(
            *((conf + BRCM_PCIE_CAP_REGS + PCI_CAP_LIST_ID) as *const u8),
            PCI_CAP_ID_EXP
        );

        *((conf + BRCM_PCIE_CAP_REGS + PCI_EXP_RTCTL) as *mut u8) =
            PCI_EXP_RTCTL_CRSSVE;

        *((conf + 0x04) as *mut u16) = 0x2 | 0x4 | 0x40 | 0x100;

        info!("done");
    }
}

// PCIe 配置空间地址映射
fn pcie_map_conf(busnr: u8, devfn: u8, whereis: usize, address: usize) -> u64 {
    // 如果槽位为0，则访问 RC（Root Complex）的寄存器
    if busnr == 0 {
        info!("devfn:{:x}", devfn);
        return if (((devfn) >> 3) & 0x1f) != 0 {
            info!("case 1");
            0
        } else {
            info!("case 2:address={:x}", address);
            (address + whereis) as u64
        };
    }
    // 对于设备，写入配置空间索引寄存器
    let idx: i32 = cfg_index(busnr, devfn, 0);
    unsafe {
        *((address + 36864) as *mut i32) = idx;
    }
    return (address + 32768 + whereis).try_into().unwrap();
}

// 获取配置空间索引
#[allow(arithmetic_overflow)]
fn cfg_index(busnr: u8, devfn: u8, reg: u8) -> i32 {
    return (((((devfn) >> 3) & 0x1f & 0x1f) << 15)
        | ((devfn & 0x07 & 0x07) << 12)
        | (busnr << 20)
        | (reg & !3))
        .into();
}


// XHCI 通知重置的标签
const TAG_XHCI_NOTIFY_RESET: usize = 0x00030058;

// 内存一致性区域的起始地址计算
const MEM_COHERENT_REGION: usize = 0x8000
    + 2 * 0x100000
    + 0x20000
    + 0x20000 * (4 - 1)
    + 0x8000
    + 0x8000 * (4 - 1)
    + 0x8000
    + 0x8000 * (4 - 1)
    + 0x8000
    + 0x8000 * (4 - 1)
    + 0x4000
    + 3 * 0x100000
    & !(2 * 0x100000 - 1);

// 获取标签信息的函数
fn get_tag() -> bool {
    info!("get_tag");
    let mut property_tag = PropertyTag {
        n_tag_id: RESET_COMMAND,
        n_value_buf_size: 32,
        n_value_length: 4 & (!(1 << 32)),
    };
    // 调用 get_tags 函数，返回其取反结果
    return !get_tags(&mut property_tag);
}

// 常量定义
const RESET_COMMAND: u32 = 1 << 20 | 0 << 15 | 0 << 12;
...
const CODE_RESPONSE_FAILURE: usize = 0x80000001;

// TPropertyBuffer 结构体定义
struct TPropertyBuffer {
    n_buffer_size: u32, 
    n_code: u32,
    tags: PropertyTag,
}

// PropertyTag 结构体定义
#[derive(Clone, Copy)]
struct PropertyTag {
    n_tag_id: u32,
    n_value_buf_size: u32, 
    n_value_length: u32,   
}

// PropertyTag 实现 Display trait 方便打印
impl Display for PropertyTag {
    fn fmt(&self, f: &mut fmt::Formatter<'_>) -> fmt::Result {
        write!(
            f,
            "[tag_id:{}, buf_size:{}, val_len:{}]",
            self.n_tag_id, self.n_value_buf_size, self.n_value_length
        )
    }
}

// 获取标签信息
fn get_tags(prop_tag: &mut PropertyTag) -> bool {
    info!("get_tags, tag:{}", prop_tag);
    let buffer_size: usize = 72 + 128 + 32;
    let p_buffer_address = phys_to_virt(MEM_COHERENT_REGION.into()).as_usize();
    let p_buffer = p_buffer_address as *mut TPropertyBuffer;
    info!("p_buffer:{:x}", p_buffer as usize);

    unsafe {
        (*p_buffer).n_buffer_size = buffer_size as u32;
        (*p_buffer).n_code = CODE_REQUEST as u32;
        (*p_buffer).tags = *prop_tag;

        let p_end_tag = (p_buffer as usize + 64 + 128) as *mut u32;

        *p_end_tag = 0x00000000;

        // barr
        barrier::dsb(SY);

        let virt_to_phys = virt_to_phys(p_buffer_address.into());
        let n_buffer_address = virt_to_phys.as_usize() & !0xC0000000 | 0xC0000000;
        info!("n_buffer_address:{:x}", n_buffer_address);

        let write_read = write_read(phys_to_virt(n_buffer_address.into()).as_usize());
        if write_read != n_buffer_address as u32 {
           
            info!("cond match:{:x}", write_read);
            return false;
        }

        barrier::dmb(SY);

        let mut n_code = (*p_buffer).n_code;

        *prop_tag = (*p_buffer).tags;
        return true;
    }
}

// MAILBOX 相关常量
const MAILBOX0_STATUS: usize = 0xFE000000 + 0xB880 + 0x18;
const MAILBOX0_READ: usize = 0xFE000000 + 0xB880 + 0x00;
const MAILBOX_STATUS_EMPTY: u32 = 0x40000000;
const MAILBOX_STATUS_FULL: u32 = 0x80000000;
const MAILBOX1_STATUS: usize = 0xFE000000 + 0xB880 + 0x38;
const MAILBOX1_WRITE: usize = 0xFE000000 + 0xB880 + 0x20;

// 写入和读取数据
fn write_read(n_data: usize) -> u32 {
    info!("write read:{}", n_data);

    unsafe {
        while !((*(phys_to_virt(MAILBOX0_STATUS.into()).as_usize() as *const u32)
            & MAILBOX_STATUS_EMPTY)
            != 0)
        {
            let usize = *(phys_to_virt(MAILBOX0_READ.into()).as_usize() as *const u32);
            // CTimer::SimpleMsDelay(20);
            info!("waiting...{:x}", usize);
            delay(1)
        }

        while (*(phys_to_virt(MAILBOX1_STATUS.into()).as_usize() as *const u32)
            & MAILBOX_STATUS_FULL)
            != 0
        {
            
            info!(
                "waiting...:{:x}",
                *(phys_to_virt(MAILBOX1_STATUS.into()).as_usize() as *const u32)
            );
        }

        assert!((n_data & 0xF) == 0);
       
        *(phys_to_virt(MAILBOX1_WRITE.into()).as_usize() as *mut u32) = 8 | n_data as u32; 
                                                                                 
        let mut n_result: u32 = 0;

        'a: loop {
            while *(phys_to_virt(MAILBOX0_STATUS.into()).as_usize() as *const u32)
                & MAILBOX_STATUS_EMPTY
                != 0
            {
                
                info!(
                    "waiting...{:x}",
                    *(phys_to_virt(MAILBOX0_STATUS.into()).as_usize() as *const u32)
                );
                delay(1);
            }

            n_result = *(phys_to_virt(MAILBOX0_READ.into()).as_usize() as *const u32);

            if !((n_result & 0xF) != 8) {
                info!("break! n_result & 0xf:{:x}", n_result & 0xf);
                break 'a;
            } else {
                info!("n_result & 0xf :{:x}", n_result & 0xf);
                delay(1);
            } 
        }

        info!("n_result:{:x}, and!0xf = {:x}", n_result, n_result & !0xF);
        return n_result & !0xF;
    }
}
```



