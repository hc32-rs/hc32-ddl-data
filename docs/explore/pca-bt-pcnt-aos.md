# PCA、BT、PCNT、AOS 与 EAU

## Cortex-M0/M0+

PCA 分为 `pca_v1` 和 `pca_v1_1`，BT 为 bt_v1–v5，PCNT 为 `pcnt_v1` 和 `pcnt_v1_1`，A052/F052 的 EAU 为 eau_v1；BTIM 与定时器组共用分类。

| 建议版本 | 系列或实例 | 布局备注 |
|---|---|---|
| `pca_v1` | F003, F005, L110 | 25 寄存器，无 CARR/EPWM |
| `pca_v1_1` | A136, F030, F072, F17x, F19x, L07x, L13x, L16x, L17x, L18x, L19x | 27 寄存器，含 CARR@0x64、EPWM@0x68 |
| `bt_v1` | A136, F030, L13x | mode-split，MODE23 子簇 a（MODE0 全部相同） |
| `bt_v2` | F072, F17x, F19x, L07x, L17x, L19x | mode-split，MODE23 子簇 b |
| `bt_v3` | L12x, L16x, L18x | mode-split，MODE1/23 均独立成簇；L12x 基地址异（0x40003000 起）但布局同簇 |
| `bt_v4` | F003, F005 | compact，6 寄存器，CTEN/GATEP 字段体系 |
| `bt_v5` | L110 | compact，BT0/1/2 命名，GATE_P/TR 字段体系 |
| `btim_v1` | A052, F052, L021, F002 | 唯一布局；AIFR/AICR 标注为 6 定时器共享全局寄存器 |
| `pcnt_v1` | A136, L13x | 13 寄存器，无 DBG |
| `pcnt_v1_1` | L07x, L16x, L17x, L18x, L19x | 14 寄存器，含 DBG@0x34 |
| `eau_v1` | A052, F052 | CSR+DIVIDEND+DIVISOR+QUOTIENT+REMAINDER |

## HC32F420（Cortex-M4）

F420 的 SVD 与驱动目录无 PCA、PCNT 或 AOS，包含 BTIM 与 GTIM。
[通用定时器](timer-gen.md)列出 CTIM 的共享视图和 BTIM 字段命名差异。

## SVD 实例与基址

表中使用 SVD 的实例名与基址。SDK 目录与文件来源见 [SVD 总览](../svd-overview.md)。

| 系列 | SVD 实例与基址 |
|---|---|
| A052 | `BTIM0@0x40001800`, `BTIM1@0x40001900`, `BTIM2@0x40001A00`, `BTIM3@0x40001C00`, `BTIM4@0x40001D00`, `BTIM5@0x40001E00`, `EAU@0x40021800` |
| A136 | `PCA@0x40001000`, `PCNT@0x40005400` |
| F002 | `BTIM3@0x40007400`, `BTIM4@0x40007500`, `BTIM5@0x40007600` |
| F003 | `PCA@0x40001000` |
| F005 | `PCA@0x40001000` |
| F030 | `PCA@0x40001000` |
| F052 | `BTIM0@0x40001800`, `BTIM1@0x40001900`, `BTIM2@0x40001A00`, `BTIM3@0x40001C00`, `BTIM4@0x40001D00`, `BTIM5@0x40001E00`, `EAU@0x40021800` |
| F072 | `PCA@0x40001000` |
| F17x | `PCA@0x40001000` |
| F19x | `PCA@0x40001000` |
| F420 | `BTIM0@0x40005000`, `BTIM1@0x40005100`, `BTIM2@0x40005200`, `BTIM3@0x40005400`, `BTIM4@0x40005500`, `BTIM5@0x40005600`, `BTIM6@0x40005800`, `BTIM7@0x40005900`, `BTIM8@0x40005A00` |
| L021 | `BTIM0@0x40001800`, `BTIM1@0x40001900`, `BTIM2@0x40001A00`, `BTIM3@0x40001C00`, `BTIM4@0x40001D00`, `BTIM5@0x40001E00` |
| L07x | `PCA@0x40001000`, `PCNT@0x40005400` |
| L110 | `BT0@0x40000C00`, `BT1@0x40000C20`, `BT2@0x40000C40`, `PCA@0x40001000` |
| L13x | `PCA@0x40001000`, `PCNT@0x40005400` |
| L16x | `PCA@0x40001000`, `PCNT@0x40005400` |
| L17x | `PCA@0x40001000`, `PCNT@0x40005400` |
| L18x | `PCA@0x40001000`, `PCNT@0x40005400` |
| L19x | `PCA@0x40001000`, `PCNT@0x40005400` |
