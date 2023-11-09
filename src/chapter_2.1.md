# 实验一：UART0 的启用

1. 在rust-raspberrypi-OS-tutorials/05_drivers_gpio_uart中输入：
   
   ```shell
   BSP=rpi4 make
   ```

   编译生成一个kernel8.img文件。

2. 把新生成的 kernel8.img 拷贝到 sd 卡上，并将SD卡插入RPi。

3. 运行`miniterm` target，在主机上打开UART设备：

   ```shell
   make miniterm
   ```

4. 将USB串口连接到主机PC，然后打开树莓派电源开关。

5. 会得到以下输出：
 
    ```shell
   Miniterm 1.0
   
   [MT] ⏳ Waiting for /dev/ttyUSB0
   [MT] ✅ Serial connected
   [0] mingo version 0.5.0
   [1] Booting on: Raspberry Pi 4
   [2] Drivers loaded:
         1. BCM PL011 UART
         2. BCM GPIO
   [3] Chars written: 117
   [4] Echoing input now
   ```
    
**如果没有树莓派主板，可以在qemu上运行生成的kernel8.img。**

   ```shell
   ./qemu-system-aarch64 -m 2G -smp 4 -cpu cortex-a72 -machine raspi4b2g -nographic -kernel rust-raspberrypi-OS-tutorials/05_drivers_gpio_uart/kernel8.img
   ```



至此，实验一结束，最终提交实验过程记录（包含出现的各类问题及解决办法）以及正确的输出结果。
