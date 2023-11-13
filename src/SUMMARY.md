# Summary

- [学习指南](./chapter_0.md)
- [第一阶段：认识树莓派及 ArceOS 编译](./chapter_1.0.md)
  - [实验一：支持树莓派4b的 Qemu 环境搭建](./chapter_1.1.md)
  - [实验二：Qemu 模拟器启动 ArceOS，打印 Hello，world](./chapter_1.2.md)
  - [实验三：Qemu 模拟器启动 ArceOS，执行 shell 命令](./chapter_1.3.md)
  - [实验四：树莓派启动 ArceOS，打印 Hello,world](./chapter_1.4.md)
  - [实验五：树莓派启动 ArceOS，执行 shell 命令](./chapter_1.5.md)
- [第二阶段：Rust 编写树莓派串口驱动](./chapter_2.0.md)
  - [实验一：UART0 的启用](./chapter_2.1.md)
  - [实验二：通过 UART0 驱动小车](./chapter_2.2.md)
  - [实验三：通过 UART0 启用 UART5](./chapter_2.3.md)
  - [实验四：通过 UART0 启用 UART2/3/4](./chapter_2.4.md)
  - [实验五：通过 UART0 发送指令，由 UART2/3/4/5 驱动小车](./chapter_2.5.md)
- [第三阶段：Rust编写树莓派USB驱动](./chapter_3.0.md)
  - [任务一：PCIe总线初始化](./chapter_3.1.md)
  - [任务二：PCIe总线检测设备，分配内存空间](./chapter_3.2.md)
  - [任务三：xhci主机控制器初始化](./chapter_3.3.md)
  - [任务四：xhci检测设备，分配地址空间](./chapter_3.4.md)
  - [任务五：解析设备配置，加载对应驱动](./chapter_3.5.md)
  - [任务六：USB转串口的设备驱动实现](./chapter_3.6.md)
- [参考资料](./chapter_0.0.md)

  <!-- - [任务零：环境搭建 C语言内核模块的编译和测试](./chapter_3.1.md)
  <!-- - [任务一：R4L e10000 网卡驱动代码内核模块编译](./chapter_3.2.md)
  <!-- - [任务二：Linux 6.1 + R4L e10000网卡驱动 在 Qemu 中运行](./chapter_3.3.md)
  <!-- - [任务三：R4L virtio-net 网卡驱动代码内核模块编译](./chapter_3.4.md)
  <!-- - [任务四：Linux 6.1 + R4L virtio-net 网卡驱动 在 Qemu 中运行](./chapter_3.5.md)
  <!-- - [任务五：R4L + dwc 网卡驱动 在 Hw204 Linux 6.1 中运行](./chapter_3.6.md) -->
<!-- - [第四阶段：Rust LDD 网卡驱动规范设计（6.1-6.20）](./chapter_4.0.md) -->
  <!-- - [任务一：两套驱动代码分析对比，输出技术分析文档](./chapter_4.1.md) -->
  <!-- - [任务二：设计并提出 Rust LDD 网卡驱动规范和接口标准](./chapter_4.2.md) -->
<!-- - [第五阶段：基线版本1.0和技术架构2.0（6.20-7.1）](./chapter_5.0.md) -->
  <!-- - [任务一：Rust LDD 并入基线版本1.0的代码主分支中](./chapter_5.1.md) -->
  <!-- - [任务二：Rust LDD 写入技术架构2.0的设计文档和PPT中](./chapter_5.2.md) -->
<!-- - [第六阶段：技术架构2.0的拓展开发（7.1-9.1）](./chapter_5.3.md) -->
  <!-- - [任务一：支持树莓派ARM系列开发板（采购）](./chapter_5.4.md) -->
  <!-- - [任务二：支持平头哥RISC-V芯片开发板（厂家赞助）](./chapter_5.5.md) -->
  <!-- - [任务三：支持地平线J3/J5系列开发板（厂家赞助）](./chapter_5.6.md) -->
  <!-- - [任务四：支持黑芝麻C1200最新芯片开发板（需要争取）](./chapter_5.7.md) -->
