# SVD 总览（20 个系列）

每份 SVD 对应表中 SDK 内的一份 SVD（内容一致；git 入库时行尾归一为 LF）。设备名和 CPU 标签分别取 XML 顶层
`<name>` 与 `<cpu><name>`。外设数和派生数按顶层 peripheral 统计，中断数取 XML 中
`<interrupt>` 节点数。生成数据时需展开 `derivedFrom`，并用设备头核对中断和内存映射。

| SVD | SDK 来源（`xh_ddl_sdk/`） | 设备名 | CPU | 外设数 | 派生外设数 | 中断节点数 |
|---|---|---|---|---:|---:|---:|
| [HC32A052.svd](../svd/HC32A052.svd) | `HC32A052_DLL_Rev1.0.1/mcu/EWARM/hc32A052.svd` | XHSC_HC32A052 | CM0 | 51 | 19 | 0 |
| [HC32A136.svd](../svd/HC32A136.svd) | `HC32A136_DDL_Rev2.1.0/mcu/EWARM/HC32A136.svd` | XHSC_HC32A136 | CM0 | 46 | 6 | 0 |
| [HC32F002.svd](../svd/HC32F002.svd) | `HC32F002_DDL_Rev1.2.1/mcu/EWARM/HC32F002.svd` | XHSC_HC32F002 | CM0P | 23 | 6 | 0 |
| [HC32F003.svd](../svd/HC32F003.svd) | `HC32F003_DDL_Rev2.0.0/mcu/EWARM/HC32F003.svd` | HDSC_HC32F003 | CM0 | 24 | 5 | 0 |
| [HC32F005.svd](../svd/HC32F005.svd) | `HC32F005_DDL_Rev2.0.0/mcu/EWARM/HC32F005.svd` | HDSC_HC32F005 | CM0 | 24 | 5 | 0 |
| [HC32F030.svd](../svd/HC32F030.svd) | `HC32F030_DDL_Rev2.2.0/mcu/EWARM/HC32F030.svd` | HDSC_HC32F030 | CM0 | 40 | 5 | 0 |
| [HC32F052.svd](../svd/HC32F052.svd) | `HC32F052_DDL_Rev1.1.0/mcu/EWARM/hc32F052.svd` | XHSC_HC32F052 | CM0 | 51 | 19 | 0 |
| [HC32F072.svd](../svd/HC32F072.svd) | `HC32F072_DDL_Rev1.2.0/mcu/EWARM/HC32F072.svd` | HDSC_HC32F072 | CM0 | 50 | 9 | 0 |
| [HC32F17x.svd](../svd/HC32F17x.svd) | `HC32F17x_DDL_Rev1.4.0/mcu/EWARM/HC32F17X.svd` | HDSC_HC32F17X | CM0 | 43 | 7 | 0 |
| [HC32F19x.svd](../svd/HC32F19x.svd) | `HC32F19x_DDL_Rev1.4.0/mcu/EWARM/HC32F19X.svd` | HDSC_HC32F19X | CM0 | 43 | 7 | 0 |
| [HC32F420.svd](../svd/HC32F420.svd) | `HC32F420_DDL_Rev1.3.1/mcu/EWARM/HC32F420.svd` | XHSC_HC32F420 | CM4 | 43 | 17 | 0 |
| [HC32L021.svd](../svd/HC32L021.svd) | `HC32L021_DDL_Rev1.2.1/mcu/EWARM/HC32L021.svd` | XHSC_HC32L021 | CM0+ | 27 | 7 | 0 |
| [HC32L07x.svd](../svd/HC32L07x.svd) | `HC32L07x_DDL_Rev1.2.0/mcu/EWARM/HC32L07X.svd` | HDSC_HC32L07X | CM0 | 55 | 10 | 0 |
| [HC32L110.svd](../svd/HC32L110.svd) | `HC32L110_DDL_Rev1.3.2/mcu/GCC/HC32L110.svd` | HDSC_HC32L110 | CM0 | 26 | 5 | 0 |
| [HC32L12x.svd](../svd/HC32L12x.svd) | `HC32L12x_DDL_Rev1.1.0/mcu/ewarm/hc32l12x.svd` | XHSC_HC32L12X | CM0P | 44 | 15 | 0 |
| [HC32L13x.svd](../svd/HC32L13x.svd) | `HC32L13x_DDL_Rev2.2.0/mcu/EWARM/HC32L13X.svd` | HDSC_HC32L13X | CM0 | 46 | 6 | 0 |
| [HC32L16x.svd](../svd/HC32L16x.svd) | `HC32L16x_DDL_Rev1.1.1/mcu/EWARM/HC32L166.svd` | HDSC_HC32L166 | CM0 | 51 | 9 | 0 |
| [HC32L17x.svd](../svd/HC32L17x.svd) | `HC32L17x_DDL_Rev1.4.0/mcu/EWARM/HC32L17X.svd` | HDSC_HC32L17X | CM0 | 49 | 9 | 0 |
| [HC32L18x.svd](../svd/HC32L18x.svd) | `hc32l186_ddl/mcu/EWARM/HC32L186.svd`（Rev1.1.0） | HDSC_HC32L186 | CM0 | 51 | 9 | 0 |
| [HC32L19x.svd](../svd/HC32L19x.svd) | `HC32L19x_DDL_Rev1.4.0/mcu/EWARM/HC32L19X.svd` | HDSC_HC32L19X | CM0 | 49 | 9 | 0 |

L18x 的 SDK 版本见包内 `ChangeLog.md`。除 F420 的设备头引用 `core_cm4.h` 外，其余
19 个系列的设备头均引用 `core_cm0plus.h`，表中仍保留各份 SVD 的原始 CPU 标签。
