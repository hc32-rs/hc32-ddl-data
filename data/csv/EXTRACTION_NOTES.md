# DDL CSV 提取说明（tools/extract_ddl_csv.py）

生成日期：2026-11-14。脚本：`tools/extract_ddl_csv.py`（自包含，可重复运行，
输出确定性一致，已用 diff 验证两次运行逐字节相同）。

```
python3 tools/extract_ddl_csv.py [--sdk-root xh_ddl_sdk] [--svd-dir hc32-ddl-data/svd] \
    [--out hc32-ddl-data/data/csv] [--series HC32F420 ...]
```

## 数据源与优先级

| CSV | 来源 |
|---|---|
| chips.csv | 各 SDK `documents/DS_*.pdf` 订购信息表（pypdf，layout/plain/row 三种解析器自动择优） |
| interrupts.csv | `mcu/common/<device>.h` 的 `IRQn_Type` 枚举 |
| dma.csv | `driver/inc/dmac.h` 触发源枚举（`DmaXxxTrig` / `DMA_TRISRC_*`） |
| peripherals.csv | `hc32-ddl-data/svd/*.svd`（地址权威）+ `sysctrl.h/clk.h` 门控枚举 |
| memory.csv | `example/template/**/*.icf`（ROM/RAM region，非 `*_ram.icf`） |

## 每系列行数统计

| 系列 | SDK 目录 | chips | interrupts | dma | peripherals(带门控) | 备注 |
|---|---|---:|---:|---:|---:|---|
| HC32A052 | HC32A052_DLL_Rev1.0.1 | 2 | 35 | 26 | 43 (40) | DS 订购表仅 2 个型号 |
| HC32A136 | HC32A136_DDL_Rev2.1.0 | 1 | 37 | 29 | 38 (34) | 单型号系列 |
| HC32F002 | HC32F002_DDL_Rev1.2.1 | 8 | 21 | 0 | 21 (18) | 无 DMA 外设 |
| HC32F003 | HC32F003_DDL_Rev2.0.0 | 4 | 27 | 0 | 24 (20) | DS 覆盖 F005+F003，按前缀拆分 |
| HC32F005 | HC32F005_DDL_Rev2.0.0 | 4 | 27 | 0 | 24 (20) | 同上 |
| HC32F030 | HC32F030_DDL_Rev2.2.0 | 6 | 31 | 29 | 32 (28) | |
| HC32F052 | HC32F052_DDL_Rev1.1.0 | 8 | 35 | 26 | 43 (40) | |
| HC32F072 | HC32F072_DDL_Rev1.2.0 | 4 | 33 | 36 | 42 (38) | |
| HC32F17x | HC32F17x_DDL_Rev1.4.0 | 6 | 32 | 35 | 35 (31) | |
| HC32F19x | HC32F19x_DDL_Rev1.4.0 | 6 | 32 | 35 | 35 (31) | |
| HC32F420 | HC32F420_DDL_Rev1.3.1 | 4 | 62 | 21 | 39 (37) | 唯一 M4，SYSCTRL APB0/1-AHB0/1 EN 门控 |
| HC32L021 | HC32L021_DDL_Rev1.2.1 | 3 | 23 | 0 | 25 (23) | 无 DMA；LVD 无门控位 |
| HC32L07x | HC32L07x_DDL_Rev1.2.0 | 7 | 37 | 37 | 47 (41) | |
| HC32L110 | HC32L110_DDL_Rev1.3.2 | 7 | 30 | 0 | 26 (23) | clk.h/clk.c（非 sysctrl）；无 DMA |
| HC32L12x | HC32L12x_DDL_Rev1.1.0 | 5 | 35 | 27 | 36 (30) | |
| HC32L13x | HC32L13x_DDL_Rev2.2.0 | 6 | 37 | 29 | 38 (34) | |
| HC32L16x | HC32L16x_DDL_Rev1.1.1 | 16 | 37 | 34 | 43 (37) | SVD 文件名 HC32L16x.svd（源自 HC32L166） |
| HC32L17x | HC32L17x_DDL_Rev1.4.0 | 6 | 37 | 35 | 41 (35) | |
| HC32L18x | hc32l186_ddl | 8 | 37 | 34 | 43 (37) | SVD 文件名 HC32L18x.svd（源自 HC32L186） |
| HC32L19x | HC32L19x_DDL_Rev1.4.0 | 6 | 37 | 35 | 41 (35) | |

合计：chips 117、interrupts 682、dma 468、peripherals 716、memory 20。

## DMA 编码语义结论（对全部 20 个系列成立）

- 老 DDL 的 DMAC **每个通道有独立的 `DMAC_CONFAx.TRI_SEL` 触发源选择字段**
  （RM 第 13 章，如 HC32F030 RM Rev2.50 §13.5.2：`TRI_SEL` 位 28:23）。
- `TRI_SEL` 的取值是**全局触发源索引**：`0x00`=软件触发，`>=0x20` 为硬件请求号
  （HC32F030 系；HC32F420/A052/F052 从 `0x40` 起；各系列以 dmac.h 枚举/宏为准）。
- **外设请求与通道之间不是固定连线**：任意请求可由软件路由到任意通道。
  因此 dma.csv 的 `channel` 列填的是 **RM/驱动头的 TRI_SEL 触发索引**（即
  `DmaXxxTrig` 枚举值 / `DMA_TRISRC_*` 的索引），而非物理通道号；物理通道由
  软件在运行时选择（F420 6 通道、F030 2 通道等，见各 RM DMAC 章节）。
  `dma` 列统一填 `DMA1`（各系列至多一个 DMAC）。软件触发（索引 0）不产出行。

## 时钟门控（peripherals.csv 的 fcg_reg/fcg_field）

这些老 DDL SDK 没有 PWC_FCG；门控由 SYSCTRL/CLOCK 外设给出：

- 多数系列：`M0P_SYSCTRL->PERI_CLKEN(0/1)`，门控枚举
  `en_sysctrl_peripheral_gate`，值 bit5=0x20 选择 PERI_CLKEN1（如 F072/L07x/F17x）。
- A052/F052：`HC_SYSCTRL->PERICLKEN0/1`（_PERICLK_REG1_MSK=0x20）。
- F002：`SYSCTRL->PERICLKEN0/1`（_PERICLK_REG1_MSK=0x80000000，门控宏
  `SYSCTRL_PERICLK_*` 而非枚举）；L021：`SYSCTRL->PERI_CLKEN0/1`
  （枚举 `en_sysctrl_periph_clock_t`，+0x20 进 CLKEN1）。
- F420：`HC_SYSCTRL->APB0EN/APB1EN/AHB0EN/AHB1EN`（掩码 0x20/0x40/0x80），
  `fcg_reg` 即这些 EN 寄存器名；bus 列亦由该寄存器推出（APB*/AHB*），
  其余系列 bus 留空（SVD/RM 未逐条核对总线）。
- L110：`M0P_CLOCK->PERI_CLKEN`（clk.h 的 `en_clk_peripheral_gate`）。

## 假设与特殊处理

1. **系列命名**与 DS 标题一致；`hc32l186_ddl` → `HC32L18x`，
   `HC32A052_DLL_Rev1.0.1` 的 `DLL` 视为拼写变体。
2. **共用 DS/RM**：F003/F005 共用一份 DS，按型号前缀（HC32F003*/HC32F005*）
   拆成两个系列分别产出行；F17x 与 F19x 各有自己的 DS（RM 共用不影响本提取）。
3. **chips 行 = part_no+package 去重**：同一 part 的 Tube 与 Tape&Reel
   （型号尾 `TR`）打包变体共享同一 (part_no, package)，只保留一行。
   因此列表格式的 DS（如 F003 的 5 行、L13x 的 10 行）会比 DS 原始行数少。
4. **freq/vdd 回填**：订购表缺主频/电压行时（F003/F005/F030/F072/F17x/F19x/
   L07x/L110/L12x/L13x/L17x/L18x/L19x/A136），从 DS 第 2~3 页产品特性取
   `NNMHz Cortex-Mxx` 与 `Vmin~Vmax V` 回填（HC32F003/L110 为 32MHz，
   其余多为 48MHz，F052 64MHz）。
5. **GPIO "16+1"** 计数（含 SWDIO/SWCLK 复用脚）取前导数字，原值记入
   other 列 `gpio_note=`。
6. **SVD 寄存器视图合并**：`TIMx_MODE0/1/23`、`ATIMxMODE0/1/23` 等同基址
   视图合并为一个实例（`TIMx`/`ATIMx`），peripherals 行数因此小于 SVD 顶层
   peripheral 数；notes 列注明。
7. **复合定时器组门控**：F420 的 BTIM0~8 与 GTIM0~2 同属复合定时器单元
   （例程 `SYSCTRL_PeriphClkEnable(SysctrlPeripheralCTim0)` 使能 BTIM0/1/2），
   BTIM 块挂到基址最近的 GTIM/CTIM 门控并写 notes；F002 的 BTIM3/4/5
   同理挂 `SysctrlPeripheralGTIM`。
8. **门控无法映射到 SVD 实例的枚举成员**（Tick/SWD → SysTick/SWD 调试口、
   F17x/F19x 的 Rtc/Pcnt 不存在于 SVD）不产出行，见运行报告 issues。
9. **interrupts.description**：设备头枚举基本无注释（仅 F420/L021 内核中断有），
   空缺留空；`irq` 为枚举原始值（含 -14/-13/-5/-2/-1 内核中断，schema 允许）。
10. **HC32L110B6YA** 为裸片形式，DS 无封装列，package/pin_count 留空。
11. **F052 的 HC32F052FATA/F8TA-LQ32** 仅出现在 DS 修订记录文字中（新增商业
    编号），订购表无其行，未产出（如需可从官网补抓，source=website）。

## 覆盖率缺口

- chips：各系列行数 = 订购表型号数（按 part+package 去重后），无缺行。
- interrupts：全部来自设备头，无缺（RM 中断表未逐条核对，抽查 F420 max irq=56
  与 DS "NMI+1 to 64 physical interrupts" 一致性未验证保留位）。
- dma：5 个无 DMA 系列正确为 0 行；其余 15 系列的触发源与 dmac.h 一致。
- peripherals：门控覆盖除 meta 块（SYSCTRL/RAM/RESET/DEBUG_ACTIVE/GPIOAUX/
  LVD(无门控位)）外全部实例；L021 LVD、F002 LVD 在门控枚举中无对应位。
- bus 列：仅 F420 由 EN 寄存器名推出 APB/AHB；其余系列留空（RM 逐外设核对
  未做，属后续工作）。
