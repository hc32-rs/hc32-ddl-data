# SDK 核验

[SVD 总览](svd-overview.md)列出 20 个系列及归档 SVD 的 SDK 路径。每个归档文件均与相应
SDK 文件内容一致（行尾归一为 LF），驱动矩阵的 52 个头文件名及 550 条系列记录已与 `driver/inc/` 全量比对。
各组的实例地址表按 SVD 生成，并核对了 869 个实例地址条目。各系列外部 IRQ 取自 20 份
SDK 设备头的 `IRQn_Type`。F420 的 IRQ 来自 `mcu/common/HC32F420.h`，门控取
`driver/inc/sysctrl.h` 和 `driver/src/sysctrl.c`，DMA 请求取 `driver/inc/dmac.h`。
A136 模拟寄存器共享基址、A052/F052 的 DMA 通道 API 覆盖差异，以及 F420 的布局差异见对应分类页。

## 核验范围

SDK 路径以下均相对于工作区的 `xh_ddl_sdk/`。F420 的包目录为 `HC32F420_DDL_Rev1.3.1/`。

| 外设组 | 核验结果 | SDK 证据 |
|---|---|---|
| [模拟](explore/analog.md) | A136 的 ADC、OPA 等块共用基址。F420 ADC 外部输入 16 路，内部输入为 AVCC/3 与温度传感器，ADC0/1 共用向量。 | A136 的 `mcu/common/hc32a136.h`。F420 的 `driver/inc/adc.h`、`driver/inc/dmac.h`。 |
| [时钟、复位与校准](explore/clock-reset.md) | F420 SYSCTRL 有 21 个寄存器，四组门控。L110 RESET 的 PREI_RESET 命名同时出现在 SVD 和设备头。 | F420 的 `driver/src/sysctrl.c`。L110 的 `mcu/common/hc32l110.h`。 |
| [电源与看门狗](explore/power-wdt.md) | F420 LVD 位域与 F002 相同，IWDT/WWDT 的寄存器名、偏移及位域与 F052 相同。 | F420、F002、F052 的 SVD，F420 设备头的 IRQn_Type。 |
| [GPIO 与中断](explore/gpio-int.md) | F420 端口块与 A052/F052 一致，GPIOAUX 有分频和定时器字段差异。57 个外部中断号覆盖 0–56。 | F420 的 `mcu/common/HC32F420.h`、`driver/inc/gpio.h` 及三份 SVD。 |
| [串口](explore/serial.md) | F420 四个 USART 的寄存器名、偏移和位域与 A052/F052 一致，每实例三条固定 IRQ。 | F420 的 `mcu/common/HC32F420.h`、`example/usart/usart_lin_master/` 及三份 SVD。 |
| [SPI、I2C 与 I2S](explore/spi-i2c-i2s.md) | F420 SPI0/1 归 `spi_v2`。I2C0/1 与 F002 的 BRREN/BRR/DR 布局一致，归 `i2c_v3`。 | F420 的 `driver/inc/spi.h`、`driver/inc/i2c.h`，F420、F002、F052、L12x 的 SVD。 |
| [高级定时器](explore/timer-adv.md) | F420 ATIM0 MODE23 有 CA3 定义，ATIM3 MODE1.CR0 有 CSA/CSB，各实例有四条固定 IRQ。 | F420 的 `mcu/common/HC32F420.h`、`driver/inc/atim0.h`，对比 F002、A052/F052 SVD。 |
| [通用定时器](explore/timer-gen.md) | F420 三组 CTIM 以 SYSCTRL 选择 GTIM/BTIM，按组共用中断；BTIM 字段按 bank 编号。 | F420 的 `driver/inc/sysctrl.h`、`mcu/common/HC32F420.h`，对比 A052/F052/F002 SVD。 |
| [PCA、BT、PCNT、AOS 与 EAU](explore/pca-bt-pcnt-aos.md) | F420 未列 PCA、PCNT 或 AOS。BTIM/GTIM 地址窗口与通用定时器组共用。 | F420 的 SVD 与 `driver/inc/`。 |
| [DMA](explore/dma.md) | 核对了 15 个有 DMAC 的系列的通道寄存器组、CONF.ST 和触发字段位宽。A052/F052 的 SVD 和设备头描述六组，驱动只接受通道 0–4。F420 驱动支持六通道。 | 各系列 DMAC SVD；A052/F052/F420 的 `mcu/common/` 和 `driver/inc/dmac.h`，A052/F052 的 `driver/src/dmac.c`。 |
| [Flash 与 RAM](explore/memory.md) | F420 的 CACHE 位于 +0x0C，使后续四个寄存器偏移改变。例程为 128 KiB Flash、24 KiB RAM。 | F420、A052/F052 SVD；F420 的 `example/flash/flash_erase_write/EWARM/hc32f420.icf`。 |
| [CRC、AES、TRNG、RTC、LCD 与 CAN](explore/crypto-comm.md) | F420 CRC 的 BUSY 是 SDK 驱动轮询位，DR 与 A052/F052 的 DATA 偏移一致。 | F420 的 `driver/src/crc.c` 及 F420、A052/F052 SVD。 |

M0/M0+ 的版本编号沿用已有聚类。核对涵盖 SDK 归属、实例存在性和表中列出的差异。
复位值、封装引脚表和各系列例程行为仍需按生成目标逐项确认。
