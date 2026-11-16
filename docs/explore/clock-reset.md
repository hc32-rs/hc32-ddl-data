# 时钟、复位与校准

SYSCTRL 由内部 RC 基础控制、外部时钟控制、PLL 扩展，再到 APB/AHB 多组门控分档。
PLL、RC48M、XTL 与复位寄存器是不同能力维度，后缀保留同档分支，不将它们视为彼此超集。
L110 的 CLOCK 目标名统一为 sysctrl，CLK_TRIM 使用完整的 clk_trim。
RESET 按状态/外设复位分组能力整理，CTRIM/CLK_TRIM 的输入源、限值位宽及字段差异分变体。

F420 SYSCTRL 有 21 个寄存器。`SYSCTRL_PeriphClkEnable/Disable()` 按枚举组标记
0x00/0x20/0x40/0x80 选择 APB0EN/APB1EN/AHB0EN/AHB1EN。
CTRIM 门控为 APB1EN bit 0，IRQ 为 0；CLK_FAULT IRQ 为 9。
F420 的 FLIM 为 8 位，A052/F052 为 12 位，且 F420 缺少 CR1.MDRCH。
L110 的 RESET 在 SVD 与设备头中都使用 PREI_RESET 拼写，保留原始字段。

[编号规则](../layout-versions.md) · [实例 CSV](../peripheral-versions.csv) · [SDK 核验](../sdk-verification.md)。

<!-- peripheral-versions:begin -->
## 版本与系列对照

以下为待确认命名，当前来源仍使用旧文件名。系列名前缀 `HC32` 省略。

### clk_trim：v1

时钟参考计数与校准。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `clk_trim_ddl_v1.rs` | [clk_v3](../../data/registers/clk_v3.yaml) | L110: CLK_TRIM | 7 / 15 | 本档位对照基准；完整定义见来源 YAML |
| `clk_trim_ddl_v1_1.rs` | [clk_v2](../../data/registers/clk_v2.yaml) | F003/F005: CLK_TRIM | 7 / 16 | 相对 clk_trim_ddl_v1：定义差异 CR/ICLR/IFR |
| `clk_trim_ddl_v1_2.rs` | [clk_v1](../../data/registers/clk_v1.yaml) | A136/F030/F072/F17x/F19x/L07x/L13x/L17x/L19x: CLK_TRIM | 7 / 21 | 相对 clk_trim_ddl_v1：定义差异 CR/ICLR/IFR |
| `clk_trim_ddl_v1_3.rs` | [clk_v4](../../data/registers/clk_v4.yaml) | L16x/L18x: CLK_TRIM | 7 / 21 | 相对 clk_trim_ddl_v1：定义差异 CR/ICLR/IFR |

### ctrim：v1

计数/捕获时钟校准；FLIM 位宽、校准源与控制位分变体。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `ctrim_ddl_v1.rs` | [ctrim_v2](../../data/registers/ctrim_v2.yaml) | F002: CTRIM | 10 / 41 | 本档位对照基准；完整定义见来源 YAML |
| `ctrim_ddl_v1_1.rs` | [ctrim_v3](../../data/registers/ctrim_v3.yaml) | F420: CTRIM | 10 / 42 | 相对 ctrim_ddl_v1：定义差异 IER/ISR |
| `ctrim_ddl_v1_2.rs` | [ctrim_v4](../../data/registers/ctrim_v4.yaml) | L021/L12x: CTRIM | 10 / 42 | 相对 ctrim_ddl_v1：定义差异 FLIM/ICR/IER/ISR |
| `ctrim_ddl_v1_3.rs` | [ctrim_v1](../../data/registers/ctrim_v1.yaml) | A052/F052: CTRIM | 10 / 43 | 相对 ctrim_ddl_v1：定义差异 CR1/FLIM/IER/ISR |
| `ctrim_ddl_v1_4.rs` | [ctrim_v5](../../data/registers/ctrim_v5.yaml) | L16x/L18x: CTRIM | 10 / 43 | 相对 ctrim_ddl_v1：定义差异 CR1/FLIM/ICR/IER/ISR |

### reset：v1

外设复位控制，分组规模及字段按变体保留。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `reset_ddl_v1.rs` | [reset_v2](../../data/registers/reset_v2.yaml) | F003/F005: RESET | 2 / 21 | 本档位对照基准；完整定义见来源 YAML |
| `reset_ddl_v1_1.rs` | [reset_v6](../../data/registers/reset_v6.yaml) | L110: RESET | 2 / 24 | 相对 reset_ddl_v1：增加 PREI_RESET；缺少 PERI_RESET |
| `reset_ddl_v1_2.rs` | [reset_v1](../../data/registers/reset_v1.yaml) | A136/F030/L13x: RESET | 2 / 37 | 相对 reset_ddl_v1：定义差异 PERI_RESET/RESET_FLAG |
| `reset_ddl_v1_3.rs` | [reset_v7](../../data/registers/reset_v7.yaml) | L12x: RESET | 3 / 36 | 相对 reset_ddl_v1：增加 PERI_RESET0/PERI_RESET1；缺少 PERI_RESET；定义差异 RESET_FLAG |
| `reset_ddl_v1_4.rs` | [reset_v4](../../data/registers/reset_v4.yaml) | F17x/F19x/L17x/L19x: RESET | 3 / 41 | 相对 reset_ddl_v1：增加 PERI_RESET0/PERI_RESET1；缺少 PERI_RESET；定义差异 RESET_FLAG |
| `reset_ddl_v1_5.rs` | [reset_v3](../../data/registers/reset_v3.yaml) | F072: RESET | 3 / 42 | 相对 reset_ddl_v1：增加 PERI_RESET0/PERI_RESET1；缺少 PERI_RESET；定义差异 RESET_FLAG |
| `reset_ddl_v1_6.rs` | [reset_v8](../../data/registers/reset_v8.yaml) | L16x/L18x: RESET | 3 / 42 | 相对 reset_ddl_v1：增加 PERI_RESET0/PERI_RESET1；缺少 PERI_RESET；定义差异 RESET_FLAG |
| `reset_ddl_v1_7.rs` | [reset_v5](../../data/registers/reset_v5.yaml) | L07x: RESET | 3 / 47 | 相对 reset_ddl_v1：增加 PERI_RESET0/PERI_RESET1；缺少 PERI_RESET；定义差异 RESET_FLAG |

### sysctrl：v1

内部 RC 时钟与基础门控，无外部晶振/PLL。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `sysctrl_ddl_v1.rs` | [sysctrl_v3](../../data/registers/sysctrl_v3.yaml) | F002: SYSCTRL | 12 / 69 | 本档位对照基准；完整定义见来源 YAML |

### sysctrl：v2

外部时钟或低速晶振控制，无 PLL。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `sysctrl_ddl_v2.rs` | [sysctrl_v4](../../data/registers/sysctrl_v4.yaml) | F003/F005: SYSCTRL | 9 / 56 | 本档位对照基准；完整定义见来源 YAML |
| `sysctrl_ddl_v2_1.rs` | [sysctrl_v11](../../data/registers/sysctrl_v11.yaml) | L12x: SYSCTRL | 9 / 73 | 相对 sysctrl_ddl_v2：增加 PERI_CLKEN0/PERI_CLKEN1；缺少 PERI_CLKEN/SYSTICK_CR；定义差异 RCH_CR/RCL_CR/SYSCTRL0/SYSCTRL1/XTH_CR 等 6 项 |
| `sysctrl_ddl_v2_2.rs` | [clock_common](../../data/registers/clock_common.yaml) | L110: CLOCK | 10 / 62 | 相对 sysctrl_ddl_v2：增加 DEBUG_ACTIVE；定义差异 PERI_CLKEN/RCH_CR/RCL_CR/SYSCTRL0/SYSCTRL1 等 8 项 |
| `sysctrl_ddl_v2_3.rs` | [sysctrl_v9](../../data/registers/sysctrl_v9.yaml) | L021: SYSCTRL | 12 / 82 | 相对 sysctrl_ddl_v2：增加 DEBUG_ACTIVE/PERI_CLKEN0/PERI_CLKEN1/PERI_RESET0/PERI_RESET1 等 7 项；缺少 PERI_CLKEN/RCH_CR/SYSTICK_CR/XTH_CR；定义差异 RCL_CR/SYSCTRL0/SYSCTRL1/SYSCTRL2/XTL_CR |

### sysctrl：v3

PLL 时钟；RC48M、门控及复位窗口按变体保留。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `sysctrl_ddl_v3.rs` | [sysctrl_v5](../../data/registers/sysctrl_v5.yaml) | F030: SYSCTRL | 9 / 68 | 本档位对照基准；完整定义见来源 YAML |
| `sysctrl_ddl_v3_1.rs` | [sysctrl_v2](../../data/registers/sysctrl_v2.yaml) | A136/L13x: SYSCTRL | 9 / 76 | 相对 sysctrl_ddl_v3：定义差异 PERI_CLKEN/SYSCTRL1 |
| `sysctrl_ddl_v3_2.rs` | [sysctrl_v7](../../data/registers/sysctrl_v7.yaml) | F17x/F19x/L17x/L19x: SYSCTRL | 10 / 80 | 相对 sysctrl_ddl_v3：增加 PERI_CLKEN0/PERI_CLKEN1；缺少 PERI_CLKEN；定义差异 SYSCTRL1 |
| `sysctrl_ddl_v3_3.rs` | [sysctrl_v12](../../data/registers/sysctrl_v12.yaml) | L16x/L18x: SYSCTRL | 11 / 87 | 相对 sysctrl_ddl_v3：增加 PERI_CLKEN0/PERI_CLKEN1/RC48M_CR；缺少 PERI_CLKEN；定义差异 SYSCTRL0/SYSCTRL1 |
| `sysctrl_ddl_v3_4.rs` | [sysctrl_v6](../../data/registers/sysctrl_v6.yaml) | F072: SYSCTRL | 12 / 84 | 相对 sysctrl_ddl_v3：增加 OVCK_CR/PERI_CLKEN0/PERI_CLKEN1/RC48M_CR；缺少 PERI_CLKEN；定义差异 SYSCTRL1 |
| `sysctrl_ddl_v3_5.rs` | [sysctrl_v10](../../data/registers/sysctrl_v10.yaml) | L07x: SYSCTRL | 12 / 91 | 相对 sysctrl_ddl_v3：增加 OVCK_CR/PERI_CLKEN0/PERI_CLKEN1/RC48M_CR；缺少 PERI_CLKEN；定义差异 SYSCTRL1 |
| `sysctrl_ddl_v3_6.rs` | [sysctrl_v1](../../data/registers/sysctrl_v1.yaml) | A052/F052: SYSCTRL | 18 / 160 | 相对 sysctrl_ddl_v3：增加 CR0/CR1/CR2/CR3/DBGSTOP 等 18 项；缺少 PERI_CLKEN/PLL_CR/RCH_CR/RCL_CR/SYSCTRL0 等 9 项 |

### sysctrl：v4

PLL 与 APB0/1、AHB0/1 四组门控/复位。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `sysctrl_ddl_v4.rs` | [sysctrl_v8](../../data/registers/sysctrl_v8.yaml) | F420: SYSCTRL | 21 / 134 | 本档位对照基准；完整定义见来源 YAML |

### vctrim：v1

比较器校准。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `vctrim_ddl_v1.rs` | [vctrim_common](../../data/registers/vctrim_common.yaml) | L12x: VCTRIM | 1 / 3 | 本档位对照基准；完整定义见来源 YAML |
<!-- peripheral-versions:end -->

## SVD 实例与基址

表中使用 SVD 的实例名与基址。SDK 目录与文件来源见 [SVD 总览](../svd-overview.md)。

| 系列 | SVD 实例与基址 |
|---|---|
| A052 | `CTRIM@0x40005000`, `SYSCTRL@0x40002000` |
| A136 | `CLK_TRIM@0x40001800`, `RESET@0x4000201C`, `SYSCTRL@0x40002000` |
| F002 | `CTRIM@0x40005000`, `SYSCTRL@0x40002000` |
| F003 | `CLK_TRIM@0x40001800`, `RESET@0x4000201C`, `SYSCTRL@0x40002000` |
| F005 | `CLK_TRIM@0x40001800`, `RESET@0x4000201C`, `SYSCTRL@0x40002000` |
| F030 | `CLK_TRIM@0x40001800`, `RESET@0x4000201C`, `SYSCTRL@0x40002000` |
| F052 | `CTRIM@0x40005000`, `SYSCTRL@0x40002000` |
| F072 | `CLK_TRIM@0x40001800`, `RESET@0x4000201C`, `SYSCTRL@0x40002000` |
| F17x | `CLK_TRIM@0x40001800`, `RESET@0x4000201C`, `SYSCTRL@0x40002000` |
| F19x | `CLK_TRIM@0x40001800`, `RESET@0x4000201C`, `SYSCTRL@0x40002000` |
| F420 | `CTRIM@0x40008000`, `SYSCTRL@0x4000B800` |
| L021 | `CTRIM@0x40009000`, `SYSCTRL@0x40021800` |
| L07x | `CLK_TRIM@0x40001800`, `RESET@0x4000201C`, `SYSCTRL@0x40002000` |
| L110 | `CLK_TRIM@0x40001800`, `CLOCK@0x40002000`, `RESET@0x4000201C` |
| L12x | `CTRIM@0x40009000`, `RESET@0x4002181C`, `SYSCTRL@0x40021800`, `VCTRIM@0x40002820` |
| L13x | `CLK_TRIM@0x40001800`, `RESET@0x4000201C`, `SYSCTRL@0x40002000` |
| L16x | `CLK_TRIM@0x40001800`, `CTRIM@0x40005000`, `RESET@0x4000201C`, `SYSCTRL@0x40002000` |
| L17x | `CLK_TRIM@0x40001800`, `RESET@0x4000201C`, `SYSCTRL@0x40002000` |
| L18x | `CLK_TRIM@0x40001800`, `CTRIM@0x40005000`, `RESET@0x4000201C`, `SYSCTRL@0x40002000` |
| L19x | `CLK_TRIM@0x40001800`, `RESET@0x4000201C`, `SYSCTRL@0x40002000` |
