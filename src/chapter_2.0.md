# （选做）第二阶段：Rust编写树莓派串口驱动

***此阶段主要关于树莓派如何启用串口。***

## 前置知识一：树莓派 GPIO 功能

### 引脚介绍
* 下图为树莓派40个引脚的介绍。

![](https://github.com/apengaaa/ROSMASTER-X3/raw/main/First%20Week/picture/%E5%BC%95%E8%84%9A.PNG)

* 根据[官方文档](https://datasheets.raspberrypi.com/bcm2711/bcm2711-peripherals.pdf)，可知各种 UART 信号如何（包括迷你 UART）映射到通用 I/O (GPIO) 上。
  

|  | Pull | ALT0 | ALT1 | ALT2 | ALT3 | ALT4 | ALT5 |
|-------|-------|-------|-------|-------|-------|-------|-------| 
| GPIO0 | High |  |  |  |  | TXD2 |  |
| GPIO1 | High |  |  |  |  | RXD2 |  |
| GPIO2 | High |  |  |  |  | CTS2 |  |
| GPIO3 | High |  |  |  |  | RTS2 |  |
| GPIO4 | High |  |  |  |  | TXD3 |  |
| GPIO5 | High |  |  |  |  | RXD3 |  |
| GPIO6 | High |  |  |  |  | CTS3 |  |
| GPIO7 | High |  |  |  |  | RTS3 |  |
| GPIO8 | High |  |  |  |  | TXD4 |  |
| GPIO9 | High |  |  |  |  | RXD4 |  |
| GPIO10 | High |  |  |  |  | CTS4 |  |
| GPIO11 | High |  |  |  |  | RTS4 |  |
| GPIO12 | High |  |  |  |  | TXD5 |  |
| GPIO13 | High |  |  |  |  | RXD5 |  |
| GPIO14 | Low | TXD0 |  |  |  | CTS5 |  |
| GPIO15 | Low | RXD0 |  |  |  | RTS5 |  |
| GPIO16 | Low |  |  |  | CTS0 |  |  |
| GPIO17 | Low |  |  |  | RTS0 |  |  |

### 相关寄存器

GPIO 寄存器基地址：0xfe200000

#### 寄存器简介

* 根据[官方文档](https://datasheets.raspberrypi.com/bcm2711/bcm2711-peripherals.pdf)，以下表格是部分寄存器的介绍。

  
| Offset | Name | Description |
|-------|-------|-------|
| 0x00 | GPFSEL0 | GPIO Function Select 0 |
| 0x04 | GPFSEL1 | GPIO Function Select 1 |
| 0x08 | GPFSEL2 | GPIO Function Select 2 |
| 0x0c | GPFSEL3 | GPIO Function Select 3 |
| 0x10 | GPFSEL4 | GPIO Function Select 4 |
| 0x14 | GPFSEL5 | GPIO Function Select 5 |
| 0xe4 | GPIO_PUP_PDN_CNTRL_REG0 | GPIO Pull-up / Pull-down Register 0 |
| 0xe8 | GPIO_PUP_PDN_CNTRL_REG1 | GPIO Pull-up / Pull-down Register 1 |
| 0xec | GPIO_PUP_PDN_CNTRL_REG2 | GPIO Pull-up / Pull-down Register 2 |
| 0xf0 | GPIO_PUP_PDN_CNTRL_REG3 | GPIO Pull-up / Pull-down Register 3 |

* GPFSEL寄存器，主要是控制GPIOxx--GPIOxx的功能选择。

  GPFSEL0控制GPIO0--GPIO9的功能选择；

  GPFSEL1控制GPIO10--GPIO19的功能选择；

  GPFSEL2控制GPIO20--GPIO29的功能选择；

  GPFSEL3控制GPIO30--GPIO39的功能选择；

  GPFSEL4控制GPIO40--GPIO49的功能选择；

  GPFSEL5控制GPIO50--GPIO57的功能选择。

  该寄存器存储32位的二进制数，每3位代表了一个GPIO引脚选择的功能，最后两位保留，其中，每个值的对应关系为
  
  ```
  000 = GPIO Pin ~ is an input
  001 = GPIO Pin ~ is an output
  100 = GPIO Pin ~ takes alternate function 0
  101 = GPIO Pin ~ takes alternate function 1
  110 = GPIO Pin ~ takes alternate function 2
  111 = GPIO Pin ~ takes alternate function 3
  011 = GPIO Pin ~ takes alternate function 4
  010 = GPIO Pin ~ takes alternate function 5
  ```

* GPIO_PUP_PDN_CNTRL_REG寄存器，主要用于对GPIOxx--GPIOxx配置GPIO引脚的上拉（Pull-Up）和下拉（Pull-Down）电阻（上拉电阻会将引脚连接到正电源，从而将引脚拉高到高电平；下拉电阻会将引脚连接到地（GND），从而将引脚拉低到低电平）。
 
  GPIO_PUP_PDN_CNTRL_REG0用于对GPIO0--GPIO15配置GPIO引脚的上拉（Pull-Up）和下拉（Pull-Down）电阻；
    
  GPIO_PUP_PDN_CNTRL_REG1用于对GPIO16--GPIO31配置GPIO引脚的上拉（Pull-Up）和下拉（Pull-Down）电阻；
    
  GPIO_PUP_PDN_CNTRL_REG2用于对GPIO32--GPIO47配置GPIO引脚的上拉（Pull-Up）和下拉（Pull-Down）电阻；
    
  GPIO_PUP_PDN_CNTRL_REG3用于对GPIO48--GPIO57配置GPIO引脚的上拉（Pull-Up）和下拉（Pull-Down）电阻。

  该寄存器存储32位的二进制数，每2位代表了一个GPIO引脚上拉/下拉电阻选择的可能配置，其中，每个值的对应关系为：

  ```
  00 = No resistor is selected
  01 = Pull up resistor is selected
  10 = Pull down resistor is selected
  11 = Reserved
  ```

  




#### 关于寄存器的代码介绍

位置：[rust-raspberrypi-OS-tutorials/06_uart_chainloader/src/bsp/device_driver/bcm/bcm2xxx_gpio.rs](https://github.com/chenlongos/rust-raspberrypi-OS-tutorials/blob/master/06_uart_chainloader/src/bsp/device_driver/bcm/bcm2xxx_gpio.rs)

```rust
register_bitfields! {
    u32,

    /// GPIO Function Select 1
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
        ],
    ],
    /// GPIO Pull-up/down Register
    /// BCM2837 only.
    GPPUD [
        /// Controls the actuation of the internal pull-up/down control line to ALL the GPIO pins.
        PUD OFFSET(0) NUMBITS(2) [
            Off = 0b00,
            PullDown = 0b01,
            PullUp = 0b10
        ]
    ],
    /// GPIO Pull-up/down Clock Register 0
    /// BCM2837 only.
    GPPUDCLK0 [
        /// Pin 15
        PUDCLK15 OFFSET(15) NUMBITS(1) [
            NoEffect = 0,
            AssertClock = 1
        ],
        /// Pin 14
        PUDCLK14 OFFSET(14) NUMBITS(1) [
            NoEffect = 0,
            AssertClock = 1
        ],
    ],

    /// GPIO Pull-up / Pull-down Register 0
    /// BCM2711 only.
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
}

register_structs! {
    #[allow(non_snake_case)]
    RegisterBlock {
        (0x00 => _reserved1),
        (0x04 => GPFSEL1: ReadWrite<u32, GPFSEL1::Register>),
        (0x08 => _reserved2),
        (0x94 => GPPUD: ReadWrite<u32, GPPUD::Register>),
        (0x98 => GPPUDCLK0: ReadWrite<u32, GPPUDCLK0::Register>),
        (0x9C => _reserved3),
        (0xE4 => GPIO_PUP_PDN_CNTRL_REG0: ReadWrite<u32, GPIO_PUP_PDN_CNTRL_REG0::Register>),
        (0xE8 => @END),
    }
}
```

根据上述内容，可以知道GPIO14、15要实现串口通信，需要启用ALT0功能。而这对应着关于GPIO的GPFSEL1寄存器，

所以，在代码中，在第12-14位和第15-17位设置为0b100：

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
        ],
 ]

```
还要设置上拉/下拉电阻，在第28、29位和第30、31位设置为0b01：

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
        ]
  ]
```

## 前置知识二：UART 通信

UART 是一种通用的串行、异步通信总线，该总线有两条数据线，TXD 用于发送数据，RXD用于接收数据，在嵌入式系统中常用于主机与辅助设备之间的通信。

UART连接方式如图：



### 基本特性和工作原理：

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



### 相关寄存器

#### 寄存器简介

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


#### 关于寄存器的代码部分
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



