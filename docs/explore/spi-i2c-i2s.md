# SPI、I2C 与 I2S

SPI 的 CR/SSN 基础布局和增加 CR2@0x10、ICLR@0x14 的扩展使用同档后缀。
CR0/CR1/HDOE/SSI 架构另设主档位；F002 无 SMP/DMA，L021 无 DMA，分别保留布局变体。
I2C 从单地址到多地址匹配分档，L110 的字段名、ADDR1/2 命名与 STAT 差异不自动合并。
F002/F420 的 BRREN/BRR/DR 布局与 TMRUN/TM/DATA 分支使用不同后缀。

I2CSLV 是专用从机控制块。HSI2C 的 L021 基础布局有 23 个寄存器，L12x 增加
MDER@0x4C、SDER@0x13C；不再采用“optional 字段即可合并”的建议。
F072/L07x 的 I2S 为 CR/SR/CFGR/PR/ICR/DRL/DRR，IRQ 与 SPI 共用。
旧生成 kind=i/hsi 被截断，当前文件名恢复为 i2c、i2cslv、i2s、hsi2c。

F420 I2C0/1 的 IRQ 为 24/25，APB0EN bit 10/11；DMAC 未列其请求。
SPI0 的 TX/RX/SSR/ERR 为 IRQ 26–29，SPI1 为 30–33，门控 bit 14/15。
DMA 请求分别为 0x50/0x51 与 0x52/0x53。
入口见 `driver/inc/spi.h` 的 Spi_Init 和 `driver/inc/i2c.h` 的 I2C_MasterInit/I2C_SlaveInit。

[编号规则](../layout-versions.md) · [实例 CSV](../peripheral-versions.csv) · [SDK 核验](../sdk-verification.md)。

<!-- peripheral-versions:begin -->
## 版本与系列对照

以下名称与生成输入保持一致。系列名前缀 `HC32` 省略。

### hsi2c：v1

高速主从 I2C；MDER/SDER DMA 扩展分变体。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `hsi2c_ddl_v1.rs` | [hsi2c_ddl_v1](../../data/registers/hsi2c_ddl_v1.yaml) | L021: HSI2C | 23 / 135 | 本档位对照基准；完整定义见来源 YAML |
| `hsi2c_ddl_v1_1.rs` | [hsi2c_ddl_v1_1](../../data/registers/hsi2c_ddl_v1_1.yaml) | L12x: HSI2C | 25 / 142 | 相对 hsi2c_ddl_v1：增加 MDER/SDER；定义差异 MSCR/SSCR |

### i2c：v1

基础单地址 I2C。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `i2c_ddl_v1.rs` | [i2c_ddl_v1](../../data/registers/i2c_ddl_v1.yaml) | L110: I2C | 6 / 12 | 本档位对照基准；完整定义见来源 YAML |
| `i2c_ddl_v1_1.rs` | [i2c_ddl_v1_1](../../data/registers/i2c_ddl_v1_1.yaml) | A136/F030/F072/F17x/F19x/L07x/L13x/L17x/L19x: I2C0, I2C1 | 6 / 18 | 相对 i2c_ddl_v1：定义差异 ADDR/CR/DATA/STAT/TM 等 6 项 |
| `i2c_ddl_v1_2.rs` | [i2c_ddl_v1_2](../../data/registers/i2c_ddl_v1_2.yaml) | F003/F005: I2C | 6 / 18 | 相对 i2c_ddl_v1：定义差异 ADDR/CR/DATA/STAT/TM 等 6 项 |

### i2c：v2

多地址匹配，命名及字段分变体。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `i2c_ddl_v2.rs` | [i2c_ddl_v2](../../data/registers/i2c_ddl_v2.yaml) | A052/F052: I2C0, I2C1 | 9 / 26 | 本档位对照基准；完整定义见来源 YAML |
| `i2c_ddl_v2_1.rs` | [i2c_ddl_v2_1](../../data/registers/i2c_ddl_v2_1.yaml) | F002: I2C | 9 / 26 | 相对 i2c_ddl_v2：增加 BRR/BRREN/DR；缺少 DATA/TM/TMRUN；定义差异 CR |
| `i2c_ddl_v2_2.rs` | [i2c_ddl_v2_2](../../data/registers/i2c_ddl_v2_2.yaml) | F420: I2C0, I2C1 | 9 / 26 | 相对 i2c_ddl_v2：增加 BRR/BRREN/DR；缺少 DATA/TM/TMRUN；定义差异 CR |
| `i2c_ddl_v2_3.rs` | [i2c_ddl_v2_3](../../data/registers/i2c_ddl_v2_3.yaml) | L12x/L16x/L18x: I2C0, I2C1 | 9 / 26 | 相对 i2c_ddl_v2：定义差异 ADDR1/ADDR2/STAT |

### i2cslv：v1

专用从机 I2C。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `i2cslv_ddl_v1.rs` | [i2cslv_ddl_v1](../../data/registers/i2cslv_ddl_v1.yaml) | A052/F052: I2CSLV | 17 / 96 | 本档位对照基准；完整定义见来源 YAML |

### i2s：v1

音频串行控制；位域分变体。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `i2s_ddl_v1.rs` | [i2s_ddl_v1](../../data/registers/i2s_ddl_v1.yaml) | F072/L07x: I2S0, I2S1 | 7 / 32 | 本档位对照基准；完整定义见来源 YAML |

### spi：v1

CR/SSN 基础及 CR2 扩展。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `spi_ddl_v1.rs` | [spi_ddl_v1](../../data/registers/spi_ddl_v1.yaml) | L110: SPI | 4 / 13 | 本档位对照基准；完整定义见来源 YAML |
| `spi_ddl_v1_1.rs` | [spi_ddl_v1_1](../../data/registers/spi_ddl_v1_1.yaml) | F003/F005: SPI | 4 / 17 | 相对 spi_ddl_v1：定义差异 CR/DATA/SSN/STAT |
| `spi_ddl_v1_2.rs` | [spi_ddl_v1_2](../../data/registers/spi_ddl_v1_2.yaml) | A136/F030/F072/F17x/F19x/L07x/L13x/L16x/L17x/L18x/L19x: SPI0, SPI1 | 6 / 27 | 相对 spi_ddl_v1：增加 CR2/ICLR；定义差异 CR/DATA/SSN/STAT |

### spi：v2

CR0/CR1、HDOE/SSI 控制，DMA/SMP 分变体。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `spi_ddl_v2.rs` | [spi_ddl_v2](../../data/registers/spi_ddl_v2.yaml) | F002: SPI | 7 / 45 | 本档位对照基准；完整定义见来源 YAML |
| `spi_ddl_v2_1.rs` | [spi_ddl_v2_1](../../data/registers/spi_ddl_v2_1.yaml) | L021: SPI | 7 / 46 | 相对 spi_ddl_v2：定义差异 CR0 |
| `spi_ddl_v2_2.rs` | [spi_ddl_v2_2](../../data/registers/spi_ddl_v2_2.yaml) | A052/F052/F420/L12x: SPI0, SPI1 | 7 / 48 | 相对 spi_ddl_v2：定义差异 CR0/CR1 |
<!-- peripheral-versions:end -->

## SVD 实例与基址

表中使用 SVD 的实例名与基址。SDK 目录与文件来源见 [SVD 总览](../svd-overview.md)。

| 系列 | SVD 实例与基址 |
|---|---|
| A052 | `I2C0@0x40000400`, `I2C1@0x40004400`, `I2CSLV@0x40004C00`, `SPI0@0x40000800`, `SPI1@0x40004800` |
| A136 | `I2C0@0x40000400`, `I2C1@0x40004400`, `SPI0@0x40000800`, `SPI1@0x40004800` |
| F002 | `I2C@0x40000400`, `SPI@0x40000800` |
| F003 | `I2C@0x40000400`, `SPI@0x40000800` |
| F005 | `I2C@0x40000400`, `SPI@0x40000800` |
| F030 | `I2C0@0x40000400`, `I2C1@0x40004400`, `SPI0@0x40000800`, `SPI1@0x40004800` |
| F052 | `I2C0@0x40000400`, `I2C1@0x40004400`, `I2CSLV@0x40004C00`, `SPI0@0x40000800`, `SPI1@0x40004800` |
| F072 | `I2C0@0x40000400`, `I2C1@0x40004400`, `I2S0@0x40002800`, `I2S1@0x40002C00`, `SPI0@0x40000800`, `SPI1@0x40004800` |
| F17x | `I2C0@0x40000400`, `I2C1@0x40004400`, `SPI0@0x40000800`, `SPI1@0x40004800` |
| F19x | `I2C0@0x40000400`, `I2C1@0x40004400`, `SPI0@0x40000800`, `SPI1@0x40004800` |
| F420 | `I2C0@0x40002800`, `I2C1@0x40002C00`, `SPI0@0x40003800`, `SPI1@0x40003C00` |
| L021 | `HSI2C@0x4000BC00`, `SPI@0x40000800` |
| L07x | `I2C0@0x40000400`, `I2C1@0x40004400`, `I2S0@0x40002800`, `I2S1@0x40002C00`, `SPI0@0x40000800`, `SPI1@0x40004800` |
| L110 | `I2C@0x40000400`, `SPI@0x40000800` |
| L12x | `HSI2C@0x4000BC00`, `I2C0@0x40000400`, `I2C1@0x40008400`, `SPI0@0x40000800`, `SPI1@0x40008800` |
| L13x | `I2C0@0x40000400`, `I2C1@0x40004400`, `SPI0@0x40000800`, `SPI1@0x40004800` |
| L16x | `I2C0@0x40000400`, `I2C1@0x40004400`, `SPI0@0x40000800`, `SPI1@0x40004800` |
| L17x | `I2C0@0x40000400`, `I2C1@0x40004400`, `SPI0@0x40000800`, `SPI1@0x40004800` |
| L18x | `I2C0@0x40000400`, `I2C1@0x40004400`, `SPI0@0x40000800`, `SPI1@0x40004800` |
| L19x | `I2C0@0x40000400`, `I2C1@0x40004400`, `SPI0@0x40000800`, `SPI1@0x40004800` |
