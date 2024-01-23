# 何为树莓派

## 树莓派

关于树莓派方面可查看：<https://chenlongos.com/raspi4-with-arceos-doc/> 

![image-20231225112317456](https://github.com/apengaaa/usb-doc/blob/master/src/assert/1.1-1.png)

[测试环境](https://github.com/arceos-usb/arceos_experiment/tree/usb-next/apps/boards/raspi4-usb)

1. **架构：** BCM2711 使用 ARM Cortex-A72 架构的四核心处理器。Cortex-A72 是 ARM 的 64位处理器架构，提供高性能的计算能力。

2. **处理器核心：** 四个 Cortex-A72 处理器核心，时钟频率通常为 1.5 GHz。

3. **GPU（图形处理单元）：** 集成 VideoCore VI GPU，支持OpenGL ES 3.0 和 Vulkan 图形API。这使得树莓派 4 具备适当的图形性能，适用于各种图形应用和小型游戏。

4. **内存：** 树莓派 4 使用 LPDDR4 SDRAM，内存容量可达 2GB、4GB 或 8GB，具体取决于型号。

5. **连接性：** 支持多种连接选项，包括 Gigabit Ethernet、802.11ac Wi-Fi 和蓝牙 5.0。此外，树莓派 4 还配备了多个 USB 3.0 和 USB 2.0 接口，HDMI 视频输出，音频输出，摄像头接口等。

6. **储存：** 提供 microSD 卡槽用于操作系统和数据存储。

7. **GPIO（通用输入输出）：** 树莓派 4 集成了扩展用的 GPIO 接口，允许用户连接和控制各种外部设备和传感器。

8. **视频输出：** 树莓派 4 支持显示器输出，通过 HDMI接口。

9. **电源：** 通过 USB-C 接口供电。

10. **操作系统：** 支持多种操作系统，包括基于 Linux 的 Raspberry Pi OS、Ubuntu 等。



## 树莓派各类接口

![image-20231225113108700](https://github.com/apengaaa/usb-doc/blob/master/src/assert/1.1-2.png)

从图中可以看到，树莓派的4个USB接口分别为2个USB2.0和2个USB3.0接口，它们由VL805这颗芯片控制，其又连接在树莓派的PCIe接口上。

因此，在树莓派上进行USB驱动开发，需要进行以下三方面内容：

1. PCIe总线
2. USB总线
3. 设备驱动



