# 前置知识二：树莓派配置及启动流程

1. SD卡的配置：
   * 创建一个名为`boot`的`FAT32`分区
   * 在SD卡上生成一个名为`config.txt`的文件，并将以下内容写入其中：

    ```txt
    arm_64bit=1
    init_uart_clock=48000000
    ```
   * 从[Raspberry Pi firmware repo](https://github.com/raspberrypi/firmware/tree/master/boot)中将以下文件复制到SD卡上：
     - [fixup4.dat](https://github.com/raspberrypi/firmware/raw/master/boot/fixup4.dat)
     - [start4.elf](https://github.com/raspberrypi/firmware/raw/master/boot/start4.elf)
     - [bcm2711-rpi-4-b.dtb](https://github.com/raspberrypi/firmware/raw/master/boot/bcm2711-rpi-4-b.dtb)
     - ~~[bootcode.bin](https://github.com/raspberrypi/firmware/raw/master/boot/bootcode.bin)~~ （树莓派3需要，4不需要）
    * 将通过编译生成的`kernel8.img`复制到SD卡上
2. 启动流程：
   * 硬件初始化： 树莓派4上电后，硬件会被初始化，包括CPU、内存、外设等。
   * GPU加载启动代码（bootcode.bin）： GPU（图形处理单元）是树莓派启动的主要控制器。在启动时，GPU会从SD卡的boot分区加载一个文件，通常是 bootcode.bin。这个文件包含了GPU的启动代码，负责初始化系统硬件，设置内存分配和加载下一阶段的启动代码。
   * 加载启动配置文件（config.txt）： GPU加载 config.txt 文件，该文件包含了系统的配置信息，比如时钟频率、内存分配等。
   * 加载启动文件（start4.elf）： GPU加载 start4.elf 文件，它是一个二进制文件，包含了树莓派系统的启动代码，它负责初始化硬件和启动ARM处理器。
   * 加载设备树文件（bcm2711-rpi-4-b.dtb）：start4.elf文件应该也会去读取设备树文件，然后设置一些基本的参数。
   * 加载操作系统内核（kernel8.img）： start4.elf 文件会加载操作系统内核，是一个名为 kernel8.img 的文件。这个内核文件是一个裸机可执行文件，包含了操作系统的核心功能。
   * 初始化和启动操作系统： 内核文件被加载到内存后，GPU将控制权交给ARM处理器，操作系统开始初始化并启动，完成系统的启动过程。

3. kernel8.img的生成
   * 首先，克隆这个仓库：

     ```shell
     git clone https://github.com/chenlongos/rust-raspberrypi-OS-tutorials.git
     ```

   * 然后，在06_uart_chainloader目录下，执行：

     ```
     BSP=rpi4 make
     ```

   便可以看到生成了一个kernel8.img文件。
   
