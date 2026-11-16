# 通用、基本与低功耗定时器

TIM0–3 的 MODE0/MODE1/MODE23 为同址视图，CSV 每个视图单独一行。
紧凑基础计数、MODE0、MODE1、MODE23 的通道能力依次分档；不同通道数、
CSA/CSB、VC0E/VC1E 与 VCE、OVF/UNDF、ARRDM 和 CM3AE/CM3BE 均保留实际布局。
原文用一个 timer3 功能编号覆盖多种位域的建议不再作为生成命名依据。

GTIM 的 IER 位数与 C0TRAD/CH0TRAD 命名分变体。
BTIM 的 AIFR/AICR 是按 bank 共享的全局寄存器，UI0/TI0 至 UI8/TI8 不能按实例删除。
LPTIM/LPTIMER 统一使用 lptim，基础计数之后是带 CMP 的比较扩展，偏移/控制位仍独立编号。

F420 CTIM0–2 各对应 GTIM0–2 及 BTIM0–2/3–5/6–8 视图，由 SYSCTRL 选择，
共用 IRQ 34–36/37–39/40–42。后缀依次为 UEV_ETR_BTIMA、CH01_BTIMB、CH23_BTIMC。
门控 APB0EN bit 20–22；仅 GTIM 视图在 `dmac.h` 有 DMA 请求 0x48–0x4A。
`driver/inc/sysctrl.h` 的 Sysctrl_Fun_Gtim*_0_Btim*_1 枚举选择视图。

[编号规则](../layout-versions.md) · [实例 CSV](../peripheral-versions.csv) · [SDK 核验](../sdk-verification.md)。

<!-- peripheral-versions:begin -->
## 版本与系列对照

以下为待确认命名，当前来源仍使用旧文件名。系列名前缀 `HC32` 省略。

### btim：v1

基本定时器；共享 AIFR/AICR bank 位图分变体。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `btim_ddl_v1.rs` | [btim_v1](../../data/registers/btim_v1.yaml) | A052/F052: BTIM0, BTIM1, BTIM2；L021: BTIM0, BTIM1, BTIM2, BTIM3, BTIM4, BTIM5 | 8 / 35 | 本档位对照基准；完整定义见来源 YAML |
| `btim_ddl_v1_1.rs` | [btim_v2](../../data/registers/btim_v2.yaml) | A052/F002/F052/F420: BTIM3, BTIM4, BTIM5 | 8 / 35 | 相对 btim_ddl_v1：定义差异 AICR/AIFR |
| `btim_ddl_v1_2.rs` | [btim_v3](../../data/registers/btim_v3.yaml) | F420: BTIM0, BTIM1, BTIM2 | 8 / 35 | 相对 btim_ddl_v1：定义差异 AICR/AIFR |
| `btim_ddl_v1_3.rs` | [btim_v4](../../data/registers/btim_v4.yaml) | F420: BTIM6, BTIM7, BTIM8 | 8 / 35 | 相对 btim_ddl_v1：定义差异 AICR/AIFR |

### gtim：v1

通用捕获/比较定时器；IER 与控制字段分变体。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `gtim_ddl_v1.rs` | [gtim_v2](../../data/registers/gtim_v2.yaml) | F002: GTIM | 12 / 48 | 本档位对照基准；完整定义见来源 YAML |
| `gtim_ddl_v1_1.rs` | [gtim_v4](../../data/registers/gtim_v4.yaml) | L021: GTIM0, GTIM1 | 12 / 50 | 相对 gtim_ddl_v1：定义差异 IER |
| `gtim_ddl_v1_2.rs` | [gtim_v1](../../data/registers/gtim_v1.yaml) | A052/F052: GTIM0, GTIM1 | 12 / 54 | 相对 gtim_ddl_v1：定义差异 IER |
| `gtim_ddl_v1_3.rs` | [gtim_v3](../../data/registers/gtim_v3.yaml) | F420: GTIM0, GTIM1, GTIM2 | 12 / 54 | 相对 gtim_ddl_v1：定义差异 IER |

### lptim：v1

低功耗计数；CMP 比较为扩展，偏移及控制字段分变体。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `lptim_ddl_v1.rs` | [lptimer_v3](../../data/registers/lptimer_v3.yaml) | L110: LPTIMER | 5 / 13 | 本档位对照基准；完整定义见来源 YAML |
| `lptim_ddl_v1_1.rs` | [lptimer_v1](../../data/registers/lptimer_v1.yaml) | A136/L13x: LPTIMER | 5 / 18 | 相对 lptim_ddl_v1：定义差异 ARR/CNT/CR/ICLR/IFR |
| `lptim_ddl_v1_2.rs` | [lptim_common](../../data/registers/lptim_common.yaml) | A052/F052: LPTIM | 5 / 19 | 相对 lptim_ddl_v1：定义差异 ARR/CNT/CR/ICLR/IFR |
| `lptim_ddl_v1_3.rs` | [lptimer_v2](../../data/registers/lptimer_v2.yaml) | L07x/L16x/L17x/L18x/L19x: LPTIMER0, LPTIMER1 | 5 / 19 | 相对 lptim_ddl_v1：定义差异 ARR/CNT/CR/ICLR/IFR |

### lptim：v2

低功耗计数；CMP 比较为扩展，偏移及控制字段分变体。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `lptim_ddl_v2.rs` | [lptimer_v4](../../data/registers/lptimer_v4.yaml) | L12x: LPTIMER0, LPTIMER1 | 6 / 22 | 本档位对照基准；完整定义见来源 YAML |

### tim：v1

紧凑基础计数器。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `tim_ddl_v1.rs` | [tim_v13](../../data/registers/tim_v13.yaml) | F003/F005: TIM0, TIM1, TIM2 | 6 / 18 | 本档位对照基准；完整定义见来源 YAML |

### tim：v2

MODE0 计数视图。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `tim_ddl_v2.rs` | [tim_v1](../../data/registers/tim_v1.yaml) | A136/F030/F072/F17x/F19x/L07x/L13x/L16x/L17x/L18x/L19x: TIM0_MODE0；L12x: TIM0_MODE0, TIM1_MODE0, TIM2_MODE0, TIM3_MODE0 | 7 / 21 | 本档位对照基准；完整定义见来源 YAML |
| `tim_ddl_v2_1.rs` | [tim_v10](../../data/registers/tim_v10.yaml) | A136/F030/F072/F17x/F19x/L07x/L13x/L16x/L17x/L18x/L19x: TIM3_MODE0 | 7 / 21 | 相对 tim_ddl_v2：寄存器坐标/内联字段未见差异；保留独立 IR 节点/枚举/描述来源，不据此合并 |
| `tim_ddl_v2_2.rs` | [tim_v4](../../data/registers/tim_v4.yaml) | A136/F030/F072/F17x/F19x/L07x/L13x/L16x/L17x/L18x/L19x: TIM1_MODE0 | 7 / 21 | 相对 tim_ddl_v2：寄存器坐标/内联字段未见差异；保留独立 IR 节点/枚举/描述来源，不据此合并 |
| `tim_ddl_v2_3.rs` | [tim_v7](../../data/registers/tim_v7.yaml) | A136/F030/F072/F17x/F19x/L07x/L13x/L16x/L17x/L18x/L19x: TIM2_MODE0 | 7 / 21 | 相对 tim_ddl_v2：寄存器坐标/内联字段未见差异；保留独立 IR 节点/枚举/描述来源，不据此合并 |

### tim：v3

MODE1 门控/捕获，CSA/CSB 按变体保留。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `tim_ddl_v3.rs` | [tim_v11](../../data/registers/tim_v11.yaml) | A136/F030/F072/F17x/F19x/L07x/L13x/L17x/L19x: TIM3_MODE1 | 8 / 29 | 本档位对照基准；完整定义见来源 YAML |
| `tim_ddl_v3_1.rs` | [tim_v2](../../data/registers/tim_v2.yaml) | A136/F030/F072/F17x/F19x/L07x/L13x/L17x/L19x: TIM0_MODE1 | 8 / 29 | 相对 tim_ddl_v3：寄存器坐标/内联字段未见差异；保留独立 IR 节点/枚举/描述来源，不据此合并 |
| `tim_ddl_v3_2.rs` | [tim_v5](../../data/registers/tim_v5.yaml) | A136/F030/F072/F17x/F19x/L07x/L13x/L17x/L19x: TIM1_MODE1 | 8 / 29 | 相对 tim_ddl_v3：寄存器坐标/内联字段未见差异；保留独立 IR 节点/枚举/描述来源，不据此合并 |
| `tim_ddl_v3_3.rs` | [tim_v8](../../data/registers/tim_v8.yaml) | A136/F030/F072/F17x/F19x/L07x/L13x/L17x/L19x: TIM2_MODE1 | 8 / 29 | 相对 tim_ddl_v3：寄存器坐标/内联字段未见差异；保留独立 IR 节点/枚举/描述来源，不据此合并 |
| `tim_ddl_v3_4.rs` | [tim_v18](../../data/registers/tim_v18.yaml) | L12x: TIM0_MODE1, TIM1_MODE1, TIM2_MODE1, TIM3_MODE1；L16x/L18x: TIM0_MODE1 | 8 / 31 | 相对 tim_ddl_v3：定义差异 CR0 |
| `tim_ddl_v3_5.rs` | [tim_v21](../../data/registers/tim_v21.yaml) | L16x/L18x: TIM1_MODE1 | 8 / 31 | 相对 tim_ddl_v3：定义差异 CR0 |
| `tim_ddl_v3_6.rs` | [tim_v23](../../data/registers/tim_v23.yaml) | L16x/L18x: TIM2_MODE1 | 8 / 31 | 相对 tim_ddl_v3：定义差异 CR0 |
| `tim_ddl_v3_7.rs` | [tim_v25](../../data/registers/tim_v25.yaml) | L16x/L18x: TIM3_MODE1 | 8 / 31 | 相对 tim_ddl_v3：定义差异 CR0 |

### tim：v4

MODE23 捕获/PWM，1 组 A 通道寄存器；ARRDM、刹车/第四通道按变体保留。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `tim_ddl_v4.rs` | [tim_v14](../../data/registers/tim_v14.yaml) | F072/F17x/F19x/L07x/L17x/L19x: TIM0_MODE23 | 14 / 97 | 本档位对照基准；完整定义见来源 YAML |
| `tim_ddl_v4_1.rs` | [tim_v15](../../data/registers/tim_v15.yaml) | F072/F17x/F19x/L07x/L17x/L19x: TIM1_MODE23 | 14 / 97 | 相对 tim_ddl_v4：寄存器坐标/内联字段未见差异；保留独立 IR 节点/枚举/描述来源，不据此合并 |
| `tim_ddl_v4_2.rs` | [tim_v16](../../data/registers/tim_v16.yaml) | F072/F17x/F19x/L07x/L17x/L19x: TIM2_MODE23 | 14 / 97 | 相对 tim_ddl_v4：寄存器坐标/内联字段未见差异；保留独立 IR 节点/枚举/描述来源，不据此合并 |
| `tim_ddl_v4_3.rs` | [tim_v3](../../data/registers/tim_v3.yaml) | A136/F030/L13x: TIM0_MODE23 | 14 / 98 | 相对 tim_ddl_v4：定义差异 DTR |
| `tim_ddl_v4_4.rs` | [tim_v6](../../data/registers/tim_v6.yaml) | A136/F030/L13x: TIM1_MODE23 | 14 / 98 | 相对 tim_ddl_v4：定义差异 DTR |
| `tim_ddl_v4_5.rs` | [tim_v9](../../data/registers/tim_v9.yaml) | A136/F030/L13x: TIM2_MODE23 | 14 / 98 | 相对 tim_ddl_v4：定义差异 DTR |
| `tim_ddl_v4_6.rs` | [tim_v19](../../data/registers/tim_v19.yaml) | L12x: TIM0_MODE23, TIM1_MODE23, TIM2_MODE23；L16x/L18x: TIM0_MODE23 | 14 / 105 | 相对 tim_ddl_v4：定义差异 ICLR/IFR/M23CR/RCR |
| `tim_ddl_v4_7.rs` | [tim_v22](../../data/registers/tim_v22.yaml) | L16x/L18x: TIM1_MODE23 | 14 / 105 | 相对 tim_ddl_v4：定义差异 ICLR/IFR/M23CR/RCR |
| `tim_ddl_v4_8.rs` | [tim_v24](../../data/registers/tim_v24.yaml) | L16x/L18x: TIM2_MODE23 | 14 / 105 | 相对 tim_ddl_v4：定义差异 ICLR/IFR/M23CR/RCR |

### tim：v5

MODE23 捕获/PWM，3 组 A 通道寄存器；ARRDM、刹车/第四通道按变体保留；MODE23 捕获/PWM，4 组 A 通道寄存器；ARRDM、刹车/第四通道按变体保留。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `tim_ddl_v5.rs` | [tim_v17](../../data/registers/tim_v17.yaml) | F072/F17x/F19x/L07x/L17x/L19x: TIM3_MODE23 | 20 / 161 | 本档位对照基准；完整定义见来源 YAML |
| `tim_ddl_v5_1.rs` | [tim_v12](../../data/registers/tim_v12.yaml) | A136/F030/L13x: TIM3_MODE23 | 20 / 162 | 相对 tim_ddl_v5：定义差异 DTR |
| `tim_ddl_v5_2.rs` | [tim_v26](../../data/registers/tim_v26.yaml) | L16x/L18x: TIM3_MODE23 | 24 / 189 | 相对 tim_ddl_v5：增加 CCR3A/CCR3B/CRCH3A/CRCH3B；定义差异 ICLR/IFR/M23CR/RCR |
| `tim_ddl_v5_3.rs` | [tim_v20](../../data/registers/tim_v20.yaml) | L12x: TIM3_MODE23 | 24 / 191 | 相对 tim_ddl_v5：增加 CCR3A/CCR3B/CRCH3A/CRCH3B；定义差异 ICLR/IFR/M23CR/RCR |
<!-- peripheral-versions:end -->

## SVD 实例与基址

表中使用 SVD 的实例名与基址。SDK 目录与文件来源见 [SVD 总览](../svd-overview.md)。

| 系列 | SVD 实例与基址 |
|---|---|
| A052 | `BTIM0@0x40001800`, `BTIM1@0x40001900`, `BTIM2@0x40001A00`, `BTIM3@0x40001C00`, `BTIM4@0x40001D00`, `BTIM5@0x40001E00`, `GTIM0@0x40001800`, `GTIM1@0x40001C00`, `LPTIM@0x40000C00` |
| A136 | `LPTIMER@0x40000F00`, `TIM0_MODE0@0x40000C00`, `TIM0_MODE1@0x40000C00`, `TIM0_MODE23@0x40000C00`, `TIM1_MODE0@0x40000D00`, `TIM1_MODE1@0x40000D00`, `TIM1_MODE23@0x40000D00`, `TIM2_MODE0@0x40000E00`, `TIM2_MODE1@0x40000E00`, `TIM2_MODE23@0x40000E00`, `TIM3_MODE0@0x40005800`, `TIM3_MODE1@0x40005800`, `TIM3_MODE23@0x40005800` |
| F002 | `BTIM3@0x40007400`, `BTIM4@0x40007500`, `BTIM5@0x40007600`, `GTIM@0x40007400` |
| F003 | `TIM0@0x40000C00`, `TIM1@0x40000C20`, `TIM2@0x40000C40` |
| F005 | `TIM0@0x40000C00`, `TIM1@0x40000C20`, `TIM2@0x40000C40` |
| F030 | `TIM0_MODE0@0x40000C00`, `TIM0_MODE1@0x40000C00`, `TIM0_MODE23@0x40000C00`, `TIM1_MODE0@0x40000D00`, `TIM1_MODE1@0x40000D00`, `TIM1_MODE23@0x40000D00`, `TIM2_MODE0@0x40000E00`, `TIM2_MODE1@0x40000E00`, `TIM2_MODE23@0x40000E00`, `TIM3_MODE0@0x40005800`, `TIM3_MODE1@0x40005800`, `TIM3_MODE23@0x40005800` |
| F052 | `BTIM0@0x40001800`, `BTIM1@0x40001900`, `BTIM2@0x40001A00`, `BTIM3@0x40001C00`, `BTIM4@0x40001D00`, `BTIM5@0x40001E00`, `GTIM0@0x40001800`, `GTIM1@0x40001C00`, `LPTIM@0x40000C00` |
| F072 | `TIM0_MODE0@0x40000C00`, `TIM0_MODE1@0x40000C00`, `TIM0_MODE23@0x40000C00`, `TIM1_MODE0@0x40000D00`, `TIM1_MODE1@0x40000D00`, `TIM1_MODE23@0x40000D00`, `TIM2_MODE0@0x40000E00`, `TIM2_MODE1@0x40000E00`, `TIM2_MODE23@0x40000E00`, `TIM3_MODE0@0x40005800`, `TIM3_MODE1@0x40005800`, `TIM3_MODE23@0x40005800` |
| F17x | `TIM0_MODE0@0x40000C00`, `TIM0_MODE1@0x40000C00`, `TIM0_MODE23@0x40000C00`, `TIM1_MODE0@0x40000D00`, `TIM1_MODE1@0x40000D00`, `TIM1_MODE23@0x40000D00`, `TIM2_MODE0@0x40000E00`, `TIM2_MODE1@0x40000E00`, `TIM2_MODE23@0x40000E00`, `TIM3_MODE0@0x40005800`, `TIM3_MODE1@0x40005800`, `TIM3_MODE23@0x40005800` |
| F19x | `TIM0_MODE0@0x40000C00`, `TIM0_MODE1@0x40000C00`, `TIM0_MODE23@0x40000C00`, `TIM1_MODE0@0x40000D00`, `TIM1_MODE1@0x40000D00`, `TIM1_MODE23@0x40000D00`, `TIM2_MODE0@0x40000E00`, `TIM2_MODE1@0x40000E00`, `TIM2_MODE23@0x40000E00`, `TIM3_MODE0@0x40005800`, `TIM3_MODE1@0x40005800`, `TIM3_MODE23@0x40005800` |
| F420 | `BTIM0@0x40005000`, `BTIM1@0x40005100`, `BTIM2@0x40005200`, `BTIM3@0x40005400`, `BTIM4@0x40005500`, `BTIM5@0x40005600`, `BTIM6@0x40005800`, `BTIM7@0x40005900`, `BTIM8@0x40005A00`, `GTIM0@0x40005000`, `GTIM1@0x40005400`, `GTIM2@0x40005800` |
| L021 | `BTIM0@0x40001800`, `BTIM1@0x40001900`, `BTIM2@0x40001A00`, `BTIM3@0x40001C00`, `BTIM4@0x40001D00`, `BTIM5@0x40001E00`, `GTIM0@0x40001800`, `GTIM1@0x40001C00` |
| L07x | `LPTIMER0@0x40000F00`, `LPTIMER1@0x40000F40`, `TIM0_MODE0@0x40000C00`, `TIM0_MODE1@0x40000C00`, `TIM0_MODE23@0x40000C00`, `TIM1_MODE0@0x40000D00`, `TIM1_MODE1@0x40000D00`, `TIM1_MODE23@0x40000D00`, `TIM2_MODE0@0x40000E00`, `TIM2_MODE1@0x40000E00`, `TIM2_MODE23@0x40000E00`, `TIM3_MODE0@0x40005800`, `TIM3_MODE1@0x40005800`, `TIM3_MODE23@0x40005800` |
| L110 | `BT0@0x40000C00`, `BT1@0x40000C20`, `BT2@0x40000C40`, `LPTIMER@0x40000C00` |
| L12x | `LPTIMER0@0x40000C00`, `LPTIMER1@0x40003C00`, `TIM0_MODE0@0x40003000`, `TIM0_MODE1@0x40003000`, `TIM0_MODE23@0x40003000`, `TIM1_MODE0@0x40003400`, `TIM1_MODE1@0x40003400`, `TIM1_MODE23@0x40003400`, `TIM2_MODE0@0x40003800`, `TIM2_MODE1@0x40003800`, `TIM2_MODE23@0x40003800`, `TIM3_MODE0@0x40009800`, `TIM3_MODE1@0x40009800`, `TIM3_MODE23@0x40009800` |
| L13x | `LPTIMER@0x40000F00`, `TIM0_MODE0@0x40000C00`, `TIM0_MODE1@0x40000C00`, `TIM0_MODE23@0x40000C00`, `TIM1_MODE0@0x40000D00`, `TIM1_MODE1@0x40000D00`, `TIM1_MODE23@0x40000D00`, `TIM2_MODE0@0x40000E00`, `TIM2_MODE1@0x40000E00`, `TIM2_MODE23@0x40000E00`, `TIM3_MODE0@0x40005800`, `TIM3_MODE1@0x40005800`, `TIM3_MODE23@0x40005800` |
| L16x | `LPTIMER0@0x40000F00`, `LPTIMER1@0x40000F40`, `TIM0_MODE0@0x40000C00`, `TIM0_MODE1@0x40000C00`, `TIM0_MODE23@0x40000C00`, `TIM1_MODE0@0x40000D00`, `TIM1_MODE1@0x40000D00`, `TIM1_MODE23@0x40000D00`, `TIM2_MODE0@0x40000E00`, `TIM2_MODE1@0x40000E00`, `TIM2_MODE23@0x40000E00`, `TIM3_MODE0@0x40005800`, `TIM3_MODE1@0x40005800`, `TIM3_MODE23@0x40005800` |
| L17x | `LPTIMER0@0x40000F00`, `LPTIMER1@0x40000F40`, `TIM0_MODE0@0x40000C00`, `TIM0_MODE1@0x40000C00`, `TIM0_MODE23@0x40000C00`, `TIM1_MODE0@0x40000D00`, `TIM1_MODE1@0x40000D00`, `TIM1_MODE23@0x40000D00`, `TIM2_MODE0@0x40000E00`, `TIM2_MODE1@0x40000E00`, `TIM2_MODE23@0x40000E00`, `TIM3_MODE0@0x40005800`, `TIM3_MODE1@0x40005800`, `TIM3_MODE23@0x40005800` |
| L18x | `LPTIMER0@0x40000F00`, `LPTIMER1@0x40000F40`, `TIM0_MODE0@0x40000C00`, `TIM0_MODE1@0x40000C00`, `TIM0_MODE23@0x40000C00`, `TIM1_MODE0@0x40000D00`, `TIM1_MODE1@0x40000D00`, `TIM1_MODE23@0x40000D00`, `TIM2_MODE0@0x40000E00`, `TIM2_MODE1@0x40000E00`, `TIM2_MODE23@0x40000E00`, `TIM3_MODE0@0x40005800`, `TIM3_MODE1@0x40005800`, `TIM3_MODE23@0x40005800` |
| L19x | `LPTIMER0@0x40000F00`, `LPTIMER1@0x40000F40`, `TIM0_MODE0@0x40000C00`, `TIM0_MODE1@0x40000C00`, `TIM0_MODE23@0x40000C00`, `TIM1_MODE0@0x40000D00`, `TIM1_MODE1@0x40000D00`, `TIM1_MODE23@0x40000D00`, `TIM2_MODE0@0x40000E00`, `TIM2_MODE1@0x40000E00`, `TIM2_MODE23@0x40000E00`, `TIM3_MODE0@0x40005800`, `TIM3_MODE1@0x40005800`, `TIM3_MODE23@0x40005800` |
