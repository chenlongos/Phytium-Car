# 实验五：树莓派启动 ArceOS,执行shell命令

1. 在ArceOS目录下，输入：
   
   ```shell
   make A=apps/cli ARCH=aarch64 PLATFORM=aarch64-raspi4 LOG=debug
   ```

   编译出ArceOS在raspi4 上的镜像。

2. 生成kernel8.img文件

   ```shell
   BSP=rpi4 make
   ```

3. 把 kernel8.img 和 cli_aarch64-raspi4.bin 通过 cat 命令拼接到一个 bin 文件中，仍然取名为 kernel8.img：

   ```shell
   cat ../rust-raspberrypi-OS-tutorials/06_uart_chainloader/kernel8.img apps/cli/cli_aarch64-raspi4.bin > kernel8.img
   ```

4. 把新生成的 kernel8.img 拷贝到 sd 卡上。

5. 将树莓派与PC相连，打开串口软件。

6. 启动树莓派板子，可以看到上电后输出 miniload 并进入到 shell 命令里：
   
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
   
   [  0.108866 0 axruntime:126] Logging is enabled.
   [  0.114596 0 axruntime:127] Primary CPU 0 started, dtb = 0x0.
   [  0.121542 0 axruntime:129] Found physcial memory regions:
   [  0.128229 0 axruntime:131]   [PA:0x80000, PA:0x8a000) .text (READ | EXECUTE | RESERVED)
   [  0.137517 0 axruntime:131]   [PA:0x8a000, PA:0x8d000) .rodata (READ | RESERVED)
   [  0.146113 0 axruntime:131]   [PA:0x8d000, PA:0x91000) .data .tdata .tbss .percpu (READ | WRITE | RESERVED)
   [  0.157053 0 axruntime:131]   [PA:0x91000, PA:0xd1000) boot stack (READ | WRITE | RESERVED)
   [  0.166603 0 axruntime:131]   [PA:0xd1000, PA:0xd2000) .bss (READ | WRITE | RESERVED)
   [  0.175633 0 axruntime:131]   [PA:0x0, PA:0x1000) spintable (READ | WRITE | RESERVED)
   [  0.184662 0 axruntime:131]   [PA:0xd2000, PA:0xfc000000) free memory (READ | WRITE | FREE)
   [  0.194213 0 axruntime:131]   [PA:0xfe201000, PA:0xfe202000) mmio (READ | WRITE | DEVICE | RESERVED)
   [  0.204545 0 axruntime:131]   [PA:0xff841000, PA:0xff849000) mmio (READ | WRITE | DEVICE | RESERVED)
   [  0.214877 0 axruntime:149] Initialize platform devices...
   [  0.221562 0 axruntime:185] Primary CPU 0 init OK.
   Available commands:
     exit
     help
     uname
     ldr
     str
   arceos#
   ```


4. 尝试修改ldr命令相关代码（位置arceos/apps/cli/src/cmd.rs）：

   * 目前的ldr只能以8个字节为间隔来读取地址的值，要求改为以4个字节为间隔来读取地址的值（地址都是以16进制表示的）
   
   * 目前的ldr只能将完整的地址输入进去才能输出，要求改为输入一个地址加一个数字，可以直接输出一排相邻地址的值（起始的值为输入的地址的值，输出的值的个数取决于输入的数字，不输入默认为1），例如：
     
     
     ```shell
     arceos# ldr ffff0000fe201000
     ldr
     Value at address 0xffff0000fe201000: 0x31
     arceos# ldr ffff0000fe201004
     ldr
     Value at address 0xffff0000fe201000: 0x0
     ```
     
     ```shell
     arceos# ldr ffff0000fe201000 5
     ldr
     Value at address 0xffff0000fe201000: 0x31
     Value at address 0xffff0000fe201004: 0x0
     Value at address 0xffff0000fe201008: 0x0
     Value at address 0xffff0000fe20100c: 0x0
     Value at address 0xffff0000fe201010: 0x0
     ```
     
5. 尝试输出字母A：
   

   尝试向ffff0000fe201000中写入41，使其输出字母A：
   
   ```shelll
   arceos# str ffff0000fe201000 41
   Aarceos# 
   ```

   可以看到输出了一个字母A。

至此，实验五结束，最终提交关于ArceOS成功运行，并且执行ldr一次输出20个值的结果（以ffff0000fe201000为起始地址），最后再成功输出字母A的结果。

   

   





