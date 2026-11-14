# Flash、RAM 与 SRAM

## Cortex-M0/M0+

FLASH 分为时序型和紧凑 OP 型。F420 的 CACHE 变体归紧凑 OP 型。存储容量与地址另取链接脚本。

| 建议版本 | 系列或实例 | 布局备注 |
|---|---|---|
| `flash_v1` | F003, F005, L110 | 时序型，16bit SLOCK |
| `flash_v2` | A136, F030, L13x | 时序型，32bit SLOCK，DPSTB_EN |
| `flash_v3` | F072, L07x | v2 + SLOCK0/1 |
| `flash_v4` | F17x, L17x | TPGS 9bit，SLOCK0/1 |
| `flash_v5` | F19x, L19x | v4 + SLOCK2/3 |
| `flash_v6` | L16x, L18x | 最宽布局，+CONTP/RO，额外时序组 |
| `flash_v7` | A052, F052 | 紧凑 OP 型 @0x00，FWUP/ROMD |
| `flash_v8` | L12x | 紧凑 OP 型 @0x20，SLOCK0/1 + LOCK_ST |
| `flash_v9` | L021 | 同 v8 无 SLOCK1 |
| `flash_v10` | F002 | 独苗，LPMODE |
| `ram_v1` | A052, F052, F072, F17x, L07x, L12x, L17x | ERRADDR[13:0] |
| `ram_v2` | F19x, L16x, L18x, L19x | ERRADDR[14:0] |
| `ram_v3` | A136, F030, L13x | ERRADDR[12:0] |
| `ram_v4` | F003, F005, L110 | CHKEN，ERRADDR[11:0] |

## HC32F420（Cortex-M4）

FLASH 为 `flash_v7_1`。控制器基址 `0x40021400`，
与 `flash_v7`（A052/F052）相比在 `+0x0C` 增加 CACHE，使 IER、IFR、ICR、BYPASS 的偏移后移 4 字节。
EWARM 示例映射为 128 KiB Flash（`0x00000000` 起）和 24 KiB RAM（`0x20000000` 起）。例程脚本位于
`example/flash/flash_erase_write/EWARM/hc32f420.icf`。
FLASH 的中断为 11，时钟门控在 AHB0EN bit 5。

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
