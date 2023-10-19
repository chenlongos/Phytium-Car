# 实验三：Qemu 模拟器启动 ArceOS,执行shell命令

1. 在ArceOS目录下，输入：
   
```shell
make A=apps/cli ARCH=aarch64 PLATFORM=aarch64-raspi4 LOG=debug
```

   编译出ArceOS在raspi4 上的镜像。

2. 在qemu模拟器上运行该镜像：

```shell
./qemu-system-aarch64 -m 2G -smp 4 -cpu cortex-a72 -machine raspi4b2g -kernel {yourpath}/cli_aarch64-raspi4.bin -nographic
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
   smp = 1
   build_mode = release
   log_level = debug

   [  0.010464 0 axruntime:126] Logging is enabled.
   [  0.012493 0 axruntime:127] Primary CPU 0 started, dtb = 0x100.
   [  0.013329 0 axruntime:129] Found physcial memory regions:
   [  0.014352 0 axruntime:131]   [PA:0x80000, PA:0x8a000) .text (READ | EXECUTE | RESERVED)
   [  0.015884 0 axruntime:131]   [PA:0x8a000, PA:0x8d000) .rodata (READ | RESERVED)
   [  0.016519 0 axruntime:131]   [PA:0x8d000, PA:0x91000) .data .tdata .tbss .percpu (READ | WRITE | RESERVED)
   [  0.017072 0 axruntime:131]   [PA:0x91000, PA:0xd1000) boot stack (READ | WRITE | RESERVED)
   [  0.017513 0 axruntime:131]   [PA:0xd1000, PA:0xd2000) .bss (READ | WRITE | RESERVED)
   [  0.017950 0 axruntime:131]   [PA:0x0, PA:0x1000) spintable (READ | WRITE | RESERVED)
   [  0.018666 0 axruntime:131]   [PA:0xd2000, PA:0xfc000000) free memory (READ | WRITE | FREE)
   [  0.019356 0 axruntime:131]   [PA:0xfe201000, PA:0xfe202000) mmio (READ | WRITE | DEVICE | RESERVED)
   [  0.020055 0 axruntime:131]   [PA:0xff841000, PA:0xff849000) mmio (READ | WRITE | DEVICE | RESERVED)
   [  0.020683 0 axruntime:149] Initialize platform devices...
   [  0.021138 0 axruntime:185] Primary CPU 0 init OK.
   Available commands:
     exit
     help
     uname
     ldr
     str
   arceos# 
```


4. 尝试运行ldr和str命令：
   
   * ldr命令是用来读取地址所存储的值，例如输入`ldr ffff0000fe201000`，就会读出在地址为ffff0000fe201000中存储的值。

   ```shell
   arceos# ldr ffff0000fe201000
   ldr
   Value at address 0xffff0000fe201000: 0x0
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
   arceos# str ffff0000fe201000 41
   Aarceos# 
   ```

   可以看到输出了一个字母A。

至此，实验三结束，最终提交关于ArceOS成功运行，并且尝试执行ldr、str命令，最后再成功输出字母A的结果。（相关代码位置：arceos/apps/cli/src/）

   

   





