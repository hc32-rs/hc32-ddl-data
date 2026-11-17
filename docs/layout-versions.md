# 外设版本与布局命名（DDL）

本页及专题采用已确认并启用的名称。寄存器 YAML、系列引用、JSON、PAC 模块和元数据使用相同文件主干。
`data/csv/register_versions.csv` 由脚本生成，`version` 包含 ddl/ll 命名空间；主块名也采用完整外设名。

## 统一规则

- DDL、LL 各自按外设从 v1 开始。主档位依控制、通道、序列和读出能力由简单到复杂排列。
- 同档位的扩展、裁剪、位域或命名分支使用 `_1`、`_2` 数字后缀；不使用字母或描述性版本后缀。
- 目标文件为 `<kind>_ddl_vN[_M].rs`。主档位与后缀都不是发布顺序或兼容性承诺。
- 复杂度存在多个维度。同档不同分支在专题说明，后缀只保证布局唯一，不表示严格的功能大小关系。
- 一个现有生成布局保留一个独立目标；本阶段不做寄存器归一化或跨库合并。
  同一旧 kind 下混入的 I2C/I2S、TMR0/2/4/6 按完整实例名恢复外设名。
  模式视图、共享窗口与 derivedFrom 实例保留明确对应，不能用行数推断物理外设数量。
- 旧文档的概念分组不能直接替代生成 IR。仅比较寄存器数量或忽略位域差异的合并建议已删除。
  `common` 也获得明确的基础版本，不再作为纳入范围的版本名。

## 数据与维护

[显式映射](peripheral-version-map.json)是生成链唯一的编号来源。
[实例对照 CSV](peripheral-versions.csv)及本页/专题的标记区由
`hc32-data-tools/scripts/export_peripheral_versions.py` 生成。脚本不会自行分配或重排编号。
从工作区运行：

```sh
python3 hc32-data-tools/scripts/export_peripheral_versions.py
python3 hc32-data-tools/scripts/export_peripheral_versions.py --check
```

CSV 列固定为 `series,instance,kind,current_version,proposed_version,target_file,status,notes`。
`kind/current_version` 是源数据的真实键，**不是双仓库合并后 PAC 的临时重映射版本**。
完整目标外设名由 `target_file` 和 notes 中的 `target_kind` 给出。
`active` 表示已启用，`excluded` 表示本次排除；排除项的新版本与目标文件名为空。
CSV 为 UTF-8、LF，每行对应系列及 SVD 实例，按当前 kind、功能版本、目标、系列、实例稳定排序。

映射记录来源 YAML 的 SHA-256 和确切系列/实例集合；来源变化、缺项、重复项、目标冲突或排除范围变化都会报错。
专题列出的寄存器/字段数是当前 IR 的条目数，仅用于核对，不是复杂度评分，也不包括原始 SVD 中未保留的属性。
变体差异提示与同主档位的基础布局比较寄存器名、偏移、访问属性及内联位域，最多列出五个名称。
枚举内容、节点名称和原始复位语义需打开来源核对；提示为空差异不等于可合并。
复位值、设备头支持位、封装与硬件行为仍需读原始来源；命名表不会扩充或修复当前生成数据。

修改分配时先核对 SVD、生成 IR 与专题列出的 SDK 证据，编辑显式映射，再重导出。
确认后不应因加入新系列自动改变既有编号。SDK 和 SVD 保持只读；版本审计 CSV 仅由脚本重生成。旧标识仅保存在显式映射的 `legacy_source` 中用于迁移追溯。

<!-- peripheral-versions:begin -->
## 覆盖统计

共 836 个系列/实例映射：836 个已分配、0 个排除。
纳入 47 个目标外设名、237 个独立生成布局。

| 外设 | 主档位 | 布局数 | 功能依据 |
|---|---|---:|---|
| [adc](explore/analog.md) | v1–v7 | 12 | 基础转换、单结果；四通道顺序转换；内嵌 BGR、SQR、单 RESULT；早期多结果读出；四 rank 顺序/插队转换、内嵌 BGR；六 rank、顺序/插队转换；十六个顺序结果、插队结果与累计结果；ALLSTART/CHMAP 等按变体保留 |
| [adt](explore/timer-adv.md) | v1–v2 | 4 | 高级定时器；影子比较及专用中断/DMA 为扩展能力，刹车位分变体 |
| [aes](explore/crypto-comm.md) | v1–v2 | 3 | AES 基础密钥控制；CRDM 镜像按变体保留；AES 密钥长度控制扩展 |
| [atim](explore/timer-adv.md) | v1–v4 | 17 | MODE0 计数视图；MODE1 门控/捕获，CSA/CSB 按变体保留；MODE23 捕获/PWM，1 组 A 通道寄存器；ARRDM、刹车/第四通道按变体保留；MODE23 捕获/PWM，3 组 A 通道寄存器；ARRDM、刹车/第四通道按变体保留；MODE23 捕获/PWM，4 组 A 通道寄存器；ARRDM、刹车/第四通道按变体保留 |
| [bgr](explore/analog.md) | v1 | 2 | 带隙基准控制 |
| [bt](explore/pca-bt-pcnt-aos.md) | v1 | 1 | 紧凑基本定时器 |
| [btim](explore/timer-gen.md) | v1 | 4 | 基本定时器；共享 AIFR/AICR bank 位图分变体 |
| [can](explore/crypto-comm.md) | v1 | 2 | CAN 控制，字段方言分变体 |
| [clk_trim](explore/clock-reset.md) | v1 | 4 | 时钟参考计数与校准 |
| [crc](explore/crypto-comm.md) | v1–v2 | 5 | 无控制窗口的基础 CRC；可配置 CRC；SEL/BUSY 与方言分变体 |
| [ctrim](explore/clock-reset.md) | v1 | 5 | 计数/捕获时钟校准；FLIM 位宽、校准源与控制位分变体 |
| [cts](explore/crypto-comm.md) | v1 | 1 | 电容触摸控制（DDL） |
| [dac](explore/analog.md) | v1–v2 | 2 | 单通道转换；双通道、双数据写入 |
| [debug_active](explore/gpio-int.md) | v1 | 5 | 单寄存器调试活动控制，停机目标位分变体 |
| [dmac](explore/dma.md) | v1–v2 | 6 | 旧式 CONF/CONFA DMA；2 个源地址窗口，触发位与字段方言分变体；旧式 CONF/CONFA DMA；6 个源地址窗口，触发位与字段方言分变体 |
| [eau](explore/pca-bt-pcnt-aos.md) | v1 | 1 | 算术除法控制 |
| [flash](explore/memory.md) | v1–v5 | 12 | 紧凑操作控制；锁区与低功耗分支分变体；紧凑操作控制与 CACHE；显式编程/擦除时序；锁区扩展，再到额外读写时序组 |
| [gpio](explore/gpio-int.md) | v1–v3 | 10 | 输入专用端口，保留选择与中断控制；精简端口与逐引脚复用；通用输入输出与复用；端口阵列、AFR 和逐引脚 SEL 为同档分支 |
| [gpioaux](explore/gpio-int.md) | v1 | 4 | GPIO 辅助复用/触发控制；目标阵容不同，保留独立变体 |
| [gtim](explore/timer-gen.md) | v1 | 4 | 通用捕获/比较定时器；IER 与控制字段分变体 |
| [hdiv](explore/analog.md) | v1 | 1 | 硬件除法 |
| [hsi2c](explore/spi-i2c-i2s.md) | v1 | 2 | 高速主从 I2C；MDER/SDER DMA 扩展分变体 |
| [i2c](explore/spi-i2c-i2s.md) | v1–v2 | 7 | 基础单地址 I2C；多地址匹配，命名及字段分变体 |
| [i2cslv](explore/spi-i2c-i2s.md) | v1 | 1 | 专用从机 I2C |
| [i2s](explore/spi-i2c-i2s.md) | v1 | 1 | 音频串行控制；位域分变体 |
| [iwdt](explore/power-wdt.md) | v1 | 4 | 同档控制；窗口偏移、锁定、阈值/滤波或字段方言分变体 |
| [lcd](explore/crypto-comm.md) | v1–v2 | 5 | 基础 LCD 驱动；RAM 位宽、COMMUX/DRVRES 分变体；电荷泵驱动 LCD |
| [lptim](explore/timer-gen.md) | v1–v2 | 5 | 低功耗计数；CMP 比较为扩展，偏移及控制字段分变体 |
| [lpuart](explore/serial.md) | v1–v4 | 7 | 低功耗串口：基础 → 波特率计数/流控 → 半双工 → BSEL/MODU；DMA 与方言分变体 |
| [lvd](explore/power-wdt.md) | v1 | 7 | 同档控制；窗口偏移、锁定、阈值/滤波或字段方言分变体 |
| [opa](explore/analog.md) | v1–v4 | 5 | 单运放基础控制；单运放及 ADC 触发；双运放及 AZ 自动调零；三运放独立控制，与双运放 AZ 为同档不同分支；五运放控制 |
| [pca](explore/pca-bt-pcnt-aos.md) | v1 | 3 | 基础计数/捕获；CARR/EPWM 扩展 |
| [pcnt](explore/pca-bt-pcnt-aos.md) | v1 | 2 | 基础脉冲计数；DBG 扩展 |
| [ram](explore/memory.md) | v1–v4 | 5 | RAM 错误地址覆盖：11 → 12 → 13 → 14 位；控制字段按变体保留 |
| [reset](explore/clock-reset.md) | v1 | 8 | 外设复位控制，分组规模及字段按变体保留 |
| [rtc](explore/crypto-comm.md) | v1–v2 | 5 | 日历与闹钟，ALMSEC 扩展分变体；日历/闹钟与时间戳/唤醒定时器 |
| [spi](explore/spi-i2c-i2s.md) | v1–v2 | 6 | CR/SSN 基础及 CR2 扩展；CR0/CR1、HDOE/SSI 控制，DMA/SMP 分变体 |
| [sysctrl](explore/clock-reset.md) | v1–v4 | 13 | 内部 RC 时钟与基础门控，无外部晶振/PLL；外部时钟或低速晶振控制，无 PLL；PLL 时钟；RC48M、门控及复位窗口按变体保留；PLL 与 APB0/1、AHB0/1 四组门控/复位 |
| [tim](explore/timer-gen.md) | v1–v5 | 26 | 紧凑基础计数器；MODE0 计数视图；MODE1 门控/捕获，CSA/CSB 按变体保留；MODE23 捕获/PWM，1 组 A 通道寄存器；ARRDM、刹车/第四通道按变体保留；MODE23 捕获/PWM，3 组 A 通道寄存器；ARRDM、刹车/第四通道按变体保留；MODE23 捕获/PWM，4 组 A 通道寄存器；ARRDM、刹车/第四通道按变体保留 |
| [trng](explore/crypto-comm.md) | v1 | 2 | 随机数控制；ENDIE 等字段按变体保留 |
| [uart](explore/serial.md) | v1–v3 | 3 | 基础串口；SCNT 波特率计数及 HDSEL 半双工为递增能力 |
| [usart](explore/serial.md) | v1 | 1 | 同步/异步串口，LIN、SmartCard、IrDA 与自动波特率 |
| [usbfs](explore/crypto-comm.md) | v1 | 1 | USB 全速控制（仅 DDL 纳入） |
| [vc](explore/analog.md) | v1–v3 | 8 | 独立比较器实例，CR0/CR1/CR2 与状态窗口；双比较器；输出路由与校准为同档不同分支；三比较器与输出路由 |
| [vctrim](explore/clock-reset.md) | v1 | 1 | 比较器校准 |
| [wdt](explore/power-wdt.md) | v1 | 3 | 同档控制；窗口偏移、锁定、阈值/滤波或字段方言分变体 |
| [wwdt](explore/power-wdt.md) | v1 | 1 | 同档控制；窗口偏移、锁定、阈值/滤波或字段方言分变体 |
<!-- peripheral-versions:end -->
