# CRC、AES、TRNG、RTC、LCD、CAN 与其他通信

CRC 先区分无控制窗口的基础布局和可配置布局，CSR/CR、SEL、BUSY 等作为变体。
F420 的 DR 与 A052/F052 的 DATA 同址，但 CSR.BUSY 是 `driver/src/crc.c` 轮询位，不能丢弃。
`SysctrlPeripheralCrc=66` 对应 AHB0EN bit 2。

AES 按 AES-128、AES-128/256 分档；L12x/L16x/L18x 的 CRDM 镜像保留数字后缀。
RTC 先分日历/闹钟与时间戳/唤醒控制；ALMSEC、COMPEN 位宽及 HCOMP/CKSEL/O2SEL 各自保留。
LCD 的 RAM8–F 位宽、COMMUX、DRVRES 与电荷泵控制不能只用寄存器数量判断。
TRNG 的布局与基址按实例登记。CAN 的生成布局差异不因原文曾合用一个分类而合并。

DDL 的 USBFS 与 CTS 本次纳入，与 LL 的排除范围无关。
F072/L07x 的 USBFS 基址为 0x40040000，IRQ 27；CTS 为 0x40005000，与时钟校准共享 IRQ 31。
这两个外设以当前生成布局建立基础档位，硬件行为不由版本号作保证。

[编号规则](../layout-versions.md) · [实例 CSV](../peripheral-versions.csv) · [SDK 核验](../sdk-verification.md)。

<!-- peripheral-versions:begin -->
## 版本与系列对照

以下为待确认命名，当前来源仍使用旧文件名。系列名前缀 `HC32` 省略。

### aes：v1

AES 基础密钥控制；CRDM 镜像按变体保留。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `aes_ddl_v1.rs` | [aes_v1](../../data/registers/aes_v1.yaml) | A136/F030/L13x: AES | 9 / 11 | 本档位对照基准；完整定义见来源 YAML |

### aes：v2

AES 密钥长度控制扩展。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `aes_ddl_v2.rs` | [aes_v2](../../data/registers/aes_v2.yaml) | F072/F17x/F19x/L07x/L17x/L19x: AES | 13 / 16 | 本档位对照基准；完整定义见来源 YAML |
| `aes_ddl_v2_1.rs` | [aes_v3](../../data/registers/aes_v3.yaml) | L12x/L16x/L18x: AES | 14 / 20 | 相对 aes_ddl_v2：增加 CRDM；定义差异 DATA0/DATA1/DATA2/DATA3/KEY0 等 12 项 |

### can：v1

CAN 控制，字段方言分变体。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `can_ddl_v1.rs` | [can_v1](../../data/registers/can_v1.yaml) | A052/F052: CAN | 29 / 86 | 本档位对照基准；完整定义见来源 YAML |
| `can_ddl_v1_1.rs` | [can_v2](../../data/registers/can_v2.yaml) | F072/L07x: CAN | 29 / 86 | 相对 can_ddl_v1：定义差异 ACF/TBSLOT/TCTRL/TRG_CFG |

### crc：v1

无控制窗口的基础 CRC。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `crc_ddl_v1.rs` | [crc_v3](../../data/registers/crc_v3.yaml) | F003/F005/L110: CRC | 2 / 3 | 本档位对照基准；完整定义见来源 YAML |

### crc：v2

可配置 CRC；SEL/BUSY 与方言分变体。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `crc_ddl_v2.rs` | [crc_v1](../../data/registers/crc_v1.yaml) | A052/F052: CRC | 3 / 5 | 本档位对照基准；完整定义见来源 YAML |
| `crc_ddl_v2_1.rs` | [crc_v2](../../data/registers/crc_v2.yaml) | A136/F030/F072/F17x/F19x/L07x/L13x/L16x/L17x/L18x/L19x: CRC | 3 / 5 | 相对 crc_ddl_v2：增加 CR；缺少 CSR |
| `crc_ddl_v2_2.rs` | [crc_v4](../../data/registers/crc_v4.yaml) | F420: CRC | 3 / 6 | 相对 crc_ddl_v2：增加 DR；缺少 DATA；定义差异 CSR |
| `crc_ddl_v2_3.rs` | [crc_v5](../../data/registers/crc_v5.yaml) | L12x: CRC | 3 / 6 | 相对 crc_ddl_v2：增加 CR；缺少 CSR |

### cts：v1

电容触摸控制（DDL）。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `cts_ddl_v1.rs` | [cts_common](../../data/registers/cts_common.yaml) | F072/L07x: CTS | 4 / 31 | 本档位对照基准；完整定义见来源 YAML |

### lcd：v1

基础 LCD 驱动；RAM 位宽、COMMUX/DRVRES 分变体。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `lcd_ddl_v1.rs` | [lcd_v1](../../data/registers/lcd_v1.yaml) | A136/L13x: LCD | 21 / 383 | 本档位对照基准；完整定义见来源 YAML |
| `lcd_ddl_v1_1.rs` | [lcd_v2](../../data/registers/lcd_v2.yaml) | F17x/F19x/L17x/L19x: LCD | 21 / 531 | 相对 lcd_ddl_v1：定义差异 POEN1/RAM8/RAM9/RAMA/RAMB 等 9 项 |
| `lcd_ddl_v1_2.rs` | [lcd_v3](../../data/registers/lcd_v3.yaml) | L07x: LCD | 21 / 532 | 相对 lcd_ddl_v1：定义差异 POEN1/RAM8/RAM9/RAMA/RAMB 等 9 项 |
| `lcd_ddl_v1_3.rs` | [lcd_v5](../../data/registers/lcd_v5.yaml) | L16x/L18x: LCD | 21 / 532 | 相对 lcd_ddl_v1：定义差异 CR1/POEN1/RAM8/RAM9/RAMA 等 10 项 |

### lcd：v2

电荷泵驱动 LCD。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `lcd_ddl_v2.rs` | [lcd_v4](../../data/registers/lcd_v4.yaml) | L12x: LCD | 22 / 396 | 本档位对照基准；完整定义见来源 YAML |

### rtc：v1

日历与闹钟，ALMSEC 扩展分变体。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `rtc_ddl_v1.rs` | [rtc_v5](../../data/registers/rtc_v5.yaml) | L110: RTC | 13 / 33 | 本档位对照基准；完整定义见来源 YAML |
| `rtc_ddl_v1_1.rs` | [rtc_v2](../../data/registers/rtc_v2.yaml) | A136/L13x: RTC | 13 / 46 | 相对 rtc_ddl_v1：定义差异 ALMHOUR/ALMMIN/ALMWEEK/COMPEN/CR0 等 13 项 |
| `rtc_ddl_v1_2.rs` | [rtc_v4](../../data/registers/rtc_v4.yaml) | L07x/L16x/L17x/L18x/L19x: RTC | 14 / 49 | 相对 rtc_ddl_v1：增加 ALMSEC；定义差异 ALMHOUR/ALMMIN/ALMWEEK/COMPEN/CR0 等 13 项 |

### rtc：v2

日历/闹钟与时间戳/唤醒定时器。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `rtc_ddl_v2.rs` | [rtc_v1](../../data/registers/rtc_v1.yaml) | A052/F052: RTC | 20 / 102 | 本档位对照基准；完整定义见来源 YAML |
| `rtc_ddl_v2_1.rs` | [rtc_v3](../../data/registers/rtc_v3.yaml) | L021/L12x: RTC | 21 / 102 | 相对 rtc_ddl_v2：增加 HCOMP/WUTR；缺少 WUTARR；定义差异 COMPEN/CR0/CR2/IER |

### trng：v1

随机数控制；ENDIE 等字段按变体保留。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `trng_ddl_v1.rs` | [trng_v1](../../data/registers/trng_v1.yaml) | A136/F030/F072/F17x/F19x/L021/L07x/L13x/L16x/L17x/L18x/L19x: TRNG | 4 / 9 | 本档位对照基准；完整定义见来源 YAML |
| `trng_ddl_v1_1.rs` | [trng_v2](../../data/registers/trng_v2.yaml) | L12x: TRNG | 4 / 9 | 相对 trng_ddl_v1：定义差异 CR |

### usbfs：v1

USB 全速控制（仅 DDL 纳入）。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `usbfs_ddl_v1.rs` | [usbfs_common](../../data/registers/usbfs_common.yaml) | F072/L07x: USBFS | 67 / 312 | 本档位对照基准；完整定义见来源 YAML |
<!-- peripheral-versions:end -->

## SVD 实例与基址

表中使用 SVD 的实例名与基址。SDK 目录与文件来源见 [SVD 总览](../svd-overview.md)。

| 系列 | SVD 实例与基址 |
|---|---|
| A052 | `CAN@0x40030000`, `CRC@0x40020800`, `RTC@0x40001400` |
| A136 | `AES@0x40021400`, `CRC@0x40020900`, `LCD@0x40005C00`, `RTC@0x40001400`, `TRNG@0x40004C00` |
| F003 | `CRC@0x40020900` |
| F005 | `CRC@0x40020900` |
| F030 | `AES@0x40021400`, `CRC@0x40020900`, `TRNG@0x40004C00` |
| F052 | `CAN@0x40030000`, `CRC@0x40020800`, `RTC@0x40001400` |
| F072 | `AES@0x40021400`, `CAN@0x40030000`, `CRC@0x40020900`, `TRNG@0x40004C00` |
| F17x | `AES@0x40021400`, `CRC@0x40020900`, `LCD@0x40005C00`, `TRNG@0x40004C00` |
| F19x | `AES@0x40021400`, `CRC@0x40020900`, `LCD@0x40005C00`, `TRNG@0x40004C00` |
| F420 | `CRC@0x40020800` |
| L021 | `RTC@0x40001400`, `TRNG@0x40008C00` |
| L07x | `AES@0x40021400`, `CAN@0x40030000`, `CRC@0x40020900`, `LCD@0x40005C00`, `RTC@0x40001400`, `TRNG@0x40004C00` |
| L110 | `CRC@0x40020900`, `RTC@0x40001400` |
| L12x | `AES@0x40021400`, `CRC@0x40020800`, `LCD@0x40009C00`, `RTC@0x40001400`, `TRNG@0x40008C00` |
| L13x | `AES@0x40021400`, `CRC@0x40020900`, `LCD@0x40005C00`, `RTC@0x40001400`, `TRNG@0x40004C00` |
| L16x | `AES@0x40021400`, `CRC@0x40020900`, `LCD@0x40005C00`, `RTC@0x40001400`, `TRNG@0x40004C00` |
| L17x | `AES@0x40021400`, `CRC@0x40020900`, `LCD@0x40005C00`, `RTC@0x40001400`, `TRNG@0x40004C00` |
| L18x | `AES@0x40021400`, `CRC@0x40020900`, `LCD@0x40005C00`, `RTC@0x40001400`, `TRNG@0x40004C00` |
| L19x | `AES@0x40021400`, `CRC@0x40020900`, `LCD@0x40005C00`, `RTC@0x40001400`, `TRNG@0x40004C00` |
