# 电源、低功耗与看门狗

## Cortex-M0/M0+

LVD 分 lvd_v1/v2，WDT 分 wdt_v1/v2（L110 的字段别名另记）；IWDT 和 WWDT 各分 v1/v2；L110 另有 lpt_v1。

| 建议版本 | 系列或实例 | 布局备注 |
|---|---|---|
| `lvd_v1` | A052, F002, F052, L021, L12x | CR@0x0 世代；字段命名漂移在 yaml 取最常见名并备注 |
| `lvd_v2` | A136, F003, F005, F030, F072, F17x, F19x, L07x, L110, L13x, L16x, L17x, L18x, L19x | CR@0x28 世代；F003/F005/L110 的 reset/FILTER 差异备注 |
| `wdt_v1` | A136, F003, F005, F030, F072, F17x, F19x, L07x, L13x, L17x, L19x | RST@0x80 |
| `wdt_v1l110` | L110 | 同 v1 偏移、字段重命名 WDTRST/WDTR/WDINT |
| `wdt_v2` | L16x, L18x | RST@0x0 |
| `iwdt_v1` | A052, F002, F052, L12x | 窗口 0x00 |
| `iwdt_v2` | L021 | 窗口 0x80 |
| `wwdt_v1` | A052, F002, F052 | CR0 rst=0x0 |
| `wwdt_v2` | L12x, L16x, L18x | CR0 rst=0x7F |
| `lpt_v1` | L110 | 仅 L110，0x60 偏移窗口 |

## HC32F420（Cortex-M4）

LVD 沿用 `lvd_v1`（F002 变体），IWDT/WWDT 沿用 `iwdt_v1/wwdt_v1`。
`IRQn_Type` 中分别为 LVD 2、IWDT 7 和 WWDT 8。

## SVD 实例与基址

表中使用 SVD 的实例名与基址。SDK 目录与文件来源见 [SVD 总览](../svd-overview.md)。

| 系列 | SVD 实例与基址 |
|---|---|
| A052 | `IWDT@0x40001080`, `LVD@0x40002480`, `WWDT@0x40006800` |
| A136 | `LPTIMER@0x40000F00`, `LVD@0x40002400`, `WDT@0x40000F00` |
| F002 | `IWDT@0x40000F80`, `LVD@0x40002680`, `WWDT@0x40006800` |
| F003 | `LVD@0x40002400`, `WDT@0x40000C00` |
| F005 | `LVD@0x40002400`, `WDT@0x40000C00` |
| F030 | `LVD@0x40002400`, `WDT@0x40000F00` |
| F052 | `IWDT@0x40001080`, `LVD@0x40002480`, `WWDT@0x40006800` |
| F072 | `LVD@0x40002400`, `WDT@0x40000F00` |
| F17x | `LVD@0x40002400`, `WDT@0x40000F00` |
| F19x | `LVD@0x40002400`, `WDT@0x40000F00` |
| F420 | `IWDT@0x40008C80`, `LVD@0x4000AA00`, `WWDT@0x40008800` |
| L021 | `IWDT@0x40001000`, `LVD@0x40002C00` |
| L07x | `LVD@0x40002400`, `WDT@0x40000F00` |
| L110 | `LPTIMER@0x40000C00`, `LVD@0x40002400`, `WDT@0x40000C00` |
| L12x | `IWDT@0x40001080`, `LVD@0x40002C00`, `WWDT@0x4000B800` |
| L13x | `LPTIMER@0x40000F00`, `LVD@0x40002400`, `WDT@0x40000F00` |
| L16x | `LVD@0x40002400`, `WDT@0x40000F80`, `WWDT@0x40007800` |
| L17x | `LVD@0x40002400`, `WDT@0x40000F00` |
| L18x | `LVD@0x40002400`, `WDT@0x40000F80`, `WWDT@0x40007800` |
| L19x | `LVD@0x40002400`, `WDT@0x40000F00` |
