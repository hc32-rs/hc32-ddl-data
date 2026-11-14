# 布局编号与功能档位

对已比较功能差异的外设，`v1` 表示基础档，后续主编号表示更多功能或更完整的控制和结果读出能力。
同一布局的扩展、裁剪和命名变体沿用主编号，使用 `_1`、`_2` 或已有的描述性后缀。
同档但无明确衍生关系的布局可用 `a`、`b` 区分。编号
本身不保证寄存器兼容，也不表示发布先后。跨系列比较使用展开 `derivedFrom` 后的寄存器和位域，
并结合 SDK 的定义。未排档的分组继续使用旧布局 ID。

| 外设 | 当前档位 | 功能依据 |
|---|---|---|
| [ADC](explore/analog.md) | `adc_v1`–`adc_v7`，另有 `adc_v7_1` | F002 为最简档，L021 为四通道档，F420 有内嵌 BGR 和 SQR、单结果寄存器。F003/F005/L110 独立为多结果基本档。A052/F052 和 L12x 分别为 4、6 rank 档。A136/F030/L13x 为长序列档，F072 等的 `adc_v7_1` 加 ALLSTART。F420 与多结果基本档侧重点不同，排序以结果读出和序列能力综合区分。 |
| [PCA](explore/pca-bt-pcnt-aos.md) | `pca_v1` → `pca_v1_1` | 基础 25 寄存器档 → 增加 CARR 和 EPWM。 |
| [PCNT](explore/pca-bt-pcnt-aos.md) | `pcnt_v1` → `pcnt_v1_1` | 基础计数 → 增加 DBG。 |
| [SPI](explore/spi-i2c-i2s.md) | `spi_v1` → `spi_v1_1` → `spi_v2` | CR/SSN 基础档 → 增加 CR2、ICLR 的扩展档 → CR0/CR1 起始的独立布局档，支持项按系列记录。 |
| [AES](explore/crypto-comm.md) | `aes_v1` → `aes_v2` → `aes_v2_crdm` | AES-128 → 增加 AES-256 → 增加 CRDM 镜像。 |

## 旧 ID 对照

| 外设 | 上次功能排序的编号 → 当前 |
|---|---|
| ADC | `v8→v7_1` |
| PCA | `v2→v1_1` |
| PCNT | `v2→v1_1` |
| SPI | `v2→v1_1`、`v3→v2` |
| AES | `v3→v2_crdm` |

F420 的 `flash_v7_1` 仍是紧凑 OP 布局标识。它相对 `flash_v7` 增加 CACHE，
同时改变 IER 至 BYPASS 的偏移和字段宽度。两个布局与时序型 FLASH 的差异见[存储组](explore/memory.md)。
