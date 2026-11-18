# 高级定时器

ADT/ADTIM 统一使用 adt 名称：基础布局之后是含 SCMAR/SCMBR、专用比较中断/DMA 控制的扩展，
L16x/L18x 的 14 路刹车输入与标准布局用后缀区分，不再忽略 EN7/EN15、POL7/POL15 的差异。

ATIM 保留每个 MODE 视图，按 MODE0 计数、MODE1 门控/捕获及 MODE23 捕获/PWM 通道规模分档。
数字后缀保留 ARRDM、CSA/CSB、刹车与第四通道控制差异。TIM3 归真实的 tim 来源，见
[通用定时器](timer-gen.md)，不再同时赋予 atim_1ch/timer3 等多套编号。

F420 ATIM0 MODE23 的 IFR/ICLR bit 18 有 CA3，A052/F052 无该字段；
ATIM3 MODE1.CR0 增加 CSA/CSB，MODE23 部分标志改名。SVD 与设备头定义这些差异，
`atim0.h` 的标志宏未列 CA3。MODE 视图共用物理基址，不能当作独立计数器数量。
ATIM0 的 UEV/CHA/CHB/BKETR IRQ 为 43–46，ATIM3 对应 47–50。
门控 APB0EN bit 24/27，DMA A/B 请求为 0x58/0x59 和 0x5E/0x5F。

MODE 视图已挂接物理实例（hc32-data-tools `csv_to_yaml.py`）：17 系列 60 个
TIM/ATIM 实例经 family YAML `views:` 关联到本文档登记的版本档位，
生成侧不再降级为纯地址外设；PAC 每视图一条同基址 const（名 = SVD 视图名）。

[编号规则](../layout-versions.md) · [实例 CSV](../peripheral-versions.csv) · [SDK 核验](../sdk-verification.md)。

<!-- peripheral-versions:begin -->
## 版本与系列对照

以下名称与生成输入保持一致。系列名前缀 `HC32` 省略。

### adt：v1

高级定时器；影子比较及专用中断/DMA 为扩展能力，刹车位分变体。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `adt_ddl_v1.rs` | [adt_ddl_v1](../../data/registers/adt_ddl_v1.yaml) | L110: ADT0, ADT1, ADT2 | 36 / 285 | 本档位对照基准；完整定义见来源 YAML |
| `adt_ddl_v1_1.rs` | [adt_ddl_v1_1](../../data/registers/adt_ddl_v1_1.yaml) | F003/F005: ADTIM4, ADTIM5, ADTIM6 | 36 / 317 | 相对 adt_ddl_v1：定义差异 AOSCL/AOSSR/BCONR/CNTER/CR 等 32 项 |

### adt：v2

高级定时器；影子比较及专用中断/DMA 为扩展能力，刹车位分变体。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `adt_ddl_v2.rs` | [adt_ddl_v2](../../data/registers/adt_ddl_v2.yaml) | L16x/L18x: ADTIM4, ADTIM5, ADTIM6 | 38 / 337 | 本档位对照基准；完整定义见来源 YAML |
| `adt_ddl_v2_1.rs` | [adt_ddl_v2_1](../../data/registers/adt_ddl_v2_1.yaml) | A136/F030/F072/F17x/F19x/L07x/L13x/L17x/L19x: ADTIM4, ADTIM5, ADTIM6 | 38 / 341 | 相对 adt_ddl_v2：定义差异 PTBKP/PTBKS |

### atim：v1

MODE0 计数视图。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `atim_ddl_v1.rs` | [atim_ddl_v1](../../data/registers/atim_ddl_v1.yaml) | A052/F052: ATIM0MODE0, ATIM1MODE0, ATIM2MODE0 | 7 / 21 | 本档位对照基准；完整定义见来源 YAML |
| `atim_ddl_v1_1.rs` | [atim_ddl_v1_1](../../data/registers/atim_ddl_v1_1.yaml) | F420: ATIM0MODE0 | 7 / 21 | 相对 atim_ddl_v1：定义差异 DTR/M0CR |
| `atim_ddl_v1_2.rs` | [atim_ddl_v1_2](../../data/registers/atim_ddl_v1_2.yaml) | L021: ATIM3_MODE0 | 7 / 21 | 相对 atim_ddl_v1：定义差异 ICLR/IFR |
| `atim_ddl_v1_3.rs` | [atim_ddl_v1_3](../../data/registers/atim_ddl_v1_3.yaml) | A052/F052: ATIM3MODE0 | 7 / 21 | 相对 atim_ddl_v1：寄存器坐标/内联字段未见差异；保留独立 IR 节点/枚举/描述来源，不据此合并 |
| `atim_ddl_v1_4.rs` | [atim_ddl_v1_4](../../data/registers/atim_ddl_v1_4.yaml) | F002/F420: ATIM3MODE0 | 7 / 21 | 相对 atim_ddl_v1：定义差异 DTR/M0CR |

### atim：v2

MODE1 门控/捕获，CSA/CSB 按变体保留。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `atim_ddl_v2.rs` | [atim_ddl_v2](../../data/registers/atim_ddl_v2.yaml) | F002: ATIM3MODE1 | 8 / 29 | 本档位对照基准；完整定义见来源 YAML |
| `atim_ddl_v2_1.rs` | [atim_ddl_v2_1](../../data/registers/atim_ddl_v2_1.yaml) | F420: ATIM0MODE1 | 8 / 31 | 相对 atim_ddl_v2：定义差异 CR0 |
| `atim_ddl_v2_2.rs` | [atim_ddl_v2_2](../../data/registers/atim_ddl_v2_2.yaml) | F420: ATIM3MODE1 | 8 / 31 | 相对 atim_ddl_v2：定义差异 CR0 |
| `atim_ddl_v2_3.rs` | [atim_ddl_v2_3](../../data/registers/atim_ddl_v2_3.yaml) | L021: ATIM3_MODE1 | 8 / 31 | 相对 atim_ddl_v2：定义差异 CCR0A/CR0/ICLR/IFR/M1CR |
| `atim_ddl_v2_4.rs` | [atim_ddl_v2_4](../../data/registers/atim_ddl_v2_4.yaml) | A052/F052: ATIM0MODE1, ATIM1MODE1, ATIM2MODE1 | 8 / 31 | 相对 atim_ddl_v2：定义差异 CR0/ICLR/M1CR |
| `atim_ddl_v2_5.rs` | [atim_ddl_v2_5](../../data/registers/atim_ddl_v2_5.yaml) | A052/F052: ATIM3MODE1 | 8 / 31 | 相对 atim_ddl_v2：定义差异 CR0/ICLR/M1CR |

### atim：v3

MODE23 捕获/PWM，1 组 A 通道寄存器；ARRDM、刹车/第四通道按变体保留。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `atim_ddl_v3.rs` | [atim_ddl_v3](../../data/registers/atim_ddl_v3.yaml) | A052/F052: ATIM0MODE23, ATIM1MODE23, ATIM2MODE23 | 13 / 103 | 本档位对照基准；完整定义见来源 YAML |
| `atim_ddl_v3_1.rs` | [atim_ddl_v3_1](../../data/registers/atim_ddl_v3_1.yaml) | F420: ATIM0MODE23 | 13 / 105 | 相对 atim_ddl_v3：定义差异 ADTR/CRCH0/DTR/ICLR/IFR 等 6 项 |

### atim：v4

MODE23 捕获/PWM，3 组 A 通道寄存器；ARRDM、刹车/第四通道按变体保留；MODE23 捕获/PWM，4 组 A 通道寄存器；ARRDM、刹车/第四通道按变体保留。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `atim_ddl_v4.rs` | [atim_ddl_v4](../../data/registers/atim_ddl_v4.yaml) | L021: ATIM3_MODE23 | 20 / 162 | 本档位对照基准；完整定义见来源 YAML |
| `atim_ddl_v4_1.rs` | [atim_ddl_v4_1](../../data/registers/atim_ddl_v4_1.yaml) | F420: ATIM3MODE23 | 21 / 177 | 相对 atim_ddl_v4：增加 CCR3/CRCH3；缺少 ARRDM；定义差异 ADTR/CRCH0/CRCH1/CRCH2/DTR 等 11 项 |
| `atim_ddl_v4_2.rs` | [atim_ddl_v4_2](../../data/registers/atim_ddl_v4_2.yaml) | F002: ATIM3MODE23 | 21 / 177 | 相对 atim_ddl_v4：增加 CCR3/CRCH3；缺少 ARRDM；定义差异 ADTR/CRCH0/CRCH1/CRCH2/DTR 等 11 项 |
| `atim_ddl_v4_3.rs` | [atim_ddl_v4_3](../../data/registers/atim_ddl_v4_3.yaml) | A052/F052: ATIM3MODE23 | 23 / 187 | 相对 atim_ddl_v4：增加 CCR3A/CCR3B/CRCH3A/CRCH3B；缺少 ARRDM；定义差异 CRCH0/CRCH1/CRCH2/FLTR/ICLR 等 8 项 |
<!-- peripheral-versions:end -->

## SVD 实例与基址

表中使用 SVD 的实例名与基址。SDK 目录与文件来源见 [SVD 总览](../svd-overview.md)。

| 系列 | SVD 实例与基址 |
|---|---|
| A052 | `ATIM0MODE0@0x40003000`, `ATIM0MODE1@0x40003000`, `ATIM0MODE23@0x40003000`, `ATIM1MODE0@0x40003400`, `ATIM1MODE1@0x40003400`, `ATIM1MODE23@0x40003400`, `ATIM2MODE0@0x40003800`, `ATIM2MODE1@0x40003800`, `ATIM2MODE23@0x40003800`, `ATIM3MODE0@0x40005800`, `ATIM3MODE1@0x40005800`, `ATIM3MODE23@0x40005800` |
| A136 | `ADTIM4@0x40003000`, `ADTIM5@0x40003400`, `ADTIM6@0x40003800` |
| F002 | `ATIM3MODE0@0x40005800`, `ATIM3MODE1@0x40005800`, `ATIM3MODE23@0x40005800` |
| F003 | `ADTIM4@0x40003000`, `ADTIM5@0x40003400`, `ADTIM6@0x40003800` |
| F005 | `ADTIM4@0x40003000`, `ADTIM5@0x40003400`, `ADTIM6@0x40003800` |
| F030 | `ADTIM4@0x40003000`, `ADTIM5@0x40003400`, `ADTIM6@0x40003800` |
| F052 | `ATIM0MODE0@0x40003000`, `ATIM0MODE1@0x40003000`, `ATIM0MODE23@0x40003000`, `ATIM1MODE0@0x40003400`, `ATIM1MODE1@0x40003400`, `ATIM1MODE23@0x40003400`, `ATIM2MODE0@0x40003800`, `ATIM2MODE1@0x40003800`, `ATIM2MODE23@0x40003800`, `ATIM3MODE0@0x40005800`, `ATIM3MODE1@0x40005800`, `ATIM3MODE23@0x40005800` |
| F072 | `ADTIM4@0x40003000`, `ADTIM5@0x40003400`, `ADTIM6@0x40003800` |
| F17x | `ADTIM4@0x40003000`, `ADTIM5@0x40003400`, `ADTIM6@0x40003800` |
| F19x | `ADTIM4@0x40003000`, `ADTIM5@0x40003400`, `ADTIM6@0x40003800` |
| F420 | `ATIM0MODE0@0x40006000`, `ATIM0MODE1@0x40006000`, `ATIM0MODE23@0x40006000`, `ATIM3MODE0@0x40006C00`, `ATIM3MODE1@0x40006C00`, `ATIM3MODE23@0x40006C00` |
| L021 | `ATIM3_MODE0@0x40009800`, `ATIM3_MODE1@0x40009800`, `ATIM3_MODE23@0x40009800` |
| L07x | `ADTIM4@0x40003000`, `ADTIM5@0x40003400`, `ADTIM6@0x40003800` |
| L110 | `ADT0@0x40003000`, `ADT1@0x40003400`, `ADT2@0x40003800` |
| L13x | `ADTIM4@0x40003000`, `ADTIM5@0x40003400`, `ADTIM6@0x40003800` |
| L16x | `ADTIM4@0x40003000`, `ADTIM5@0x40003400`, `ADTIM6@0x40003800` |
| L17x | `ADTIM4@0x40003000`, `ADTIM5@0x40003400`, `ADTIM6@0x40003800` |
| L18x | `ADTIM4@0x40003000`, `ADTIM5@0x40003400`, `ADTIM6@0x40003800` |
| L19x | `ADTIM4@0x40003000`, `ADTIM5@0x40003400`, `ADTIM6@0x40003800` |
