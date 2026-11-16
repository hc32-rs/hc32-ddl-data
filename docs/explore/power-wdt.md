# 电源、低功耗与看门狗

LVD/WDT/IWDT/WWDT 保留同档布局变体，偏移、阈值、滤波、锁定和字段方言不自动合并。
LVD 有 CR@0 与 CR@0x28 两类窗口；L12x 的 CR/IFR 布局也独立保留。
WDT 的 RST@0x80 与 RST@0x00 必须区分，L110 的 WDTRST/WDTR/WDINT 命名单独保留。
LPTIM/LPTIMER 统一在[通用定时器](timer-gen.md)登记，不再在电源页重复编号。

F420 LVD 使用 F002 的生成布局，IWDT/WWDT 使用 A052/F052 对应布局，IRQ 分别为 2/7/8。
现有生成 IR 对 WWDT 采用 common 布局，不因只存在于原始 SVD 的复位值差异虚构新生成布局。
复位值差异仍以 SVD 和审计 CSV 为准，编号不能替代这些数据。

[编号规则](../layout-versions.md) · [实例 CSV](../peripheral-versions.csv) · [SDK 核验](../sdk-verification.md)。

<!-- peripheral-versions:begin -->
## 版本与系列对照

以下为待确认命名，当前来源仍使用旧文件名。系列名前缀 `HC32` 省略。

### iwdt：v1

同档控制；窗口偏移、锁定、阈值/滤波或字段方言分变体。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `iwdt_ddl_v1.rs` | [iwdt_v1](../../data/registers/iwdt_v1.yaml) | A052/F052/F420: IWDT | 5 / 17 | 本档位对照基准；完整定义见来源 YAML |
| `iwdt_ddl_v1_1.rs` | [iwdt_v2](../../data/registers/iwdt_v2.yaml) | F002: IWDT | 5 / 17 | 相对 iwdt_ddl_v1：定义差异 CR |
| `iwdt_ddl_v1_2.rs` | [iwdt_v3](../../data/registers/iwdt_v3.yaml) | L021: IWDT | 5 / 17 | 相对 iwdt_ddl_v1：定义差异 ARR/CR/KR/SR/WINR |
| `iwdt_ddl_v1_3.rs` | [iwdt_v4](../../data/registers/iwdt_v4.yaml) | L12x: IWDT | 5 / 17 | 相对 iwdt_ddl_v1：定义差异 SR |

### lvd：v1

同档控制；窗口偏移、锁定、阈值/滤波或字段方言分变体。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `lvd_ddl_v1.rs` | [lvd_v6](../../data/registers/lvd_v6.yaml) | L110: LVD | 2 / 11 | 本档位对照基准；完整定义见来源 YAML |
| `lvd_ddl_v1_1.rs` | [lvd_v4](../../data/registers/lvd_v4.yaml) | F003/F005: LVD | 2 / 13 | 相对 lvd_ddl_v1：定义差异 CR/IFR |
| `lvd_ddl_v1_2.rs` | [lvd_v1](../../data/registers/lvd_v1.yaml) | A052/F052: LVD | 2 / 14 | 相对 lvd_ddl_v1：增加 SR；缺少 IFR；定义差异 CR |
| `lvd_ddl_v1_3.rs` | [lvd_v2](../../data/registers/lvd_v2.yaml) | A136/F030/F072/F17x/F19x/L07x/L13x/L16x/L17x/L18x/L19x: LVD | 2 / 14 | 相对 lvd_ddl_v1：定义差异 CR/IFR |
| `lvd_ddl_v1_4.rs` | [lvd_v3](../../data/registers/lvd_v3.yaml) | F002/F420: LVD | 2 / 14 | 相对 lvd_ddl_v1：增加 SR；缺少 IFR；定义差异 CR |
| `lvd_ddl_v1_5.rs` | [lvd_v5](../../data/registers/lvd_v5.yaml) | L021: LVD | 2 / 14 | 相对 lvd_ddl_v1：增加 SR；缺少 IFR；定义差异 CR |
| `lvd_ddl_v1_6.rs` | [lvd_v7](../../data/registers/lvd_v7.yaml) | L12x: LVD | 2 / 14 | 相对 lvd_ddl_v1：定义差异 CR/IFR |

### wdt：v1

同档控制；窗口偏移、锁定、阈值/滤波或字段方言分变体。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `wdt_ddl_v1.rs` | [wdt_v2](../../data/registers/wdt_v2.yaml) | L110: WDT | 2 / 6 | 本档位对照基准；完整定义见来源 YAML |
| `wdt_ddl_v1_1.rs` | [wdt_v1](../../data/registers/wdt_v1.yaml) | A136/F003/F005/F030/F072/F17x/F19x/L07x/L13x/L17x/L19x: WDT | 2 / 8 | 相对 wdt_ddl_v1：定义差异 CON/RST |
| `wdt_ddl_v1_2.rs` | [wdt_v3](../../data/registers/wdt_v3.yaml) | L16x/L18x: WDT | 2 / 8 | 相对 wdt_ddl_v1：定义差异 CON/RST |

### wwdt：v1

同档控制；窗口偏移、锁定、阈值/滤波或字段方言分变体。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `wwdt_ddl_v1.rs` | [wwdt_common](../../data/registers/wwdt_common.yaml) | A052/F002/F052/F420/L12x/L16x/L18x: WWDT | 3 / 9 | 本档位对照基准；完整定义见来源 YAML |
<!-- peripheral-versions:end -->

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
