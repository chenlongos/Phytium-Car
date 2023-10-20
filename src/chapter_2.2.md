# 前置知识二：UART 通信

UART 是一种通用的串行、异步通信总线，该总线有两条数据线，TXD 用于发送数据，RXD用于接收数据，在嵌入式系统中常用于主机与辅助设备之间的通信。

UART连接方式如图：



## 基本特性和工作原理：

1. 异步性：
UART是异步通信协议，发送和接收设备之间不需要共享时钟信号。相反，它使用起始位（Start Bit）和停止位（Stop Bit）来标识每个数据字节的开始和结束，以及一个或多个数据位组成的数据字节。

2. 数据帧：
UART通信的数据传输以数据帧为单位。每个数据帧通常包括一个起始位、8位数据（通常是8位，但也可以是其他位数）、一个可选的奇偶校验位和一个或多个停止位。起始位和停止位提供了数据的边界，确保接收端能够准确地识别每个字节。

3. 波特率：
波特率是UART通信中的数据传输速度，通常以每秒位数（bps，bits per second）为单位。发送和接收设备必须使用相同的波特率进行通信，以确保数据的正确传输。
波特率计算公式：波特率=时钟频率/16*（IBRD+(FBRD)/64）

4. 起始位和停止位：
起始位指示数据传输的开始，停止位标志着数据传输的结束。当接收端检测到起始位时，它开始接收数据位，然后是奇偶校验位（如果启用），最后是停止位。接收端通过检测停止位来确定整个数据帧的结束。

5. 奇偶校验：
UART通信可以使用奇偶校验位来验证数据的准确性。奇偶校验可以选择奇数或偶数校验。发送端计算数据位中1的数量，如果选择奇校验，发送端会确保总位数（包括校验位）为奇数；如果选择偶校验，发送端会确保总位数为偶数。



## 相关寄存器

### 寄存器简介

根据[官方文档](https://datasheets.raspberrypi.com/bcm2711/bcm2711-peripherals.pdf)，以下是关于部分寄存器的介绍。

| Offset | Name | Description |
|-------|-------|-------|
| 0x00 | DR | Data Register |
| 0x04 | RSRECR |  |
| 0x18 | FR | Flag register |
| 0x24 | IBRD | Integer Baud rate divisor |
| 0x28 | FBRD | Fractional Baud rate divisor |
| 0x2c | LCRH | Line Control register |
| 0x30 | CR | Control register |
| 0x44 | ICR | Interrupt Clear Register |
| 0x48 | DMACR | DMA Control Register |

* DR寄存器，数据寄存器，用于存储将要发送的数据或接收到的数据。
  
* FR寄存器，标志寄存器。   
   
* IBRD寄存器，整数波特率除数寄存器。
   
* FBRD寄存器，分数波特率除数寄存器。

IBRD和FBRD寄存器决定了波特率的数值。
     
* LCR_H寄存器，线控制寄存器，用于配置 UART 的数据格式，包括数据位数、停止位数、奇偶校验设置等。
   
* CR寄存器，控制寄存器，控制UART的启用，控制发送和接收数据的启用。
    
* ICR寄存器，中断清除寄存器，用于清除 UART 中断标志，以便在处理中断后复位相应的中断状态。具体来说，当某个中断发生时，相应的标志位会被设置。通过写入 ICR 寄存器，可以清除这些标志位。


### 关于寄存器的代码部分
位置：[rust-raspberrypi-OS-tutorials/06_uart_chainloader/src/bsp/device_driver/bcm/bcm2xxx_pl011_uart.rs](https://github.com/chenlongos/rust-raspberrypi-OS-tutorials/blob/master/06_uart_chainloader/src/bsp/device_driver/bcm/bcm2xxx_pl011_uart.rs)：

```rust
register_bitfields! {
    u32,

    /// Flag Register.
    FR [
        /// Transmit FIFO empty. The meaning of this bit depends on the state of the FEN bit in the
        /// Line Control Register, LCR_H.
        ///
        /// - If the FIFO is disabled, this bit is set when the transmit holding register is empty.
        /// - If the FIFO is enabled, the TXFE bit is set when the transmit FIFO is empty.
        /// - This bit does not indicate if there is data in the transmit shift register.
        TXFE OFFSET(7) NUMBITS(1) [],

        /// Transmit FIFO full. The meaning of this bit depends on the state of the FEN bit in the
        /// LCR_H Register.
        ///
        /// - If the FIFO is disabled, this bit is set when the transmit holding register is full.
        /// - If the FIFO is enabled, the TXFF bit is set when the transmit FIFO is full.
        TXFF OFFSET(5) NUMBITS(1) [],

        /// Receive FIFO empty. The meaning of this bit depends on the state of the FEN bit in the
        /// LCR_H Register.
        ///
        /// - If the FIFO is disabled, this bit is set when the receive holding register is empty.
        /// - If the FIFO is enabled, the RXFE bit is set when the receive FIFO is empty.
        RXFE OFFSET(4) NUMBITS(1) [],

        /// UART busy. If this bit is set to 1, the UART is busy transmitting data. This bit remains
        /// set until the complete byte, including all the stop bits, has been sent from the shift
        /// register.
        ///
        /// This bit is set as soon as the transmit FIFO becomes non-empty, regardless of whether
        /// the UART is enabled or not.
        BUSY OFFSET(3) NUMBITS(1) []
    ],

    /// Integer Baud Rate Divisor.
    IBRD [
        /// The integer baud rate divisor.
        BAUD_DIVINT OFFSET(0) NUMBITS(16) []
    ],

    /// Fractional Baud Rate Divisor.
    FBRD [
        ///  The fractional baud rate divisor.
        BAUD_DIVFRAC OFFSET(0) NUMBITS(6) []
    ],

    /// Line Control Register.
    LCR_H [
        /// Word length. These bits indicate the number of data bits transmitted or received in a
        /// frame.
        #[allow(clippy::enum_variant_names)]
        WLEN OFFSET(5) NUMBITS(2) [
            FiveBit = 0b00,
            SixBit = 0b01,
            SevenBit = 0b10,
            EightBit = 0b11
        ],

        /// Enable FIFOs:
        ///
        /// 0 = FIFOs are disabled (character mode) that is, the FIFOs become 1-byte-deep holding
        /// registers.
        ///
        /// 1 = Transmit and receive FIFO buffers are enabled (FIFO mode).
        FEN  OFFSET(4) NUMBITS(1) [
            FifosDisabled = 0,
            FifosEnabled = 1
        ]
    ],

    /// Control Register.
    CR [
        /// Receive enable. If this bit is set to 1, the receive section of the UART is enabled.
        /// Data reception occurs for either UART signals or SIR signals depending on the setting of
        /// the SIREN bit. When the UART is disabled in the middle of reception, it completes the
        /// current character before stopping.
        RXE OFFSET(9) NUMBITS(1) [
            Disabled = 0,
            Enabled = 1
        ],

        /// Transmit enable. If this bit is set to 1, the transmit section of the UART is enabled.
        /// Data transmission occurs for either UART signals, or SIR signals depending on the
        /// setting of the SIREN bit. When the UART is disabled in the middle of transmission, it
        /// completes the current character before stopping.
        TXE OFFSET(8) NUMBITS(1) [
            Disabled = 0,
            Enabled = 1
        ],

        /// UART enable:
        ///
        /// 0 = UART is disabled. If the UART is disabled in the middle of transmission or
        /// reception, it completes the current character before stopping.
        ///
        /// 1 = The UART is enabled. Data transmission and reception occurs for either UART signals
        /// or SIR signals depending on the setting of the SIREN bit
        UARTEN OFFSET(0) NUMBITS(1) [
            /// If the UART is disabled in the middle of transmission or reception, it completes the
            /// current character before stopping.
            Disabled = 0,
            Enabled = 1
        ]
    ],

    /// Interrupt Clear Register.
    ICR [
        /// Meta field for all pending interrupts.
        ALL OFFSET(0) NUMBITS(11) []
    ]
}

register_structs! {
    #[allow(non_snake_case)]
    pub RegisterBlock {
        (0x00 => DR: ReadWrite<u32>),
        (0x04 => _reserved1),
        (0x18 => FR: ReadOnly<u32, FR::Register>),
        (0x1c => _reserved2),
        (0x24 => IBRD: WriteOnly<u32, IBRD::Register>),
        (0x28 => FBRD: WriteOnly<u32, FBRD::Register>),
        (0x2c => LCR_H: WriteOnly<u32, LCR_H::Register>),
        (0x30 => CR: WriteOnly<u32, CR::Register>),
        (0x34 => _reserved3),
        (0x44 => ICR: WriteOnly<u32, ICR::Register>),
        (0x48 => @END),
    }
}

impl PL011UartInner {
    
    ... ...

    pub fn init(&mut self) {
        
        self.flush();

        // Turn the UART off temporarily.
        self.registers.CR.set(0);

        // Clear all pending interrupts.
        self.registers.ICR.write(ICR::ALL::CLEAR);

        // Set new baud rate = 115200
        // (48_000_000 / 16) / 115200 = 26.0416667
        // INTEGER((0.0416667 * 64) + 0.5) = 3.16666688
        self.registers.IBRD.write(IBRD::BAUD_DIVINT.val(26));
        self.registers.FBRD.write(FBRD::BAUD_DIVFRAC.val(3));

        self.registers
            .LCR_H
            .write(LCR_H::WLEN::EightBit + LCR_H::FEN::FifosEnabled);

        // Turn the UART on.
        self.registers
            .CR
            .write(CR::UARTEN::Enabled + CR::TXE::Enabled + CR::RXE::Enabled);
    }

    ... ...
}
```

要将波特率设置为115200，所以，在代码中：

```
self.registers.IBRD.write(IBRD::BAUD_DIVINT.val(26));
self.registers.FBRD.write(FBRD::BAUD_DIVFRAC.val(3));
```

要启用FIFO，并且将发送或接收的数据位数设置为8位，所以：

```
self.registers
            .LCR_H
            .write(LCR_H::WLEN::EightBit + LCR_H::FEN::FifosEnabled);
```

还要启用表示UART启用，同时启用发送和接收数据，所以：

```
self.registers
    .CR
    .write(CR::UARTEN::Enabled + CR::TXE::Enabled + CR::RXE::Enabled);
```

