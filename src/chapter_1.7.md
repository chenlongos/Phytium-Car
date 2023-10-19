# 实验四：树莓派启动 ArceOS,打印Hello,world

1. 在ArceOS目录下，输入：

   ```shell
   make A=apps/helloworld ARCH=aarch64 PLATFORM=aarch64-raspi4  LOG=debug 
   ```

   编译出ArceOS在raspi4 上的一个镜像。

2. 根据前置知识二编译生成一个kernel8.img文件：

   ```shell
   BSP=rpi4 make
   ```

3. 把 kernel8.img 和 helloworld_aarch64-raspi4.bin 通过 cat 命令拼接到一个 bin 文件中，仍然取名为 kernel8.img

   ```
   cat ../rust-raspberrypi-OS-tutorials/06_uart_chainloader/kernel8.img apps/helloworld/helloworld_aarch64-raspi4.bin > kernel8.img
   ```

4. 把新生成的 kernel8.img 拷贝到 sd 卡上，重新启动树莓派板子，可以看到上电后输出 miniload 并进入到 arceos helloworld 里

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
   [  0.114576 0 axruntime:127] Primary CPU 0 started, dtb = 0x0.
   [  0.121522 0 axruntime:129] Found physcial memory regions:
   [  0.128209 0 axruntime:131]   [PA:0x80000, PA:0x86000) .text (READ | EXECUTE | RESERVED)
   [  0.137498 0 axruntime:131]   [PA:0x86000, PA:0x88000) .rodata (READ | RESERVED)
   [  0.146093 0 axruntime:131]   [PA:0x88000, PA:0x8c000) .data .tdata .tbss .percpu (READ | WRITE | RESERVED)
   [  0.157033 0 axruntime:131]   [PA:0x8c000, PA:0xcc000) boot stack (READ | WRITE | RESERVED)
   [  0.166583 0 axruntime:131]   [PA:0xcc000, PA:0xcd000) .bss (READ | WRITE | RESERVED)
   [  0.175613 0 axruntime:131]   [PA:0x0, PA:0x1000) spintable (READ | WRITE | RESERVED)
   [  0.184642 0 axruntime:131]   [PA:0xcd000, PA:0xfc000000) free memory (READ | WRITE | FREE)
   [  0.194193 0 axruntime:131]   [PA:0xfe201000, PA:0xfe202000) mmio (READ | WRITE | DEVICE | RESERVED)
   [  0.204525 0 axruntime:131]   [PA:0xff841000, PA:0xff849000) mmio (READ | WRITE | DEVICE | RESERVED)
   [  0.214857 0 axruntime:149] Initialize platform devices...
   [  0.221542 0 axruntime:185] Primary CPU 0 init OK.
   Hello, world! 
   [  9.556116 0 axruntime:198] main task exited: exit_code=0
   [  9.560792 0 axhal::platform::aarch64_raspi::misc:21] Shutting down...
   ```

至此，实验四结束，最终提交ArceOS成功运行，打印Hello，world的结果。



