# PCA、BT、PCNT 与 EAU

PCA 的基础布局有 25 个寄存器，扩展增加 CARR@0x64 和 EPWM@0x68，使用同主档位的数字后缀。
PCNT 的基础计数布局有 13 个寄存器，DBG@0x34 的扩展保留后缀。
EAU 包含 CSR、DIVIDEND、DIVISOR、QUOTIENT、REMAINDER。

本页的 bt 仅指 L110 的 BT0–2 紧凑实例。原文将 TIM 的 MODE 视图也称为 BT 版本，
容易与真实 BT 实例混淆，现统一在[通用定时器](timer-gen.md)按 tim 登记。
BTIM/GTIM 也在该页。F420 无 PCA、PCNT 或 AOS，包含 BTIM 与 GTIM。

[编号规则](../layout-versions.md) · [实例 CSV](../peripheral-versions.csv) · [SDK 核验](../sdk-verification.md)。

<!-- peripheral-versions:begin -->
## 版本与系列对照

以下为待确认命名，当前来源仍使用旧文件名。系列名前缀 `HC32` 省略。

### bt：v1

紧凑基本定时器。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `bt_ddl_v1.rs` | [bt_common](../../data/registers/bt_common.yaml) | L110: BT0, BT1, BT2 | 6 / 13 | 本档位对照基准；完整定义见来源 YAML |

### eau：v1

算术除法控制。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `eau_ddl_v1.rs` | [eau_common](../../data/registers/eau_common.yaml) | A052/F052: EAU | 5 / 9 | 本档位对照基准；完整定义见来源 YAML |

### pca：v1

基础计数/捕获；CARR/EPWM 扩展。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `pca_ddl_v1.rs` | [pca_v3](../../data/registers/pca_v3.yaml) | L110: PCA | 25 / 73 | 本档位对照基准；完整定义见来源 YAML |
| `pca_ddl_v1_1.rs` | [pca_v2](../../data/registers/pca_v2.yaml) | F003/F005: PCA | 25 / 98 | 相对 pca_ddl_v1：定义差异 CCAP0/CCAP0H/CCAP0L/CCAP1/CCAP1H 等 25 项 |
| `pca_ddl_v1_2.rs` | [pca_v1](../../data/registers/pca_v1.yaml) | A136/F030/F072/F17x/F19x/L07x/L13x/L16x/L17x/L18x/L19x: PCA | 27 / 102 | 相对 pca_ddl_v1：增加 CARR/EPWM；定义差异 CCAP0/CCAP0H/CCAP0L/CCAP1/CCAP1H 等 25 项 |

### pcnt：v1

基础脉冲计数；DBG 扩展。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `pcnt_ddl_v1.rs` | [pcnt_v1](../../data/registers/pcnt_v1.yaml) | A136/L13x: PCNT | 13 / 58 | 本档位对照基准；完整定义见来源 YAML |
| `pcnt_ddl_v1_1.rs` | [pcnt_v2](../../data/registers/pcnt_v2.yaml) | L07x/L16x/L17x/L18x/L19x: PCNT | 14 / 60 | 相对 pcnt_ddl_v1：增加 DBG |
<!-- peripheral-versions:end -->

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
