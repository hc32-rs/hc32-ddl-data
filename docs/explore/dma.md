# DMA

## Cortex-M0/M0+

旧式 DMAC 为 dma_v1–v3；通道数和触发编号按系列登记。A052/F052 的 SVD 与设备头都描述通道 0–5，
两份 `driver/inc/dmac.h` 的 `DmaCh` 枚举及 `driver/src/dmac.c` 的参数检查都只覆盖 0–4。
用六组寄存器描述寄存器映射，驱动 API 的支持范围另记。A052 的 `dmac.h` 已列入驱动矩阵。

| 建议版本 | 系列或实例 | 布局备注 |
|---|---|---|
| `dma_v1` | A136 F030 L13x | 2 通道，CONF 有 ST，TRI_SEL 为 [28:23]。 |
| `dma_v2` | F072 F17x F19x L07x L17x L19x | 2 通道，CONF 有 ST，TRI_SEL 为 [28:22]。 |
| `dma_v3` | A052 F052 L12x L16x L18x F420 | A052/F052/F420 6 组，L12x/L16x/L18x 2 组。CONF 无 ST，触发选择字段为 [28:22]。 |

## HC32F420（Cortex-M4）

沿用 `dma_v3`，六通道固定向量 51–56，DMAC 基址 `0x40020000`。
F420 的 `CONFAx.TRI_SEL[28:22]` 和 `CONFBx.FIS_IE/ERR_IE` 在 A052/F052 的 SVD 中分别名为
`TRISEL`、`FISIE/ERRIE`，位位置和宽度相同。`driver/inc/dmac.h` 的通道枚举覆盖 0–5，门控为 AHB0EN bit 0。

| F420 请求源 | 触发值 |
|---|---|
| 软件 | `0x00` |
| USART0–3 TX/RX（交错） | `0x40–0x47` |
| GTIM0–2 | `0x48–0x4A` |
| ADC0/1 顺序单次转换完成 | `0x4D/0x4F` |
| SPI0/1 TX/RX（交错） | `0x50–0x53` |
| ATIM0 A/B、ATIM3 A/B | `0x58/0x59`、`0x5E/0x5F` |

请求编号以各系列的 `driver/inc/dmac.h` 为准。14 个带 DMAC 的 DDL M0/M0+ 系列
在设备头使用共享 `DMAC_IRQn=4`。L12x 的通道 1 地址寄存器在 SVD 和设备头中均写作
`SRCADR/DSTADR`，偏移为 `0x28/0x2C`。

## SVD 实例与基址

表中使用 SVD 的实例名与基址。SDK 目录与文件来源见 [SVD 总览](../svd-overview.md)。

| 系列 | SVD 实例与基址 |
|---|---|
| A052 | `DMAC@0x40021000` |
| A136 | `DMAC@0x40021000` |
| F030 | `DMAC@0x40021000` |
| F052 | `DMAC@0x40021000` |
| F072 | `DMAC@0x40021000` |
| F17x | `DMAC@0x40021000` |
| F19x | `DMAC@0x40021000` |
| F420 | `DMAC@0x40020000` |
| L07x | `DMAC@0x40021000` |
| L12x | `DMAC@0x40021000` |
| L13x | `DMAC@0x40021000` |
| L16x | `DMAC@0x40021000` |
| L17x | `DMAC@0x40021000` |
| L18x | `DMAC@0x40021000` |
| L19x | `DMAC@0x40021000` |
