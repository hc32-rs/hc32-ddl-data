# DMA

DDL 的实例名为 DMAC，目标外设名使用 dmac。旧 CONF/CONFA 架构按源地址通道窗口规模分档，
ST、TRI_SEL 位宽、命名及通道数量分别保留为布局变体。
A136/F030/L13x 的 TRI_SEL 为 [28:23]；后续两通道组为 [28:22]。
A052/F052/L12x/L16x/L18x/F420 的 CONF 无 ST，不能与有 ST 的布局混用。

A052/F052 的 SVD 与设备头描述通道 0–5，但 `driver/inc/dmac.h` 的 DmaCh 枚举和
`driver/src/dmac.c` 参数检查只覆盖 0–4。对照表保留六组寄存器，驱动支持范围另记。
F420 的驱动支持六通道，固定 IRQ 51–56，基址 0x40020000，门控 AHB0EN bit 0。
F420 的 TRI_SEL/FIS_IE/ERR_IE 与 A052/F052 的 TRISEL/FISIE/ERRIE 名称不同，保留布局后缀。
14 个带 DMAC 的 DDL M0/M0+ 系列在设备头使用共享 DMAC_IRQn=4。
L12x 通道 1 的 SRCADR/DSTADR 偏移为 0x28/0x2C。

| F420 请求源 | 触发值 |
|---|---|
| 软件 | `0x00` |
| USART0–3 TX/RX（交错） | `0x40–0x47` |
| GTIM0–2 | `0x48–0x4A` |
| ADC0/1 顺序单次转换完成 | `0x4D/0x4F` |
| SPI0/1 TX/RX（交错） | `0x50–0x53` |
| ATIM0 A/B、ATIM3 A/B | `0x58/0x59`、`0x5E/0x5F` |

[编号规则](../layout-versions.md) · [实例 CSV](../peripheral-versions.csv) · [SDK 核验](../sdk-verification.md)。

<!-- peripheral-versions:begin -->
## 版本与系列对照

以下为待确认命名，当前来源仍使用旧文件名。系列名前缀 `HC32` 省略。

### dmac：v1

旧式 CONF/CONFA DMA；2 个源地址窗口，触发位与字段方言分变体。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `dmac_ddl_v1.rs` | [dmac_v5](../../data/registers/dmac_v5.yaml) | L12x: DMAC | 9 / 41 | 本档位对照基准；完整定义见来源 YAML |
| `dmac_ddl_v1_1.rs` | [dmac_v6](../../data/registers/dmac_v6.yaml) | L16x/L18x: DMAC | 9 / 41 | 相对 dmac_ddl_v1：增加 DSTADR1/SRCADR1；缺少 DSTADR/SRCADR |
| `dmac_ddl_v1_2.rs` | [dmac_v2](../../data/registers/dmac_v2.yaml) | A136/F030/L13x: DMAC | 9 / 42 | 相对 dmac_ddl_v1：增加 DSTADR1/SRCADR1；缺少 DSTADR/SRCADR；定义差异 CONF/CONFA0/CONFA1 |
| `dmac_ddl_v1_3.rs` | [dmac_v3](../../data/registers/dmac_v3.yaml) | F072/F17x/F19x/L07x/L17x/L19x: DMAC | 9 / 42 | 相对 dmac_ddl_v1：增加 DSTADR1/SRCADR1；缺少 DSTADR/SRCADR；定义差异 CONF |

### dmac：v2

旧式 CONF/CONFA DMA；6 个源地址窗口，触发位与字段方言分变体。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `dmac_ddl_v2.rs` | [dmac_v1](../../data/registers/dmac_v1.yaml) | A052/F052: DMAC | 25 / 117 | 本档位对照基准；完整定义见来源 YAML |
| `dmac_ddl_v2_1.rs` | [dmac_v4](../../data/registers/dmac_v4.yaml) | F420: DMAC | 25 / 117 | 相对 dmac_ddl_v2：定义差异 CONFA0/CONFA1/CONFA2/CONFA3/CONFA4 等 12 项 |
<!-- peripheral-versions:end -->

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
