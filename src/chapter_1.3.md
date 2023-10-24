# 实验三：Qemu 模拟器启动 ArceOS,执行shell命令

1. 在ArceOS目录下，输入：
   
   ```shell
   make A=apps/fs/shell ARCH=aarch64 PLATFORM=aarch64-raspi4 SMP=4 BLK=y FEATURES=driver-ramdisk
   ```

   编译出ArceOS在raspi4 上的镜像。

2. 在qemu模拟器上运行该镜像：

   ```shell
   ./qemu-system-aarch64 -m 2G -smp 4 -cpu cortex-a72 -machine raspi4b2g  -nographic -kernel arceos/apps/fs/shell/shell_aarch64-raspi4.bin
   ```

3. 看到ArceOS在qemu模拟器中成功运行：
   
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
   smp = 4
   build_mode = release
   log_level = warn
   
   [  0.032734 fatfs::boot_sector:615] Invalid FAT type
   [  0.055230 fatfs::dir:140] Is a directory
   [  0.059208 fatfs::dir:140] Is a directory
   [  0.064018 fatfs::dir:140] Is a directory
   [  0.066387 fatfs::dir:140] Is a directory
   Available commands:
     cat
     cd
     echo
     exit
     help
     ls
     mkdir
     pwd
     rm
     uname
     ldr
     str
   arceos:/$
   ```


4. 尝试运行ldr和str命令：
   
   * ldr命令是用来读取地址所存储的值，例如输入`ldr ffff0000fe201000`，就会读出在地址为ffff0000fe201000中存储的值。

   ```shell
   arceos# ldr ffff0000fe201000
   ldr
   Value at address 0xffff0000fe201000: 0x66
   ```

   * str命令是用来往地址中写入值的，例如输入`str ffff0000400fe000 123`，就会往地址为ffff0000400fe000中写入123,输入234，就会写入234。

   ```shell
   arceos# str ffff0000400fe000 123
   arceos# ldr ffff0000400fe000
   ldr
   Value at address 0xffff0000400fe000: 0x123
   arceos# str ffff0000400fe000 234
   arceos# ldr ffff0000400fe000
   ldr
   Value at address 0xffff0000400fe000: 0x234
   ```

5. 尝试输出字母A：

   尝试向ffff0000fe201000中写入41，使其输出字母A：
   ```shelll
   arceos:/$ str ffff0000fe201000 41
   AWrite value at address ffff0000fe201000: 0x41
   ```

   可以在最后一行的开头看到输出了一个字母A。

至此，实验三结束，最终提交实验过程记录（包含出现的各类问题及解决办法）以及关于ArceOS成功运行，并且尝试执行ldr、str命令，最后再成功输出字母A的结果。（相关代码位置：arceos/apps/cli/src/）

   

   





