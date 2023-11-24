# 第三阶段：Rust编写树莓派USB驱动

经过第二阶段的了解、学习，可以发现目前如果想要用树莓派驱动小车需要对STM32板子进行飞线处理，而这需要进行焊接，非常的不方便，而且树莓派本身含有许多的USB接口，因此，考虑编写USB转串口驱动。

背景资料：<https://github.com/orgs/chenlongos/discussions/14>

参考资料：
1. <https://blog.csdn.net/fudan_abc>
2. [VL805相关手册](https://github.com/chenlongos/raspi4-with-arceos-doc/blob/master/src/assert/DS_VLI_VL805_093.pdf)
3. [xhci协议](https://github.com/chenlongos/raspi4-with-arceos-doc/blob/master/src/assert/extensible-host-controler-interface-usb-xhci.pdf)

大概可以分为以下几步：

1. PCIe总线初始化

2. PCIe检测设备、分配内存空间

3. XHCI主机控制器初始化

4. XHCI检测设备、分配地址空间

5. 解析设备配置，加载对应驱动

6. USB转串口的设备驱动实现

7. 完成技术需求，撰写技术总结文档

