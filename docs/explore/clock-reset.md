# 时钟、复位与校准

## Cortex-M0/M0+

旧式 SYSCTRL 负责系统时钟和外设门控。L110 的节点名为 CLOCK；部分系列的 RESET 由 SVD 单列。

| 建议版本 | 系列或实例 | 布局备注 |
|---|---|---|
| `sysctrl_v1` | F17x F19x L17x L19x | SYSCTRL0–2、振荡器控制和 PERI_CLKEN0/1。 |
| `sysctrl_v2` | A136 L13x | 门控寄存器为 PERI_CLKEN。 |
| `sysctrl_v3` | F030 | 单组 PERI_CLKEN，字段与 A136/L13x 有差异。 |
| `sysctrl_v4` | A052 F052 | CR0–3、PERICLKEN0/1、PERIRESET0/1。 |
| `sysctrl_v5` | F003 F005 | 含 SYSTICK_CR，无 PLL。 |
| `sysctrl_v6` | F072 | 含 OVCK_CR 和 RC48M_CR。 |
| `sysctrl_v7` | L07x | 含 OVCK_CR 和 RC48M_CR，字段与 F072 有差异。 |
| `sysctrl_v8` | L16x L18x | 含 RC48M_CR，无 XTL_CR。 |
| `sysctrl_v9` | F002 | 无 XTH、XTL、PLL 控制。 |
| `sysctrl_v10` | L021 | SYSCTRL@0x40021800。 |
| `sysctrl_v11` | L12x | SYSCTRL@0x40021800，单列 RESET 窗口。 |
| `sysctrl_v12` | L110 | 节点名 CLOCK，与 F003/F005 布局相近。 |
| `reset_v1` | F17x F19x L17x L19x | RESET_FLAG 和 PERI_RESET0/1。 |
| `reset_v2` | F072 | RESET_FLAG、PERI_RESET0/1；位域按系列记录。 |
| `reset_v3` | L07x | RESET_FLAG、PERI_RESET0/1；位域按系列记录。 |
| `reset_v4` | L16x L18x | RESET_FLAG、PERI_RESET0/1；位域按系列记录。 |
| `reset_v5` | L12x | RESET_FLAG、PERI_RESET0/1，基址 0x4002181C。 |
| `reset_v6` | A136 F030 L13x | RESET_FLAG、PERI_RESET。 |
| `reset_v7` | F003 F005 | RESET_FLAG、PERI_RESET；位域按系列记录。 |
| `reset_v8` | L110 | SVD 与 SDK 设备头均写作 PREI_RESET。 |
| `clktrim_v1` | A136 F030 F072 F17x F19x L07x L13x L17x L19x | — |
| `clktrim_v2` | F003 F005 | — |
| `clktrim_v3` | L16x L18x | — |
| `clktrim_v4` | L110 | — |
| `ctrim_v1` | A052 F052 L16x L18x | — |
| `ctrim_v2` | F002 | — |
| `ctrim_v3` | L021 | — |
| `ctrim_v4` | L12x | — |
| `vctrim_v1` | L12x | VC 的单寄存器校准块。 |

## HC32F420（Cortex-M4）

SYSCTRL 归 `sysctrl_v14`，有 21 个寄存器，时钟门控分为 APB0/1 和 AHB0/1 四组。
CTRIM 归 `ctrim_v6`，与 A052/F052 同骨架但 `FLIM.FLIM` 位宽为 8 位（A052/F052 为 12 位），
且缺少 `CR1.MDRCH`。设备头中 `CLK_FAULT_IRQn=9`、`CTRIM_IRQn=0`。
`SYSCTRL_PeriphClkEnable()` 和 `SYSCTRL_PeriphClkDisable()` 按门控枚举值选择寄存器。
枚举组标记 `0x00/0x20/0x40/0x80` 分别对应 APB0EN/APB1EN/AHB0EN/AHB1EN。
CTRIM 门控在 APB1EN bit 0。

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
