# 实验三：通过 UART0 启用 UART5

## 若已有树莓派开发板则进行以下操作：

1. GPIO改为ALT4功能启用UART5
   
   根据前置知识，已经知道GPIO12和GPIO13的ALT4功能可以用来进行串口通信，为UART5。其中，GPIO12对应着TXD/发送、GPIO13对应着RXD/接收。

   * 在原先启用GPIO14、15的ALT0的基础上，需要在偏移地址为0x04的GPFSEL1寄存器的第6-8位和第9-11位将值设置为0b011，也就是将值变为0b0010 0100 0110 1100 0000即0x246c0。
  
     ```shell
     let str_addr1 = "ffff0000fe200004 246c0";
     do_str(str_addr1);
     ```

   * 在偏移地址为0xe4的GPIO_PUP_PDN_CNTRL_REG0寄存器中将值变为0b0101_0101_0000_0000_0000_0000_0000_0000即0x5500_0000。
  
     ```shell
     let str_addr2 = "ffff0000fe2000e4 55000000";
     do_str(str_addr2);
     ```

   这样GPIO12、13的功能就变为了ALT4。

2. 配置UART相关寄存器

   通过bcm2711的数据手册，可以知道UART5的起始地址为**0xfe201a00**。

   * 波特率设置为115200，将偏移地址为0x24的IBRD寄存器的值变为26，将偏移地址为0x28的FBRD寄存器的值变为3。
  
     ```shell
     let str_addr3 = "ffff0000fe201a24 1A";
     let str_addr4 = "ffff0000fe201a28 3";
     do_str(str_addr3);
     do_str(str_addr4);
     ```

   * 启用FIFO缓冲区，设置数据位数为8位，即将偏移地址为0x2c的寄存器的值变为70。
    
     ```shell
     let str_addr5 = "ffff0000fe201a2c 70";
     do_str(str_addr5);
     ```

   * 启用UART，启用发送接收，即将偏移地址为0x30的寄存器的值变为301。

     ```shell
     let str_addr6 = "ffff0000fe201a30 301";
     do_str(str_addr6);
     ```

     所以，在shell命令的源代码中，添加一个命令 UART ，使得输入 UART 5 便可以启用 UART 5：

     ```rust
     fn do_UART(args: &str) {
         match args {
         "5" =>{
             let str_addr0 = "ffff0000fe200000 1B";
             let str_addr1 = "ffff0000fe200004 246c0";
             let str_addr2 = "ffff0000fe2000e4 55000000";
             let str_addr3 = "ffff0000fe201a24 1A";
             let str_addr4 = "ffff0000fe201a28 3";
             let str_addr5 = "ffff0000fe201a2c 70";
             let str_addr6 = "ffff0000fe201a30 301";
             //调用str写入函数
             do_str(str_addr0);
             do_str(str_addr1);
             do_str(str_addr2);
             do_str(str_addr3);
             do_str(str_addr4);
             do_str(str_addr5);
             do_str(str_addr6);
             }
         _ => {}
         } 
     }
     ```

3. 将以下测试函数添加到shell命令中：
   
   ```rust
   fn do_test(args: &str) {
           fn delay(seconds: u64) {
            for i in 1..seconds + 1 {
                fn fibonacci_recursive(n: u64) -> u64 {
                    if n == 0 {
                        return 0;
                    }
                    if n == 1 {
                        return 1;
                    }
                    return fibonacci_recursive(n - 1) + fibonacci_recursive(n - 2);
                }
                fibonacci_recursive(36 + (i % 2));
            }
        }
    if args == "run" {
        loop {
            let arges = "ffff0000fe201a00 41";
            do_str(arges);
            delay(4);
        }
    }
   }
   ```
   
   再启用UART 5后，运行此命令 test run ,然后将UART 5的TXD、RXD和GND与PC相连，观察输出。

   如果屏幕中会不断地输出字母A，则表示UART5被成功启用，可以通过UART5来进行数据通信。

## 若没有开发板则进行以下操作：

1. 查看rust-raspberrypi-OS-tutorials/06_uart_chainloader关于gpio和uart的代码

   位置：`rust-raspberrypi-OS-tutorials/06_uart_chainloader/src/bsp/device_driver/bcm/`

   修改bcm2xxx_gpio.rs中的：

   ```rust
   GPFSEL1 [
        /// Pin 15
        FSEL15 OFFSET(15) NUMBITS(3) [
            Input = 0b000,
            Output = 0b001,
            AltFunc0 = 0b100  // PL011 UART RX

        ],

        /// Pin 14
        FSEL14 OFFSET(12) NUMBITS(3) [
            Input = 0b000,
            Output = 0b001,
            AltFunc0 = 0b100  // PL011 UART TX
        ]
    ],
   ```

   将FSEL14、15改为12、13，将OFFSET偏移位改为6、9， 由于需要启用的UART5的ALT功能为4，所以需要将`AltFunc0 = 0b100`变为`AltFunc4 = 0b010`

   ```rust
   GPIO_PUP_PDN_CNTRL_REG0 [
        /// Pin 15
        GPIO_PUP_PDN_CNTRL15 OFFSET(30) NUMBITS(2) [
            NoResistor = 0b00,
            PullUp = 0b01
        ],

        /// Pin 14
        GPIO_PUP_PDN_CNTRL14 OFFSET(28) NUMBITS(2) [
            NoResistor = 0b00,
            PullUp = 0b01
        ],
    ]
   ```

   将OFFSET偏移位改为24、26

2. 用UART5的基地址替换掉UART0的基地址:

   位置：`rust-raspberrypi-OS-tutorials/06_uart_chainloader/src/bsp/raspberrypi/memory.rs`

   ```rust
   pub const UART_OFFSET:         usize = 0x0020_1000;
   ···
   pub const START:            usize =         0xFE00_0000;
   pub const PL011_UART_START: usize = START + UART_OFFSET;
   ```

   将UART0的基地址fe201000变为UART5的基地址fe201a00

3. 生成kernel8.img文件，尝试在qemu模拟器中运行kernel8.img

上述步骤是将原先可用的串口UART0变为了串口UART5，所以还是只启用了一个串口，因此

4. 在rust-raspberrypi-OS-tutorials/06_uart_chainloader中修改并添加代码，使得串口UART0和UART5都被直接启用

    

至此，实验三结束，最终提交实验过程记录（包含出现的各类问题及解决办法）以及屏幕不断打印字母A的截图。
