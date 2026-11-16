# SVD 修改日志（hc32-ddl-data）

`svd/` 下的 SVD 允许修改，但每次修改前必须在此追加一行记录
（规则见工作区根目录 AGENTS.md）：

| 日期 | 文件 | 修改内容 | 原因 |
|------|------|----------|------|
| 2026-11-14 | 全部 20 个 SVD | 行尾统一为 CRLF（沿用 SDK 原件 mcu/EWARM/*.svd 的行尾），寄存器内容不变 | 入库归一化（首个归档提交信息所写"归一为 LF"仅适用于 LL 仓库，此处更正） |
| 2026-11-16 | 全部 20 个 SVD | 按各系列 data/family 映射为外设注入 `<interrupt>` 节点（共 494 个，value 取 data/interrupts/*.yaml 的设备头 IRQn 编号；family 中在 SVD 拆分为 `*_MODE*` 无同名节点的 TIM0~3/ATIM0~3 条目不注入，明细见 data/csv/interrupt_injection_report.txt） | 为 metapac 生成提供完备中断数据源（hc32-data-tools/patch_svd_interrupts.py） |
| 2026-11-16 | 全部 20 个 SVD | 行尾改由 .gitattributes 统一管控：仓库内归一 LF、检出统一 CRLF（原 `* -text` 策略废止；本仓库 SVD 原以 CRLF 入库，仓库内转为 LF，磁盘检出仍为 CRLF） | 全仓库行尾统一（见 .gitattributes） |
| 2026-11-16 | 全部 20 个 SVD | 检出行尾由 CRLF 改回 LF；仓库内始终 LF，文件内容不变 | 行尾策略最终统一为全 LF（见 .gitattributes） |
