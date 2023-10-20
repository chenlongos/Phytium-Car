# 实验五：通过 UART0 发送指令，由 UART2/3/4/5 驱动小车

1. 将包含shell命令的kernel8.img文件复制到小车SD卡上。

2. 将小车与PC通过UART0相连，进去shell命令。

3. 通过往寄存器里写入值，来启用UART5。（2/3/4）

   ```shell
   let str_addr1 = "ffff0000fe200004 246c0";
   do_str(str_addr1);
   let str_addr2 = "ffff0000fe2000e4 55000000";
   do_str(str_addr2);
   let str_addr3 = "ffff0000fe201a24 1A";
   let str_addr4 = "ffff0000fe201a28 3";
   do_str(str_addr3);
   do_str(str_addr4);
   let str_addr5 = "ffff0000fe201a2c 70";
   do_str(str_addr5);
   let str_addr6 = "ffff0000fe201a30 301";
   do_str(str_addr6);
   ```

   此时，UART5便被启用，可以通信。

4. 将UART5串口与小车相连。

5. 通过go命令驱动小车进行运动：

   ```shell
   let uart_base = 0xffff_0000_fe20_1a00 as *mut u8;
   let mut uart = Pl011Uart::new(uart_base);

   match args {
        "f" => {
            //前进
            uart.putchar(0xff);
            uart.putchar(0xfc);
            uart.putchar(0x07);
            uart.putchar(0x11);
            uart.putchar(0x01);
            uart.putchar(0x01);
            uart.putchar(0x64);
            uart.putchar(0x00);
            uart.putchar(0x7e);
        }
        "s" => {
            //停止
            uart.putchar(0xff);
            uart.putchar(0xfc);
            uart.putchar(0x07);
            uart.putchar(0x11);
            uart.putchar(0x01);
            uart.putchar(0x00);
            uart.putchar(0x00);
            uart.putchar(0x00);
            uart.putchar(0x19);
        }
        "r" => {
            //右转
            uart.putchar(0xff);
            uart.putchar(0xfc);
            uart.putchar(0x07);
            uart.putchar(0x11);
            uart.putchar(0x01);
            uart.putchar(0x06);
            uart.putchar(0x64);
            uart.putchar(0x00);
            uart.putchar(0x83);
        }
        "l" => {
            //左转
            uart.putchar(0xff);
            uart.putchar(0xfc);
            uart.putchar(0x07);
            uart.putchar(0x11);
            uart.putchar(0x01);
            uart.putchar(0x05);
            uart.putchar(0x64);
            uart.putchar(0x00);
            uart.putchar(0x82);
        }
        "w" => {
            //鸣笛
            uart.putchar(0xff);
            uart.putchar(0xfc);
            uart.putchar(0x05);
            uart.putchar(0x02);
            uart.putchar(0x60);
            uart.putchar(0x00);
            uart.putchar(0x67);
        }
        _ => {}
   }
   ```

   在shell中，输入 go f ，小车便会向前行驶。

   


