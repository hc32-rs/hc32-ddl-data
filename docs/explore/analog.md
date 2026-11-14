# 模拟

## 功能档位

ADC 按基础转换、序列控制及结果读出能力分档。`adc_v7_1` 在 `adc_v7` 的 SQR/JQR 布局上增加 `ALLSTART@0xCC`，并有 CHMAP 和 RTC 触发位差异。

A136 的 SDK 设备头 `HC32A136_DDL_Rev2.1.0/mcu/common/hc32a136.h` 与 SVD 一致，
ADC、OPA、VC、LVD、BGR 的基址都是 `0x40002400`。各块的寄存器按偏移拆开，
如 BGR.CR 在 `+0x00`、ADC.CR0 在 `+0x04`、VC.CR 在 `+0x10`、
LVD.CR 在 `+0x28`、OPA.CR0 在 `+0x30`。

| 建议版本 | 系列或实例 | 布局备注 |
|---|---|---|
| `adc_v1` | F002 | 9 寄存器最简型 |
| `adc_v2` | L021 | 11 寄存器 4 通道 |
| `adc_v3` | F420 | 16 寄存器，内嵌 BGR、SQR 与单个 RESULT。 |
| `adc_v4` | F003 F005 L110 | 18 寄存器早期基本型（RESULT0..8） |
| `adc_v5` | A052 F052 | 27 寄存器、4 rank、块首内嵌 BGR 寄存器 |
| `adc_v6` | L12x | 28 寄存器 6 rank + 内嵌 BGR |
| `adc_v7` | A136 F030 L13x | 37 个寄存器，无 ALLSTART。 |
| `adc_v7_1` | F072 L07x F17x F19x L16x L17x L18x L19x | 38 个寄存器，包含 16 个 SQR 结果寄存器、JQR 和 ALLSTART。 |
| `dac_v1` | F17x F19x L16x L17x L18x L19x | 8 寄存器单通道 12bit |
| `dac_v2` | F072 L07x | 15 寄存器双通道 |
| `opa_v1` | A136 F030 L13x | 3 运放、每运放一套 CR |
| `opa_v2` | F072 L07x | 5 运放阵列位段 |
| `opa_v3` | F17x F19x L17x L19x | 单运放+ADC 触发 |
| `opa_v4` | L16x L18x | 单运放精简 |
| `opa_v5` | A052 F052 | 2 运放 + AZ |
| `vc_v1` | F072 F17x F19x L07x L16x L17x L18x L19x | 8 寄存器 3 比较器 |
| `vc_v2` | A136 F030 L13x | 6 寄存器 2 比较器（OUT_CFG→TIMxRCLR/TIMBK） |
| `vc_v2_1` | F003 F005 | 同上，OUT_CFG→PCA 系 |
| `vc_v2_2` | L110 | 同上，OUT_CFG 目标再多 TIM3G/TIM3ECLK |
| `vc_v3` | A052 F052 | VC0/VC1 独立实例（derivedFrom），4 寄存器 |
| `vc_v3_1` | L12x | 同上，字段命名差异 |
| `vc_v4` | L021 | 9 寄存器含 VC_TRIM |
| `bgr_v1` | A136 F003 F005 F030 F072 F17x F19x L07x L110 L13x L16x L17x L18x L19x | 单寄存器 |
| `hdiv_v1` | A136 F030 F072 L07x L13x | 6 寄存器 |

## HC32F420（Cortex-M4）

ADC0/1 归 `adc_v3`，VC0/1 归 `vc_v3_2`。ADC 驱动头列出 16 路外部输入和
AVCC/3、温度传感器两路内部输入。ADC0/1 共用 IRQ 10，
VC0/1 共用 IRQ 1。ADC0/1 和 VC 的门控为 APB1EN bit 8/9/10。
ADC 的 EOC 通过 `ADC_Enable_EOC_DMATransfer()` 使能 DMA 请求，
在 `driver/inc/dmac.h` 中 ADC0/1 请求值为 `0x4D`/`0x4F`。

`adc_v3` 有 16 个寄存器，保留内嵌 BGR 和 SQR，采用单 RESULT，省去 JQR 和 SQRRESULT 数组。
`vc_v3_2` 保留 A052/F052 的 CR0/CR1/CR2/SR 布局，去掉 CR2.ATIM1CLR/ATIM2CLR。

## SVD 实例与基址

表中使用 SVD 的实例名与基址。SDK 目录与文件来源见 [SVD 总览](../svd-overview.md)。

| 系列 | SVD 实例与基址 |
|---|---|
| A052 | `ADC@0x40002500`, `OPA@0x40002600`, `VC0@0x40002400`, `VC1@0x40002410` |
| A136 | `ADC@0x40002400`, `BGR@0x40002400`, `HDIV@0x40021800`, `OPA@0x40002400`, `VC@0x40002400` |
| F002 | `ADC@0x40002400` |
| F003 | `ADC@0x40002400`, `BGR@0x40002400`, `VC@0x40002400` |
| F005 | `ADC@0x40002400`, `BGR@0x40002400`, `VC@0x40002400` |
| F030 | `ADC@0x40002400`, `BGR@0x40002400`, `HDIV@0x40021800`, `OPA@0x40002400`, `VC@0x40002400` |
| F052 | `ADC@0x40002500`, `OPA@0x40002600`, `VC0@0x40002400`, `VC1@0x40002410` |
| F072 | `ADC@0x40002400`, `BGR@0x40002400`, `DAC@0x40002500`, `HDIV@0x40021800`, `OPA@0x40002400`, `VC@0x40002400` |
| F17x | `ADC@0x40002400`, `BGR@0x40002400`, `DAC@0x40002500`, `OPA@0x40002400`, `VC@0x40002400` |
| F19x | `ADC@0x40002400`, `BGR@0x40002400`, `DAC@0x40002500`, `OPA@0x40002400`, `VC@0x40002400` |
| F420 | `ADC0@0x4000A000`, `ADC1@0x4000A400`, `VC0@0x4000A800`, `VC1@0x4000A810` |
| L021 | `ADC@0x40002400`, `VC@0x40002800` |
| L07x | `ADC@0x40002400`, `BGR@0x40002400`, `DAC@0x40002500`, `HDIV@0x40021800`, `OPA@0x40002400`, `VC@0x40002400` |
| L110 | `ADC@0x40002400`, `BGR@0x40002400`, `VC@0x40002400` |
| L12x | `ADC@0x40002400`, `VC0@0x40002800`, `VC1@0x40002810`, `VCTRIM@0x40002820` |
| L13x | `ADC@0x40002400`, `BGR@0x40002400`, `HDIV@0x40021800`, `OPA@0x40002400`, `VC@0x40002400` |
| L16x | `ADC@0x40002400`, `BGR@0x40002400`, `DAC@0x40002500`, `OPA@0x40002400`, `VC@0x40002400` |
| L17x | `ADC@0x40002400`, `BGR@0x40002400`, `DAC@0x40002500`, `OPA@0x40002400`, `VC@0x40002400` |
| L18x | `ADC@0x40002400`, `BGR@0x40002400`, `DAC@0x40002500`, `OPA@0x40002400`, `VC@0x40002400` |
| L19x | `ADC@0x40002400`, `BGR@0x40002400`, `DAC@0x40002500`, `OPA@0x40002400`, `VC@0x40002400` |
