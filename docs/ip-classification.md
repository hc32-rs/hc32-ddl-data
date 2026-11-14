# HC32 DDL 外设 IP 分类

覆盖 19 个 Cortex-M0/M0+ 系列和 Cortex-M4 的 HC32F420。HC32F420 使用旧式 DDL 接口，时钟、GPIO 等仍与本仓库若干 M0/M0+ 系列同族。

[布局编号与功能档位](layout-versions.md)列出已重排的分组和依据。生成寄存器数据前需比较寄存器偏移、
访问属性和位域；基址、中断、时钟门控与通道数按系列或实例记录。

| 外设组 | 本仓库分类 |
|---|---|
| [模拟](explore/analog.md) | ADC、DAC、OPA、VC、BGR 与 HDIV。F420 增补 `adc_v3` 和 `vc_v3_2`。 |
| [时钟、复位与校准](explore/clock-reset.md) | SYSCTRL、RESET、CLK_TRIM、CTRIM、VCTRIM。F420 增补 `sysctrl_v14` 和 `ctrim_v6`。 |
| [电源、低功耗与看门狗](explore/power-wdt.md) | LVD 分 lvd_v1/v2，WDT 分 wdt_v1/v2（L110 的字段别名另记）；IWDT 和 WWDT 各分 v1/v2；L110 另有 lpt_v1。 |
| [GPIO、中断与调试](explore/gpio-int.md) | GPIO 分 gpio_v1–v3；DEBUG_ACTIVE 归 dbg_v1。IRQ 取自设备头文件，L110 有独立兼容头。 |
| [UART、LPUART、USART 与 LIN](explore/serial.md) | UART 分 uart_v1–v3；LPUART 分 lpuart_v1–v5、v7；A052/F052/L12x/F420 的 USART 为 `usart_v1`。LIN 是 USART 的模式。 |
| [SPI、I2C 与 I2S](explore/spi-i2c-i2s.md) | SPI 的 CR/SSN 布局为 `spi_v1`、`spi_v1_1`，CR0/CR1 布局为 `spi_v2`；I2C 为 i2c_v1–v3；另有 I2CSLV、HSI2C、I2S。F420 的 SPI/I2C 分属 `spi_v2`、`i2c_v3`。 |
| [高级定时器与紧急刹车](explore/timer-adv.md) | ADT 为 adt_v1–v3；ATIM 按模式视图分类。F420 的候选为 `atim_1ch_v3`、`atim_v8`。 |
| [通用、基本与低功耗定时器](explore/timer-gen.md) | TIMER3、GTIM、BTIM 与 LPTIM。F420 的 CTIM0–2 各复用一组 GTIM 和 BTIM。 |
| [PCA、BT、PCNT、AOS 与 EAU](explore/pca-bt-pcnt-aos.md) | PCA 分为 `pca_v1` 和 `pca_v1_1`，BT 为 bt_v1–v5，PCNT 为 `pcnt_v1` 和 `pcnt_v1_1`，A052/F052 的 EAU 为 eau_v1；BTIM 与定时器组共用分类。 |
| [DMA](explore/dma.md) | 旧式 DMAC 为 dma_v1–v3；通道数和触发编号按系列登记。A052/F052 的设备头和 SVD 描述六通道，DDL API 支持五通道。F420 六通道均由驱动支持。 |
| [Flash、RAM 与 SRAM](explore/memory.md) | F420 的 `flash_v7_1` 是 A052/F052 紧凑 OP 布局的 CACHE 变体。RAM 错误控制为 `ram_v1`–`ram_v4`。存储容量与地址另取链接脚本。 |
| [CRC、AES、TRNG、RTC、LCD 与 CAN](explore/crypto-comm.md) | CRC 有 v1、v1_csr、v1_sel、v1_min；AES、TRNG、RTC、LCD 和 CAN 的系列划分见对应页。 |

## 已登记的其他外设

F072 和 L07x 的设备头与 SVD 均包含 USBFS（`0x40040000`，IRQ 27）及 CTS
（`0x40005000`，与时钟校准共享 IRQ 31）。两系列对应块的寄存器名、偏移和位域一致，
已记录存在性，版本命名与驱动行为待核对。

[SDK 核验](sdk-verification.md)列出核对范围和 SDK 证据。
