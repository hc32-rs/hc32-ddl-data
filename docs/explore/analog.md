# ADC、DAC、运放与比较器

ADC 以基础转换、序列控制和结果读出能力排序。F002 为基础转换，L021 为四通道顺序转换，
F420 有内嵌 BGR/SQR 和单 RESULT；F003/F005/L110 为早期多结果布局。
A052/F052 与 L12x 分别为四、六 rank，长序列组保留十六个顺序结果和插队结果。
长序列的 ALLSTART、CHMAP、RTC 触发位及字段命名不同，分别保留布局变体。
F420 与早期多结果组侧重点不同，主档位排序不是寄存器兼容或功能包含关系。

OPA 从单运放基础控制、ADC 触发扩展，再到双运放自动调零/三运放分支和五运放。
VC 按独立实例、双比较器窗口、三比较器路由分档；L021 的校准窗口不能当作其他系列的同址窗口。
DAC 按单通道、双通道及双数据写入分档。

A136 的 SDK 设备头 `HC32A136_DDL_Rev2.1.0/mcu/common/hc32a136.h` 与 SVD 一致，
ADC、OPA、VC、LVD、BGR 基址均为 `0x40002400`。寄存器分处不同偏移：
BGR.CR +0x00、ADC.CR0 +0x04、VC.CR +0x10、LVD.CR +0x28、OPA.CR0 +0x30。

F420 ADC0/1 有 16 路外部输入以及 AVCC/3、温度传感器输入，共用 IRQ 10。
VC0/1 共用 IRQ 1，ADC0/1 和 VC 门控为 APB1EN bit 8/9/10。
`ADC_Enable_EOC_DMATransfer()` 使能 EOC DMA，请求值见 `driver/inc/dmac.h` 的 0x4D/0x4F。
VC0/1 的 CR2 不含 A052/F052 的 ATIM1CLR/ATIM2CLR。F420 两个 ADC 的生成布局仍分别登记。

[编号规则](../layout-versions.md) · [实例 CSV](../peripheral-versions.csv) · [SDK 核验](../sdk-verification.md)。

<!-- peripheral-versions:begin -->
## 版本与系列对照

以下名称与生成输入保持一致。系列名前缀 `HC32` 省略。

### adc：v1

基础转换、单结果。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `adc_ddl_v1.rs` | [adc_ddl_v1](../../data/registers/adc_ddl_v1.yaml) | F002: ADC | 9 / 21 | 本档位对照基准；完整定义见来源 YAML |

### adc：v2

四通道顺序转换。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `adc_ddl_v2.rs` | [adc_ddl_v2](../../data/registers/adc_ddl_v2.yaml) | L021: ADC | 11 / 30 | 本档位对照基准；完整定义见来源 YAML |

### adc：v3

内嵌 BGR、SQR、单 RESULT。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `adc_ddl_v3.rs` | [adc_ddl_v3](../../data/registers/adc_ddl_v3.yaml) | F420: ADC0 | 16 / 87 | 本档位对照基准；完整定义见来源 YAML |
| `adc_ddl_v3_1.rs` | [adc_ddl_v3_1](../../data/registers/adc_ddl_v3_1.yaml) | F420: ADC1 | 16 / 87 | 相对 adc_ddl_v3：寄存器坐标/内联字段未见差异；保留独立 IR 节点/枚举/描述来源，不据此合并 |

### adc：v4

早期多结果读出。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `adc_ddl_v4.rs` | [adc_ddl_v4](../../data/registers/adc_ddl_v4.yaml) | F003/F005/L110: ADC | 18 / 47 | 本档位对照基准；完整定义见来源 YAML |

### adc：v5

四 rank 顺序/插队转换、内嵌 BGR。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `adc_ddl_v5.rs` | [adc_ddl_v5](../../data/registers/adc_ddl_v5.yaml) | A052/F052: ADC | 27 / 112 | 本档位对照基准；完整定义见来源 YAML |

### adc：v6

六 rank、顺序/插队转换。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `adc_ddl_v6.rs` | [adc_ddl_v6](../../data/registers/adc_ddl_v6.yaml) | L12x: ADC | 28 / 89 | 本档位对照基准；完整定义见来源 YAML |

### adc：v7

十六个顺序结果、插队结果与累计结果；ALLSTART/CHMAP 等按变体保留。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `adc_ddl_v7.rs` | [adc_ddl_v7](../../data/registers/adc_ddl_v7.yaml) | A136/F030/L13x: ADC | 37 / 178 | 本档位对照基准；完整定义见来源 YAML |
| `adc_ddl_v7_1.rs` | [adc_ddl_v7_1](../../data/registers/adc_ddl_v7_1.yaml) | L16x/L18x: ADC | 38 / 178 | 相对 adc_ddl_v7：增加 ALLSTART/SQRRESULT13；缺少 SQR_RESULT13；定义差异 EXTTRIGGER0/EXTTRIGGER1 |
| `adc_ddl_v7_2.rs` | [adc_ddl_v7_2](../../data/registers/adc_ddl_v7_2.yaml) | F072: ADC | 38 / 179 | 相对 adc_ddl_v7：增加 ALLSTART；定义差异 CR0/EXTTRIGGER0/EXTTRIGGER1 |
| `adc_ddl_v7_3.rs` | [adc_ddl_v7_3](../../data/registers/adc_ddl_v7_3.yaml) | F17x/F19x/L17x/L19x: ADC | 38 / 180 | 相对 adc_ddl_v7：增加 ALLSTART |
| `adc_ddl_v7_4.rs` | [adc_ddl_v7_4](../../data/registers/adc_ddl_v7_4.yaml) | L07x: ADC | 38 / 181 | 相对 adc_ddl_v7：增加 ALLSTART；定义差异 CR0 |

### bgr：v1

带隙基准控制。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `bgr_ddl_v1.rs` | [bgr_ddl_v1](../../data/registers/bgr_ddl_v1.yaml) | L110: BGR | 1 / 2 | 本档位对照基准；完整定义见来源 YAML |
| `bgr_ddl_v1_1.rs` | [bgr_ddl_v1_1](../../data/registers/bgr_ddl_v1_1.yaml) | A136/F003/F005/F030/F072/F17x/F19x/L07x/L13x/L16x/L17x/L18x/L19x: BGR | 1 / 3 | 相对 bgr_ddl_v1：定义差异 CR |

### dac：v1

单通道转换。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `dac_ddl_v1.rs` | [dac_ddl_v1](../../data/registers/dac_ddl_v1.yaml) | F17x/F19x/L16x/L17x/L18x/L19x: DAC | 8 / 23 | 本档位对照基准；完整定义见来源 YAML |

### dac：v2

双通道、双数据写入。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `dac_ddl_v2.rs` | [dac_ddl_v2](../../data/registers/dac_ddl_v2.yaml) | F072/L07x: DAC | 15 / 50 | 本档位对照基准；完整定义见来源 YAML |

### hdiv：v1

硬件除法。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `hdiv_ddl_v1.rs` | [hdiv_ddl_v1](../../data/registers/hdiv_ddl_v1.yaml) | A136/F030/F072/L07x/L13x: HDIV | 6 / 7 | 本档位对照基准；完整定义见来源 YAML |

### opa：v1

单运放基础控制。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `opa_ddl_v1.rs` | [opa_ddl_v1](../../data/registers/opa_ddl_v1.yaml) | L16x/L18x: OPA | 1 / 8 | 本档位对照基准；完整定义见来源 YAML |

### opa：v2

单运放及 ADC 触发。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `opa_ddl_v2.rs` | [opa_ddl_v2](../../data/registers/opa_ddl_v2.yaml) | F17x/F19x/L17x/L19x: OPA | 2 / 15 | 本档位对照基准；完整定义见来源 YAML |

### opa：v3

双运放及 AZ 自动调零；三运放独立控制，与双运放 AZ 为同档不同分支。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `opa_ddl_v3.rs` | [opa_ddl_v3](../../data/registers/opa_ddl_v3.yaml) | A052/F052: OPA | 3 / 14 | 本档位对照基准；完整定义见来源 YAML |
| `opa_ddl_v3_1.rs` | [opa_ddl_v3_1](../../data/registers/opa_ddl_v3_1.yaml) | A136/F030/L13x: OPA | 4 / 38 | 相对 opa_ddl_v3：增加 CR/CR2；缺少 AZ；定义差异 CR0/CR1 |

### opa：v4

五运放控制。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `opa_ddl_v4.rs` | [opa_ddl_v4](../../data/registers/opa_ddl_v4.yaml) | F072/L07x: OPA | 4 / 34 | 本档位对照基准；完整定义见来源 YAML |

### vc：v1

独立比较器实例，CR0/CR1/CR2 与状态窗口。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `vc_ddl_v1.rs` | [vc_ddl_v1](../../data/registers/vc_ddl_v1.yaml) | F420: VC0, VC1 | 4 / 26 | 本档位对照基准；完整定义见来源 YAML |
| `vc_ddl_v1_1.rs` | [vc_ddl_v1_1](../../data/registers/vc_ddl_v1_1.yaml) | A052/F052: VC0, VC1 | 4 / 28 | 相对 vc_ddl_v1：定义差异 CR2 |
| `vc_ddl_v1_2.rs` | [vc_ddl_v1_2](../../data/registers/vc_ddl_v1_2.yaml) | L12x: VC0, VC1 | 4 / 29 | 相对 vc_ddl_v1：增加 IFR；缺少 SR；定义差异 CR0/CR1/CR2 |

### vc：v2

双比较器；输出路由与校准为同档不同分支。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `vc_ddl_v2.rs` | [vc_ddl_v2](../../data/registers/vc_ddl_v2.yaml) | A136/F030/L13x: VC | 6 / 61 | 本档位对照基准；完整定义见来源 YAML |
| `vc_ddl_v2_1.rs` | [vc_ddl_v2_1](../../data/registers/vc_ddl_v2_1.yaml) | L110: VC | 6 / 61 | 相对 vc_ddl_v2：定义差异 CR/IFR/VC0_CR/VC0_OUT_CFG/VC1_CR 等 6 项 |
| `vc_ddl_v2_2.rs` | [vc_ddl_v2_2](../../data/registers/vc_ddl_v2_2.yaml) | F003/F005: VC | 6 / 63 | 相对 vc_ddl_v2：定义差异 VC0_CR/VC0_OUT_CFG/VC1_CR/VC1_OUT_CFG |
| `vc_ddl_v2_3.rs` | [vc_ddl_v2_3](../../data/registers/vc_ddl_v2_3.yaml) | L021: VC | 9 / 53 | 相对 vc_ddl_v2：增加 VC0_CR0/VC0_CR1/VC0_CR2/VC0_SR/VC1_CR0 等 9 项；缺少 CR/IFR/VC0_CR/VC0_OUT_CFG/VC1_CR 等 6 项 |

### vc：v3

三比较器与输出路由。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `vc_ddl_v3.rs` | [vc_ddl_v3](../../data/registers/vc_ddl_v3.yaml) | F072/F17x/F19x/L07x/L16x/L17x/L18x/L19x: VC | 8 / 89 | 本档位对照基准；完整定义见来源 YAML |
<!-- peripheral-versions:end -->

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
