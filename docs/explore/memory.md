# Flash 与 RAM 控制

FLASH 先区分紧凑操作控制与 CACHE 扩展，再列显式编程/擦除时序、锁区扩展及额外读写时序组。
紧凑型与时序型为不同架构，主档位仅表达控制复杂度，不表示前者被后者包含。
原先 F002 被排在末尾、L021/L12x 被按发现顺序编号的做法不再沿用。

时序型的 SLOCK 16/32 位、DPSTB_EN、SLOCK0–3、TPGS 位宽和 L16x/L18x 的 CONTP/RO、
额外时序组均保留在各自来源布局。RAM 依错误地址覆盖 11/12/13/14 位分档，CHKEN 等控制位分变体。

F420 的紧凑 FLASH 在 +0x0C 增加 CACHE，使 IER/IFR/ICR/BYPASS 后移四字节。
控制器基址 0x40021400，IRQ 11，门控 AHB0EN bit 5。
`example/flash/flash_erase_write/EWARM/hc32f420.icf` 的示例为 128 KiB Flash 和 24 KiB RAM。
容量与链接地址取芯片/链接脚本数据，不编码进外设版本。

[编号规则](../layout-versions.md) · [实例 CSV](../peripheral-versions.csv) · [SDK 核验](../sdk-verification.md)。

<!-- peripheral-versions:begin -->
## 版本与系列对照

以下名称与生成输入保持一致。系列名前缀 `HC32` 省略。

### flash：v1

紧凑操作控制；锁区与低功耗分支分变体。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `flash_ddl_v1.rs` | [flash_ddl_v1](../../data/registers/flash_ddl_v1.yaml) | L021: FLASH | 6 / 19 | 本档位对照基准；完整定义见来源 YAML |
| `flash_ddl_v1_1.rs` | [flash_ddl_v1_1](../../data/registers/flash_ddl_v1_1.yaml) | L12x: FLASH | 7 / 20 | 相对 flash_ddl_v1：增加 SLOCK1 |
| `flash_ddl_v1_2.rs` | [flash_ddl_v1_2](../../data/registers/flash_ddl_v1_2.yaml) | F002: FLASH | 7 / 20 | 相对 flash_ddl_v1：增加 LOCKSTATE/SLOCK/WAIT；缺少 LOCK_ST/SLOCK0；定义差异 BYPASS/CR/ICLR/IFR |
| `flash_ddl_v1_3.rs` | [flash_ddl_v1_3](../../data/registers/flash_ddl_v1_3.yaml) | A052/F052: FLASH | 9 / 24 | 相对 flash_ddl_v1：增加 ICR/IER/LOCKST/SLOCK1/WAIT；缺少 ICLR/LOCK_ST；定义差异 BYPASS/CR/IFR |

### flash：v2

紧凑操作控制与 CACHE。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `flash_ddl_v2.rs` | [flash_ddl_v2](../../data/registers/flash_ddl_v2.yaml) | F420: FLASH | 10 / 25 | 本档位对照基准；完整定义见来源 YAML |

### flash：v3

显式编程/擦除时序；锁区扩展，再到额外读写时序组。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `flash_ddl_v3.rs` | [flash_ddl_v3](../../data/registers/flash_ddl_v3.yaml) | L110: FLASH | 13 / 18 | 本档位对照基准；完整定义见来源 YAML |
| `flash_ddl_v3_1.rs` | [flash_ddl_v3_1](../../data/registers/flash_ddl_v3_1.yaml) | F003/F005: FLASH | 13 / 30 | 相对 flash_ddl_v3：定义差异 BYPASS/CR/ICLR/IFR/TMERASE 等 12 项 |
| `flash_ddl_v3_2.rs` | [flash_ddl_v3_2](../../data/registers/flash_ddl_v3_2.yaml) | A136/F030/L13x: FLASH | 13 / 31 | 相对 flash_ddl_v3：定义差异 BYPASS/CR/ICLR/IFR/SLOCK 等 13 项 |
| `flash_ddl_v3_3.rs` | [flash_ddl_v3_3](../../data/registers/flash_ddl_v3_3.yaml) | F072/L07x: FLASH | 14 / 32 | 相对 flash_ddl_v3：增加 SLOCK0/SLOCK1；缺少 SLOCK；定义差异 BYPASS/CR/ICLR/IFR/TMERASE 等 12 项 |
| `flash_ddl_v3_4.rs` | [flash_ddl_v3_4](../../data/registers/flash_ddl_v3_4.yaml) | F17x/L17x: FLASH | 14 / 32 | 相对 flash_ddl_v3：增加 SLOCK0/SLOCK1；缺少 SLOCK；定义差异 BYPASS/CR/ICLR/IFR/TMERASE 等 12 项 |

### flash：v4

显式编程/擦除时序；锁区扩展，再到额外读写时序组。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `flash_ddl_v4.rs` | [flash_ddl_v4](../../data/registers/flash_ddl_v4.yaml) | F19x/L19x: FLASH | 16 / 34 | 本档位对照基准；完整定义见来源 YAML |

### flash：v5

显式编程/擦除时序；锁区扩展，再到额外读写时序组。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `flash_ddl_v5.rs` | [flash_ddl_v5](../../data/registers/flash_ddl_v5.yaml) | L16x/L18x: FLASH | 24 / 52 | 本档位对照基准；完整定义见来源 YAML |

### ram：v1

RAM 错误地址覆盖：11 → 12 → 13 → 14 位；控制字段按变体保留。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `ram_ddl_v1.rs` | [ram_ddl_v1](../../data/registers/ram_ddl_v1.yaml) | F003/F005/L110: RAM | 4 / 5 | 本档位对照基准；完整定义见来源 YAML |

### ram：v2

RAM 错误地址覆盖：11 → 12 → 13 → 14 位；控制字段按变体保留。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `ram_ddl_v2.rs` | [ram_ddl_v2](../../data/registers/ram_ddl_v2.yaml) | A136/F030/L13x: RAM | 4 / 8 | 本档位对照基准；完整定义见来源 YAML |

### ram：v3

RAM 错误地址覆盖：11 → 12 → 13 → 14 位；控制字段按变体保留。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `ram_ddl_v3.rs` | [ram_ddl_v3](../../data/registers/ram_ddl_v3.yaml) | A052/F052: RAM | 4 / 8 | 本档位对照基准；完整定义见来源 YAML |
| `ram_ddl_v3_1.rs` | [ram_ddl_v3_1](../../data/registers/ram_ddl_v3_1.yaml) | F072/F17x/L07x/L12x/L17x: RAM | 4 / 8 | 相对 ram_ddl_v3：定义差异 ICLR |

### ram：v4

RAM 错误地址覆盖：11 → 12 → 13 → 14 位；控制字段按变体保留。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `ram_ddl_v4.rs` | [ram_ddl_v4](../../data/registers/ram_ddl_v4.yaml) | F19x/L16x/L18x/L19x: RAM | 4 / 8 | 本档位对照基准；完整定义见来源 YAML |
<!-- peripheral-versions:end -->

## SVD 实例与基址

表中使用 SVD 的实例名与基址。SDK 目录与文件来源见 [SVD 总览](../svd-overview.md)。

| 系列 | SVD 实例与基址 |
|---|---|
| A052 | `FLASH@0x40020000`, `RAM@0x40020400` |
| A136 | `FLASH@0x40020000`, `RAM@0x40020400` |
| F002 | `FLASH@0x40020000` |
| F003 | `FLASH@0x40020000`, `RAM@0x40020400` |
| F005 | `FLASH@0x40020000`, `RAM@0x40020400` |
| F030 | `FLASH@0x40020000`, `RAM@0x40020400` |
| F052 | `FLASH@0x40020000`, `RAM@0x40020400` |
| F072 | `FLASH@0x40020000`, `RAM@0x40020400` |
| F17x | `FLASH@0x40020000`, `RAM@0x40020400` |
| F19x | `FLASH@0x40020000`, `RAM@0x40020400` |
| F420 | `FLASH@0x40021400` |
| L021 | `FLASH@0x40020000` |
| L07x | `FLASH@0x40020000`, `RAM@0x40020400` |
| L110 | `FLASH@0x40020000`, `RAM@0x40020400` |
| L12x | `FLASH@0x40020000`, `RAM@0x40020400` |
| L13x | `FLASH@0x40020000`, `RAM@0x40020400` |
| L16x | `FLASH@0x40020000`, `RAM@0x40020400` |
| L17x | `FLASH@0x40020000`, `RAM@0x40020400` |
| L18x | `FLASH@0x40020000`, `RAM@0x40020400` |
| L19x | `FLASH@0x40020000`, `RAM@0x40020400` |
