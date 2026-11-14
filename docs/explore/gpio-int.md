# GPIO、中断与调试

## Cortex-M0/M0+

GPIO 分 gpio_v1–v3；DEBUG_ACTIVE 归 dbg_v1。IRQ 取自设备头文件，L110 有独立兼容头。

| 建议版本 | 系列或实例 | 布局备注 |
|---|---|---|
| `gpio_v1` | A136 F003 F005 F030 F072 F17x F19x L07x L110 L12x L13x L16x L17x L18x L19x（15） | Pxy_SEL 引脚选择式。v1a 为标准块，v1b（F003/F005/L110）为精简块，v1c（L12x）缺 CTRL0、有 FLTCFG，v1d（L16x/L18x）缺 CTRL0、端口稀疏。 |
| `gpio_v2` | A052 F052（+F002 变体） | GPIOA–D 与 AFRL/AFRH。F002 变体无 DRIVER/AFRH，PU 对应 PUPD。GPIOAUX 为单独的寄存器窗口。 |
| `gpio_v3` | L021 | 混合布局（PINxx_SEL + 端口块 + 输入-only GPIOB），独居一版 |
| `dbg_v1` | A136 F003 F005 F030 F072 F17x F19x L07x L12x L13x L16x L17x L18x L19x（14） | DEBUG_ACTIVE 单寄存器；L12x 基址 0x40021838 |

## HC32F420（Cortex-M4）

GPIO 端口块归 `gpio_v2`。端口 A–D 的固定向量为 3–6。
GPIOAUX 与 SYSCTRL 共用基址 `0x4000B800`，寄存器位于 `+0x70/+0x74/+0x78`，
与 A052/F052 的 GPIOAUX 骨架相同，但 `CR0` 分成 PCLK0/PCLK1 两路分频，
`CR1/CR2` 的部分触发和捕获字段对应不同定时器。GPIOA–D 的时钟门控为
AHB1EN bit 0–3。`driver/inc/gpio.h` 的 `GPIO_PA00_AF_ATIM0_ETR_SET()`
等复用宏直接写 AFRL/AFRH 的 AFSEL 位域。

## SVD 实例与基址

表中使用 SVD 的实例名与基址。SDK 目录与文件来源见 [SVD 总览](../svd-overview.md)。

| 系列 | SVD 实例与基址 |
|---|---|
| A052 | `GPIOA@0x40020C00`, `GPIOAUX@0x40002000`, `GPIOB@0x40021C00`, `GPIOC@0x40022000`, `GPIOD@0x40022400` |
| A136 | `DEBUG_ACTIVE@0x40002038`, `GPIO@0x40020C00` |
| F002 | `GPIOA@0x40020C00`, `GPIOAUX@0x40020CA0`, `GPIOB@0x40020D00`, `GPIOC@0x40020E00`, `GPIOD@0x40020F00` |
| F003 | `DEBUG_ACTIVE@0x40002038`, `GPIO@0x40020C00` |
| F005 | `DEBUG_ACTIVE@0x40002038`, `GPIO@0x40020C00` |
| F030 | `DEBUG_ACTIVE@0x40002038`, `GPIO@0x40020C00` |
| F052 | `GPIOA@0x40020C00`, `GPIOAUX@0x40002000`, `GPIOB@0x40021C00`, `GPIOC@0x40022000`, `GPIOD@0x40022400` |
| F072 | `DEBUG_ACTIVE@0x40002038`, `GPIO@0x40020C00` |
| F17x | `DEBUG_ACTIVE@0x40002038`, `GPIO@0x40020C00` |
| F19x | `DEBUG_ACTIVE@0x40002038`, `GPIO@0x40020C00` |
| F420 | `GPIOA@0x40024000`, `GPIOAUX@0x4000B800`, `GPIOB@0x40024400`, `GPIOC@0x40024800`, `GPIOD@0x40024C00` |
| L021 | `GPIOA@0x40020C00`, `GPIOAUX@0x40020C00`, `GPIOB@0x40020C40` |
| L07x | `DEBUG_ACTIVE@0x40002038`, `GPIO@0x40020C00` |
| L110 | `GPIO@0x40020C00` |
| L12x | `DEBUG_ACTIVE@0x40021838`, `GPIO@0x40020C00` |
| L13x | `DEBUG_ACTIVE@0x40002038`, `GPIO@0x40020C00` |
| L16x | `DEBUG_ACTIVE@0x40002038`, `GPIO@0x40020C00` |
| L17x | `DEBUG_ACTIVE@0x40002038`, `GPIO@0x40020C00` |
| L18x | `DEBUG_ACTIVE@0x40002038`, `GPIO@0x40020C00` |
| L19x | `DEBUG_ACTIVE@0x40002038`, `GPIO@0x40020C00` |

## 外部中断向量

下表取自每个 SDK 设备头的 `IRQn_Type`。表内名称省略 `_IRQn` 后缀，编号为外部 IRQ 号。

### A052

`HC32A052_DLL_Rev1.0.1/mcu/common/hc32a052.h`

| IRQ | 名称 | IRQ | 名称 | IRQ | 名称 |
|---:|---|---:|---|---:|---|
| 0 | `PORTA` | 1 | `PORTB` | 2 | `PORTC` |
| 3 | `PORTD` | 4 | `DMAC` | 5 | `ATIM3` |
| 6 | `USART0` | 7 | `USART1` | 8 | `LPUART0` |
| 9 | `LPUART1` | 10 | `SPI0` | 11 | `SPI1` |
| 12 | `I2C0` | 13 | `I2C1` | 14 | `ATIM0` |
| 15 | `ATIM1` | 16 | `ATIM2` | 17 | `LPTIM` |
| 18 | `GTIM0_BTIM0_1_2` | 19 | `GTIM1_BTIM3_4_5` | 20 | `I2CSLV` |
| 21 | `WWDT` | 22 | `IWDT` | 23 | `RTC` |
| 24 | `ADC` | 26 | `VC0_VC1_LVD` | 28 | `CAN` |
| 29 | `ATIM_BK` | 30 | `FLASH_RAM` | 31 | `CTRIM` |

### A136

`HC32A136_DDL_Rev2.1.0/mcu/common/hc32a136.h`

| IRQ | 名称 | IRQ | 名称 | IRQ | 名称 |
|---:|---|---:|---|---:|---|
| 0 | `PORTA` | 1 | `PORTB` | 2 | `PORTC` |
| 3 | `PORTD` | 4 | `DMAC` | 5 | `TIM3` |
| 6 | `UART0` | 7 | `UART1` | 8 | `LPUART0` |
| 9 | `LPUART1` | 10 | `SPI0` | 11 | `SPI1` |
| 12 | `I2C0` | 13 | `I2C1` | 14 | `TIM0` |
| 15 | `TIM1` | 16 | `TIM2` | 17 | `LPTIM` |
| 18 | `ADTIM4` | 19 | `ADTIM5` | 20 | `ADTIM6` |
| 21 | `PCA` | 22 | `WDT` | 23 | `RTC` |
| 24 | `ADC` | 25 | `PCNT` | 26 | `VC0` |
| 27 | `VC1` | 28 | `LVD` | 29 | `LCD` |
| 30 | `FLASH_RAM` | 31 | `CLKTRIM` |  |  |

### F002

`HC32F002_DDL_Rev1.2.1/mcu/common/HC32F002.h`

| IRQ | 名称 | IRQ | 名称 | IRQ | 名称 |
|---:|---|---:|---|---:|---|
| 0 | `PORTA` | 1 | `PORTB` | 2 | `PORTC` |
| 3 | `PORTD` | 5 | `ATIM3` | 8 | `LPUART0` |
| 9 | `LPUART1` | 10 | `SPI0` | 12 | `I2C` |
| 15 | `BTIM3_4_5` | 15 | `GTIM` | 22 | `IWDT_WWDT` |
| 24 | `ADC` | 28 | `LVD` | 30 | `FLASH` |
| 31 | `CTRIM` |  |  |  |  |

### F003

`HC32F003_DDL_Rev2.0.0/mcu/common/hc32f003.h`

| IRQ | 名称 | IRQ | 名称 | IRQ | 名称 |
|---:|---|---:|---|---:|---|
| 0 | `PORT0` | 1 | `PORT1` | 2 | `PORT2` |
| 3 | `PORT3` | 6 | `UART0` | 7 | `UART1` |
| 10 | `SPI` | 12 | `I2C` | 14 | `TIM0` |
| 15 | `TIM1` | 16 | `TIM2` | 18 | `ADTIM4` |
| 19 | `ADTIM5` | 20 | `ADTIM6` | 21 | `PCA` |
| 22 | `WDT` | 24 | `ADC` | 26 | `VC0` |
| 27 | `VC1` | 28 | `LVD` | 30 | `FLASH_RAM` |
| 31 | `CLKTRIM` |  |  |  |  |

### F005

`HC32F005_DDL_Rev2.0.0/mcu/common/hc32f005.h`

| IRQ | 名称 | IRQ | 名称 | IRQ | 名称 |
|---:|---|---:|---|---:|---|
| 0 | `PORT0` | 1 | `PORT1` | 2 | `PORT2` |
| 3 | `PORT3` | 6 | `UART0` | 7 | `UART1` |
| 10 | `SPI` | 12 | `I2C` | 14 | `TIM0` |
| 15 | `TIM1` | 16 | `TIM2` | 18 | `ADTIM4` |
| 19 | `ADTIM5` | 20 | `ADTIM6` | 21 | `PCA` |
| 22 | `WDT` | 24 | `ADC` | 26 | `VC0` |
| 27 | `VC1` | 28 | `LVD` | 30 | `FLASH_RAM` |
| 31 | `CLKTRIM` |  |  |  |  |

### F030

`HC32F030_DDL_Rev2.2.0/mcu/common/HC32F030.h`

| IRQ | 名称 | IRQ | 名称 | IRQ | 名称 |
|---:|---|---:|---|---:|---|
| 0 | `PORTA` | 1 | `PORTB` | 2 | `PORTC` |
| 3 | `PORTD` | 4 | `DMAC` | 5 | `TIM3` |
| 6 | `UART0` | 7 | `UART1` | 10 | `SPI0` |
| 11 | `SPI1` | 12 | `I2C0` | 13 | `I2C1` |
| 14 | `TIM0` | 15 | `TIM1` | 16 | `TIM2` |
| 18 | `ADTIM4` | 19 | `ADTIM5` | 20 | `ADTIM6` |
| 21 | `PCA` | 22 | `WDT` | 24 | `ADC` |
| 26 | `VC0` | 27 | `VC1` | 28 | `LVD` |
| 30 | `FLASH_RAM` | 31 | `CLKTRIM` |  |  |

### F052

`HC32F052_DDL_Rev1.1.0/mcu/common/hc32f052.h`

| IRQ | 名称 | IRQ | 名称 | IRQ | 名称 |
|---:|---|---:|---|---:|---|
| 0 | `PORTA` | 1 | `PORTB` | 2 | `PORTC` |
| 3 | `PORTD` | 4 | `DMAC` | 5 | `ATIM3` |
| 6 | `USART0` | 7 | `USART1` | 8 | `LPUART0` |
| 9 | `LPUART1` | 10 | `SPI0` | 11 | `SPI1` |
| 12 | `I2C0` | 13 | `I2C1` | 14 | `ATIM0` |
| 15 | `ATIM1` | 16 | `ATIM2` | 17 | `LPTIM` |
| 18 | `GTIM0_BTIM0_1_2` | 19 | `GTIM1_BTIM3_4_5` | 20 | `I2CSLV` |
| 21 | `WWDT` | 22 | `IWDT` | 23 | `RTC` |
| 24 | `ADC` | 26 | `VC0_VC1_LVD` | 28 | `CAN` |
| 29 | `ATIM_BK` | 30 | `FLASH_RAM` | 31 | `CTRIM` |

### F072

`HC32F072_DDL_Rev1.2.0/mcu/common/HC32F072.h`

| IRQ | 名称 | IRQ | 名称 | IRQ | 名称 |
|---:|---|---:|---|---:|---|
| 0 | `PORTA` | 1 | `PORTB` | 2 | `PORTC_E` |
| 3 | `PORTD_F` | 4 | `DMAC` | 5 | `TIM3` |
| 6 | `UART0_2` | 7 | `UART1_3` | 8 | `LPUART0` |
| 9 | `LPUART1` | 10 | `SPI0_I2S0` | 11 | `SPI1_I2S1` |
| 12 | `I2C0` | 13 | `I2C1` | 14 | `TIM0` |
| 15 | `TIM1` | 16 | `TIM2` | 18 | `ADTIM4` |
| 19 | `ADTIM5` | 20 | `ADTIM6` | 21 | `PCA` |
| 22 | `WDT` | 24 | `ADC_DAC` | 26 | `VC0_1_2_LVD` |
| 27 | `USBFS` | 28 | `CAN` | 30 | `FLASH_RAM` |
| 31 | `CLKTRIM_CTS` |  |  |  |  |

### F17x

`HC32F17x_DDL_Rev1.4.0/mcu/common/hc32f17x.h`

| IRQ | 名称 | IRQ | 名称 | IRQ | 名称 |
|---:|---|---:|---|---:|---|
| 0 | `PORTA` | 1 | `PORTB` | 2 | `PORTC_E` |
| 3 | `PORTD_F` | 4 | `DMAC` | 5 | `TIM3` |
| 6 | `UART0_2` | 7 | `UART1_3` | 10 | `SPI0` |
| 11 | `SPI1` | 12 | `I2C0` | 13 | `I2C1` |
| 14 | `TIM0` | 15 | `TIM1` | 16 | `TIM2` |
| 18 | `ADTIM4` | 19 | `ADTIM5` | 20 | `ADTIM6` |
| 21 | `PCA` | 22 | `WDT` | 24 | `ADC_DAC` |
| 26 | `VC0` | 27 | `VC1_2` | 28 | `LVD` |
| 29 | `LCD` | 30 | `FLASH_RAM` | 31 | `CLKTRIM` |

### F19x

`HC32F19x_DDL_Rev1.4.0/mcu/common/hc32f19x.h`

| IRQ | 名称 | IRQ | 名称 | IRQ | 名称 |
|---:|---|---:|---|---:|---|
| 0 | `PORTA` | 1 | `PORTB` | 2 | `PORTC_E` |
| 3 | `PORTD_F` | 4 | `DMAC` | 5 | `TIM3` |
| 6 | `UART0_2` | 7 | `UART1_3` | 10 | `SPI0` |
| 11 | `SPI1` | 12 | `I2C0` | 13 | `I2C1` |
| 14 | `TIM0` | 15 | `TIM1` | 16 | `TIM2` |
| 18 | `ADTIM4` | 19 | `ADTIM5` | 20 | `ADTIM6` |
| 21 | `PCA` | 22 | `WDT` | 24 | `ADC_DAC` |
| 26 | `VC0` | 27 | `VC1_2` | 28 | `LVD` |
| 29 | `LCD` | 30 | `FLASH_RAM` | 31 | `CLKTRIM` |

### F420

`HC32F420_DDL_Rev1.3.1/mcu/common/HC32F420.h`

| IRQ | 名称 | IRQ | 名称 | IRQ | 名称 |
|---:|---|---:|---|---:|---|
| 0 | `CTRIM` | 1 | `VC` | 2 | `LVD` |
| 3 | `PORTA` | 4 | `PORTB` | 5 | `PORTC` |
| 6 | `PORTD` | 7 | `IWDT` | 8 | `WWDT` |
| 9 | `CLK_FAULT` | 10 | `ADC0_ADC1` | 11 | `FLASH` |
| 12 | `USART0_RX` | 13 | `USART0_TX` | 14 | `USART0_FLAG` |
| 15 | `USART1_RX` | 16 | `USART1_TX` | 17 | `USART1_FLAG` |
| 18 | `USART2_RX` | 19 | `USART2_TX` | 20 | `USART2_FLAG` |
| 21 | `USART3_RX` | 22 | `USART3_TX` | 23 | `USART3_FLAG` |
| 24 | `I2C0` | 25 | `I2C1` | 26 | `SPI0_TX` |
| 27 | `SPI0_RX` | 28 | `SPI0_SSR` | 29 | `SPI0_ERR` |
| 30 | `SPI1_TX` | 31 | `SPI1_RX` | 32 | `SPI1_SSR` |
| 33 | `SPI1_ERR` | 34 | `GTIM0_UEV_ETR_BTIMA` | 35 | `GTIM0_CH01_BTIMB` |
| 36 | `GTIM0_CH23_BTIMC` | 37 | `GTIM1_UEV_ETR_BTIMA` | 38 | `GTIM1_CH01_BTIMB` |
| 39 | `GTIM1_CH23_BTIMC` | 40 | `GTIM2_UEV_ETR_BTIMA` | 41 | `GTIM2_CH01_BTIMB` |
| 42 | `GTIM2_CH23_BTIMC` | 43 | `ATIM0_UEV` | 44 | `ATIM0_CHA` |
| 45 | `ATIM0_CHB` | 46 | `ATIM0_BKETR` | 47 | `ATIM3_UEV` |
| 48 | `ATIM3_CHA` | 49 | `ATIM3_CHB` | 50 | `ATIM3_BK_ETR_OV_UD_CMP3` |
| 51 | `DMA_CH0` | 52 | `DMA_CH1` | 53 | `DMA_CH2` |
| 54 | `DMA_CH3` | 55 | `DMA_CH4` | 56 | `DMA_CH5` |

### L021

`HC32L021_DDL_Rev1.2.1/mcu/common/hc32l021.h`

| IRQ | 名称 | IRQ | 名称 | IRQ | 名称 |
|---:|---|---:|---|---:|---|
| 0 | `PORTA` | 1 | `PORTB` | 5 | `ATIM3` |
| 8 | `LPUART0` | 9 | `LPUART1` | 10 | `SPI` |
| 14 | `CTIM0` | 15 | `CTIM1` | 18 | `HSI2C` |
| 22 | `IWDT` | 23 | `RTC` | 24 | `ADC` |
| 26 | `VC0` | 27 | `VC1` | 28 | `LVD` |
| 30 | `FLASH` | 31 | `CTRIM_CLKDET` |  |  |

### L07x

`HC32L07x_DDL_Rev1.2.0/mcu/common/hc32l07x.h`

| IRQ | 名称 | IRQ | 名称 | IRQ | 名称 |
|---:|---|---:|---|---:|---|
| 0 | `PORTA` | 1 | `PORTB` | 2 | `PORTC_E` |
| 3 | `PORTD_F` | 4 | `DMAC` | 5 | `TIM3` |
| 6 | `UART0_2` | 7 | `UART1_3` | 8 | `LPUART0` |
| 9 | `LPUART1` | 10 | `SPI0_I2S0` | 11 | `SPI1_I2S1` |
| 12 | `I2C0` | 13 | `I2C1` | 14 | `TIM0` |
| 15 | `TIM1` | 16 | `TIM2` | 17 | `LPTIM_0_1` |
| 18 | `ADTIM4` | 19 | `ADTIM5` | 20 | `ADTIM6` |
| 21 | `PCA` | 22 | `WDT` | 23 | `RTC` |
| 24 | `ADC_DAC` | 25 | `PCNT` | 26 | `VC0_1_2_LVD` |
| 27 | `USBFS` | 28 | `CAN` | 29 | `LCD` |
| 30 | `FLASH_RAM` | 31 | `CLKTRIM_CTS` |  |  |

### L110

`HC32L110_DDL_Rev1.3.2/mcu/common/hc32l110.h`

| IRQ | 名称 | IRQ | 名称 | IRQ | 名称 |
|---:|---|---:|---|---:|---|
| 0 | `PORT0` | 1 | `PORT1` | 2 | `PORT2` |
| 3 | `PORT3` | 6 | `UART0` | 7 | `UART1` |
| 8 | `LPUART` | 10 | `SPI` | 12 | `I2C` |
| 14 | `TIM0` | 15 | `TIM1` | 16 | `TIM2` |
| 17 | `LPTIM` | 18 | `TIM4` | 19 | `TIM5` |
| 20 | `TIM6` | 21 | `PCA` | 22 | `WDT` |
| 23 | `RTC` | 24 | `ADC` | 26 | `VC0` |
| 27 | `VC1` | 28 | `LVD` | 30 | `FLASH_RAM` |
| 31 | `CLK_TRIM` |  |  |  |  |

### L12x

`HC32L12x_DDL_Rev1.1.0/mcu/common/hc32l12x.h`

| IRQ | 名称 | IRQ | 名称 | IRQ | 名称 |
|---:|---|---:|---|---:|---|
| 0 | `PORTA` | 1 | `PORTB` | 2 | `PORTC` |
| 3 | `PORTD` | 4 | `DMAC` | 5 | `TIM3` |
| 6 | `USART0` | 7 | `USART1` | 8 | `LPUART0` |
| 9 | `LPUART1` | 10 | `SPI0` | 11 | `SPI1` |
| 12 | `I2C0` | 13 | `I2C1` | 14 | `TIM0` |
| 15 | `TIM1` | 16 | `TIM2` | 17 | `LPTIM_0_1` |
| 18 | `HSI2C` | 19 | `LPUART2` | 21 | `WWDT` |
| 22 | `IWDT` | 23 | `RTC` | 24 | `ADC` |
| 26 | `VC0` | 27 | `VC1` | 28 | `LVD` |
| 29 | `LCD` | 30 | `FLASH_RAM` | 31 | `CTRIM_CLKDET` |

### L13x

`HC32L13x_DDL_Rev2.2.0/mcu/common/hc32l13x.h`

| IRQ | 名称 | IRQ | 名称 | IRQ | 名称 |
|---:|---|---:|---|---:|---|
| 0 | `PORTA` | 1 | `PORTB` | 2 | `PORTC` |
| 3 | `PORTD` | 4 | `DMAC` | 5 | `TIM3` |
| 6 | `UART0` | 7 | `UART1` | 8 | `LPUART0` |
| 9 | `LPUART1` | 10 | `SPI0` | 11 | `SPI1` |
| 12 | `I2C0` | 13 | `I2C1` | 14 | `TIM0` |
| 15 | `TIM1` | 16 | `TIM2` | 17 | `LPTIM` |
| 18 | `ADTIM4` | 19 | `ADTIM5` | 20 | `ADTIM6` |
| 21 | `PCA` | 22 | `WDT` | 23 | `RTC` |
| 24 | `ADC` | 25 | `PCNT` | 26 | `VC0` |
| 27 | `VC1` | 28 | `LVD` | 29 | `LCD` |
| 30 | `FLASH_RAM` | 31 | `CLKTRIM` |  |  |

### L16x

`HC32L16x_DDL_Rev1.1.1/mcu/common/HC32L166.h`

| IRQ | 名称 | IRQ | 名称 | IRQ | 名称 |
|---:|---|---:|---|---:|---|
| 0 | `PORTA` | 1 | `PORTB` | 2 | `PORTC_E` |
| 3 | `PORTD_F` | 4 | `DMAC` | 5 | `TIM3` |
| 6 | `UART0_2` | 7 | `UART1_3` | 8 | `LPUART0` |
| 9 | `LPUART1` | 10 | `SPI0` | 11 | `SPI1` |
| 12 | `I2C0` | 13 | `I2C1` | 14 | `TIM0` |
| 15 | `TIM1` | 16 | `TIM2` | 17 | `LPTIM_0_1` |
| 18 | `ADTIM4` | 19 | `ADTIM5` | 20 | `ADTIM6` |
| 21 | `PCA_WWDT` | 22 | `WDT` | 23 | `RTC` |
| 24 | `ADC_DAC` | 25 | `PCNT` | 26 | `VC0` |
| 27 | `VC1_2` | 28 | `LVD` | 29 | `LCD` |
| 30 | `FLASH_RAM` | 31 | `CLKTRIM` |  |  |

### L17x

`HC32L17x_DDL_Rev1.4.0/mcu/common/hc32l17x.h`

| IRQ | 名称 | IRQ | 名称 | IRQ | 名称 |
|---:|---|---:|---|---:|---|
| 0 | `PORTA` | 1 | `PORTB` | 2 | `PORTC_E` |
| 3 | `PORTD_F` | 4 | `DMAC` | 5 | `TIM3` |
| 6 | `UART0_2` | 7 | `UART1_3` | 8 | `LPUART0` |
| 9 | `LPUART1` | 10 | `SPI0` | 11 | `SPI1` |
| 12 | `I2C0` | 13 | `I2C1` | 14 | `TIM0` |
| 15 | `TIM1` | 16 | `TIM2` | 17 | `LPTIM_0_1` |
| 18 | `ADTIM4` | 19 | `ADTIM5` | 20 | `ADTIM6` |
| 21 | `PCA` | 22 | `WDT` | 23 | `RTC` |
| 24 | `ADC_DAC` | 25 | `PCNT` | 26 | `VC0` |
| 27 | `VC1_2` | 28 | `LVD` | 29 | `LCD` |
| 30 | `FLASH_RAM` | 31 | `CLKTRIM` |  |  |

### L18x

`hc32l186_ddl/mcu/common/HC32L186.h`

| IRQ | 名称 | IRQ | 名称 | IRQ | 名称 |
|---:|---|---:|---|---:|---|
| 0 | `PORTA` | 1 | `PORTB` | 2 | `PORTC_E` |
| 3 | `PORTD_F` | 4 | `DMAC` | 5 | `TIM3` |
| 6 | `UART0_2` | 7 | `UART1_3` | 8 | `LPUART0` |
| 9 | `LPUART1` | 10 | `SPI0` | 11 | `SPI1` |
| 12 | `I2C0` | 13 | `I2C1` | 14 | `TIM0` |
| 15 | `TIM1` | 16 | `TIM2` | 17 | `LPTIM_0_1` |
| 18 | `ADTIM4` | 19 | `ADTIM5` | 20 | `ADTIM6` |
| 21 | `PCA_WWDT` | 22 | `WDT` | 23 | `RTC` |
| 24 | `ADC_DAC` | 25 | `PCNT` | 26 | `VC0` |
| 27 | `VC1_2` | 28 | `LVD` | 29 | `LCD` |
| 30 | `FLASH_RAM` | 31 | `CLKTRIM` |  |  |

### L19x

`HC32L19x_DDL_Rev1.4.0/mcu/common/hc32l19x.h`

| IRQ | 名称 | IRQ | 名称 | IRQ | 名称 |
|---:|---|---:|---|---:|---|
| 0 | `PORTA` | 1 | `PORTB` | 2 | `PORTC_E` |
| 3 | `PORTD_F` | 4 | `DMAC` | 5 | `TIM3` |
| 6 | `UART0_2` | 7 | `UART1_3` | 8 | `LPUART0` |
| 9 | `LPUART1` | 10 | `SPI0` | 11 | `SPI1` |
| 12 | `I2C0` | 13 | `I2C1` | 14 | `TIM0` |
| 15 | `TIM1` | 16 | `TIM2` | 17 | `LPTIM_0_1` |
| 18 | `ADTIM4` | 19 | `ADTIM5` | 20 | `ADTIM6` |
| 21 | `PCA` | 22 | `WDT` | 23 | `RTC` |
| 24 | `ADC_DAC` | 25 | `PCNT` | 26 | `VC0` |
| 27 | `VC1_2` | 28 | `LVD` | 29 | `LCD` |
| 30 | `FLASH_RAM` | 31 | `CLKTRIM` |  |  |
