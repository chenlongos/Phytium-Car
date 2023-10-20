# 前置知识一：树莓派 GPIO 功能

## 引脚介绍
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

## 相关寄存器

GPIO 寄存器基地址：0xfe200000

### 寄存器简介

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

  




### 关于寄存器的代码介绍

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
