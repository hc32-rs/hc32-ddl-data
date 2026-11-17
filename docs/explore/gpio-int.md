# GPIO、中断与调试

GPIO 的输入专用端口、精简端口与通用复用控制分档。逐引脚 SEL、AFR 端口和混合布局是不同分支，
同档不表示兼容。GPIOA/B/C/D 派生实例统一使用 gpio 目标名，GPIOAUX 保持独立窗口。
DEBUG_ACTIVE 使用 debug_active 名称，其停机目标位图分变体。

F420 GPIOA–D 固定 IRQ 为 3–6，门控 AHB1EN bit 0–3。
GPIOAUX 与 SYSCTRL 共用 0x4000B800 基址，寄存器位于 +0x70/+0x74/+0x78。
相对 A052/F052，CR0 分成 PCLK0/PCLK1，CR1/CR2 的触发/捕获目标也不同。
`driver/inc/gpio.h` 的 GPIO_PA00_AF_ATIM0_ETR_SET 等宏直接写 AFRL/AFRH。
IRQ 始终按设备头 IRQn_Type 记录，不从版本号推断。

[编号规则](../layout-versions.md) · [实例 CSV](../peripheral-versions.csv) · [SDK 核验](../sdk-verification.md)。

<!-- peripheral-versions:begin -->
## 版本与系列对照

以下名称与生成输入保持一致。系列名前缀 `HC32` 省略。

### debug_active：v1

单寄存器调试活动控制，停机目标位分变体。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `debug_active_ddl_v1.rs` | [debug_active_ddl_v1](../../data/registers/debug_active_ddl_v1.yaml) | F003/F005/F030/F072: DEBUG_ACTIVE | 1 / 10 | 本档位对照基准；完整定义见来源 YAML |
| `debug_active_ddl_v1_1.rs` | [debug_active_ddl_v1_1](../../data/registers/debug_active_ddl_v1_1.yaml) | L12x: DEBUG_ACTIVE | 1 / 10 | 相对 debug_active_ddl_v1：定义差异 DEBUG_ACTIVE |
| `debug_active_ddl_v1_2.rs` | [debug_active_ddl_v1_2](../../data/registers/debug_active_ddl_v1_2.yaml) | A136/L13x: DEBUG_ACTIVE | 1 / 12 | 相对 debug_active_ddl_v1：定义差异 DEBUG_ACTIVE |
| `debug_active_ddl_v1_3.rs` | [debug_active_ddl_v1_3](../../data/registers/debug_active_ddl_v1_3.yaml) | F17x/F19x/L07x/L17x/L19x: DEBUG_ACTIVE | 1 / 13 | 相对 debug_active_ddl_v1：定义差异 DEBUG_ACTIVE |
| `debug_active_ddl_v1_4.rs` | [debug_active_ddl_v1_4](../../data/registers/debug_active_ddl_v1_4.yaml) | L16x/L18x: DEBUG_ACTIVE | 1 / 14 | 相对 debug_active_ddl_v1：定义差异 DEBUG_ACTIVE |

### gpio：v1

输入专用端口，保留选择与中断控制。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `gpio_ddl_v1.rs` | [gpio_ddl_v1](../../data/registers/gpio_ddl_v1.yaml) | L021: GPIOB | 8 / 16 | 本档位对照基准；完整定义见来源 YAML |

### gpio：v2

精简端口与逐引脚复用。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `gpio_ddl_v2.rs` | [gpio_ddl_v2](../../data/registers/gpio_ddl_v2.yaml) | F003/F005/L110: GPIO | 77 / 270 | 本档位对照基准；完整定义见来源 YAML |

### gpio：v3

通用输入输出与复用；端口阵列、AFR 和逐引脚 SEL 为同档分支。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `gpio_ddl_v3.rs` | [gpio_ddl_v3](../../data/registers/gpio_ddl_v3.yaml) | F002: GPIOA, GPIOB, GPIOC, GPIOD | 15 / 142 | 本档位对照基准；完整定义见来源 YAML |
| `gpio_ddl_v3_1.rs` | [gpio_ddl_v3_1](../../data/registers/gpio_ddl_v3_1.yaml) | A052/F052/F420: GPIOA, GPIOB, GPIOC, GPIOD | 17 / 301 | 相对 gpio_ddl_v3：增加 AFRH/DRIVER/PUPD；缺少 PU；定义差异 ADS/BRR/BSRR/DIR/FALLIE 等 13 项 |
| `gpio_ddl_v3_2.rs` | [gpio_ddl_v3_2](../../data/registers/gpio_ddl_v3_2.yaml) | L021: GPIOA | 31 / 302 | 相对 gpio_ddl_v3：增加 BCLR/BSET/BSETCLR/FIE/HIE 等 26 项；缺少 AFRL/BRR/BSRR/FALLIE/HIGHIE 等 10 项；定义差异 ADS/DIR/IN/OUT/PU |
| `gpio_ddl_v3_3.rs` | [gpio_ddl_v3_3](../../data/registers/gpio_ddl_v3_3.yaml) | A136/F030/L13x: GPIO | 131 / 1113 | 相对 gpio_ddl_v3：增加 CTRL0/CTRL1/CTRL2/PA00_SEL/PA01_SEL 等 131 项；缺少 ADS/AFRL/BRR/BSRR/DIR 等 15 项 |
| `gpio_ddl_v3_4.rs` | [gpio_ddl_v3_4](../../data/registers/gpio_ddl_v3_4.yaml) | L12x: GPIO | 131 / 1119 | 相对 gpio_ddl_v3：增加 CTRL1/CTRL2/FLTCFG/PA00_SEL/PA01_SEL 等 131 项；缺少 ADS/AFRL/BRR/BSRR/DIR 等 15 项 |
| `gpio_ddl_v3_5.rs` | [gpio_ddl_v3_5](../../data/registers/gpio_ddl_v3_5.yaml) | L16x/L18x: GPIO | 180 / 1425 | 相对 gpio_ddl_v3：增加 CTRL1/CTRL2/PA00_SEL/PA01_SEL/PA02_SEL 等 180 项；缺少 ADS/AFRL/BRR/BSRR/DIR 等 15 项 |
| `gpio_ddl_v3_6.rs` | [gpio_ddl_v3_6](../../data/registers/gpio_ddl_v3_6.yaml) | F072: GPIO | 201 / 1797 | 相对 gpio_ddl_v3：增加 CTRL0/CTRL1/CTRL2/PA00_SEL/PA01_SEL 等 201 项；缺少 ADS/AFRL/BRR/BSRR/DIR 等 15 项 |
| `gpio_ddl_v3_7.rs` | [gpio_ddl_v3_7](../../data/registers/gpio_ddl_v3_7.yaml) | F17x/F19x/L07x/L17x/L19x: GPIO | 202 / 1804 | 相对 gpio_ddl_v3：增加 CTRL0/CTRL1/CTRL2/PA00_SEL/PA01_SEL 等 202 项；缺少 ADS/AFRL/BRR/BSRR/DIR 等 15 项 |

### gpioaux：v1

GPIO 辅助复用/触发控制；目标阵容不同，保留独立变体。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `gpioaux_ddl_v1.rs` | [gpioaux_ddl_v1](../../data/registers/gpioaux_ddl_v1.yaml) | F002: GPIOAUX | 2 / 6 | 本档位对照基准；完整定义见来源 YAML |
| `gpioaux_ddl_v1_1.rs` | [gpioaux_ddl_v1_1](../../data/registers/gpioaux_ddl_v1_1.yaml) | A052/F052: GPIOAUX | 3 / 14 | 相对 gpioaux_ddl_v1：增加 CR0/CR2；缺少 CR4；定义差异 CR1 |
| `gpioaux_ddl_v1_2.rs` | [gpioaux_ddl_v1_2](../../data/registers/gpioaux_ddl_v1_2.yaml) | F420: GPIOAUX | 3 / 15 | 相对 gpioaux_ddl_v1：增加 CR0/CR2；缺少 CR4；定义差异 CR1 |
| `gpioaux_ddl_v1_3.rs` | [gpioaux_ddl_v1_3](../../data/registers/gpioaux_ddl_v1_3.yaml) | L021: GPIOAUX | 5 / 20 | 相对 gpioaux_ddl_v1：增加 CTRL1/CTRL2/FLTCFG/TIMCPS/TIMES；缺少 CR1/CR4 |
<!-- peripheral-versions:end -->

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
