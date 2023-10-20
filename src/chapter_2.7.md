# 实验三：通过 UART0 启用 UART5

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

此时，UART5便被成功启用，可以通过UART5来进行数据通信。



     

     

