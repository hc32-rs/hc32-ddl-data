# hc32-ddl-data

小华半导体 HC32 DDL 驱动系列的寄存器数据与外设分类资料。覆盖 **20 个系列**，
包括 19 个 Cortex-M0/M0+ 系列和 Cortex-M4 的 HC32F420。
HC32F420 使用旧式 DDL 接口，时钟、GPIO 等仍与本仓库若干 M0/M0+ 系列同族。

## 资料

- [SVD 与 SDK 对照表](docs/svd-overview.md)列出本仓库的系列、SDK 版本与 SVD 元数据。
- [驱动矩阵](docs/driver-matrix.json)按 SDK 的 `driver/inc/*.h` 生成。
  值为系列名与 SDK 修订号的拼接。SDK 目录别名包括 A052 的 `DLL` 和 L18x 的 `hc32l186_ddl`。
- [外设 IP 分类](docs/ip-classification.md)按外设组链接分类、寄存器布局和 SDK 核验依据。
- [布局编号与功能档位](docs/layout-versions.md)记录功能档位和旧编号对照。
- [SDK 核验记录](docs/sdk-verification.md)列出逐组证据及待确认项。

SDK 源码在工作区相邻的 `../xh_ddl_sdk/`。设备头、驱动和例程用于核对 SVD 中的寄存器、
中断、时钟门控与触发源。当前阶段已归档原始 SVD 和分类文档。

## 数据目录规划

后续 YAML 用于生成 Rust PAC 并为 HAL 提供实例级数据。布局沿用原项目的目录规划。

| 目录 | 用途 |
|---|---|
| `data/chips/` | 型号、封装、内存与系列引用 |
| `data/family/` | 系列共享外设 |
| `data/peripherals/` | 外设实例、时钟门控、引脚功能和 DMA 请求 |
| `data/registers/` | 按 `<kind>_<version>.yaml` 命名的寄存器块 |
| `data/interrupts/` | 中断向量 |
| `data/dma/` | DMA 通道与请求映射 |
| `data/memory/` | 链接脚本使用的内存布局 |
