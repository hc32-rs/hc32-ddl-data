# UART、LPUART、USART 与 LIN

UART 按基础、SCNT 波特率计数及 HDSEL 半双工分档。
LPUART 从基础六寄存器、波特率计数/流控、半双工到 BSEL/MODU 扩展。
L021 缺少 DMARXEN/DMATXEN，F002 使用 DR/CR/ADDR/ADDRMASK 命名；这些差异保留为变体。
原文建议把 8051 命名与现代命名直接合并，现改为保留实际生成字段。

A052/F052/L12x/F420 的 USART 当前共用一份生成 IR，含 LIN、SmartCard、IrDA、同步和自动波特率。
L12x 的 BRR/SR/TDR/ICR 复位值与 A052/F052 的 SVD 记录不同，原始值继续保留在审计来源。
F420 USART0–3 各有 RX/TX/FLAG 三条固定 IRQ：12+3x、13+3x、14+3x。
门控为 APB0EN bit 0–3，DMA TX/RX 请求为 0x40+2x/0x41+2x。
SDK 的 `example/usart/usart_lin_master/` 使用 USART 的 LIN 模式。

[编号规则](../layout-versions.md) · [实例 CSV](../peripheral-versions.csv) · [SDK 核验](../sdk-verification.md)。

<!-- peripheral-versions:begin -->
## 版本与系列对照

以下为待确认命名，当前来源仍使用旧文件名。系列名前缀 `HC32` 省略。

### lpuart：v1

低功耗串口：基础 → 波特率计数/流控 → 半双工 → BSEL/MODU；DMA 与方言分变体。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `lpuart_ddl_v1.rs` | [lpuart_v6](../../data/registers/lpuart_v6.yaml) | L110: LPUART | 6 / 22 | 本档位对照基准；完整定义见来源 YAML |

### lpuart：v2

低功耗串口：基础 → 波特率计数/流控 → 半双工 → BSEL/MODU；DMA 与方言分变体。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `lpuart_ddl_v2.rs` | [lpuart_v2](../../data/registers/lpuart_v2.yaml) | A136/L13x: LPUART0, LPUART1 | 7 / 41 | 本档位对照基准；完整定义见来源 YAML |

### lpuart：v3

低功耗串口：基础 → 波特率计数/流控 → 半双工 → BSEL/MODU；DMA 与方言分变体。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `lpuart_ddl_v3.rs` | [lpuart_v3](../../data/registers/lpuart_v3.yaml) | F002: LPUART0, LPUART1 | 7 / 40 | 本档位对照基准；完整定义见来源 YAML |
| `lpuart_ddl_v3_1.rs` | [lpuart_v1](../../data/registers/lpuart_v1.yaml) | A052/F052: LPUART0, LPUART1 | 7 / 42 | 相对 lpuart_ddl_v3：增加 SADDR/SADEN/SBUF/SCNT/SCON；缺少 ADDR/ADDRMASK/BRR/CR/DR；定义差异 ICR |
| `lpuart_ddl_v3_2.rs` | [lpuart_v4](../../data/registers/lpuart_v4.yaml) | F072/L07x/L17x/L19x: LPUART0, LPUART1 | 7 / 42 | 相对 lpuart_ddl_v3：增加 SADDR/SADEN/SBUF/SCNT/SCON；缺少 ADDR/ADDRMASK/BRR/CR/DR；定义差异 ICR |

### lpuart：v4

低功耗串口：基础 → 波特率计数/流控 → 半双工 → BSEL/MODU；DMA 与方言分变体。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `lpuart_ddl_v4.rs` | [lpuart_v5](../../data/registers/lpuart_v5.yaml) | L021: LPUART0, LPUART1 | 9 / 44 | 本档位对照基准；完整定义见来源 YAML |
| `lpuart_ddl_v4_1.rs` | [lpuart_v7](../../data/registers/lpuart_v7.yaml) | L12x: LPUART0, LPUART1, LPUART2；L16x/L18x: LPUART0, LPUART1 | 9 / 46 | 相对 lpuart_ddl_v4：定义差异 SADDR/SADEN/SCON |

### uart：v1

基础串口；SCNT 波特率计数及 HDSEL 半双工为递增能力。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `uart_ddl_v1.rs` | [uart_v2](../../data/registers/uart_v2.yaml) | F003/F005/L110: UART0, UART1 | 6 / 18 | 本档位对照基准；完整定义见来源 YAML |

### uart：v2

基础串口；SCNT 波特率计数及 HDSEL 半双工为递增能力。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `uart_ddl_v2.rs` | [uart_v1](../../data/registers/uart_v1.yaml) | A136/F030/L13x: UART0, UART1 | 7 / 40 | 本档位对照基准；完整定义见来源 YAML |

### uart：v3

基础串口；SCNT 波特率计数及 HDSEL 半双工为递增能力。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `uart_ddl_v3.rs` | [uart_v3](../../data/registers/uart_v3.yaml) | F072/F17x/F19x/L07x/L16x/L17x/L18x/L19x: UART0, UART1, UART2, UART3 | 7 / 41 | 本档位对照基准；完整定义见来源 YAML |

### usart：v1

同步/异步串口，LIN、SmartCard、IrDA 与自动波特率。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `usart_ddl_v1.rs` | [usart_common](../../data/registers/usart_common.yaml) | A052/F052/L12x: USART0, USART1；F420: USART0, USART1, USART2, USART3 | 14 / 96 | 本档位对照基准；完整定义见来源 YAML |
<!-- peripheral-versions:end -->

## SVD 实例与基址

表中使用 SVD 的实例名与基址。SDK 目录与文件来源见 [SVD 总览](../svd-overview.md)。

| 系列 | SVD 实例与基址 |
|---|---|
| A052 | `LPUART0@0x40000000`, `LPUART1@0x40004000`, `USART0@0x40006000`, `USART1@0x40006400` |
| A136 | `LPUART0@0x40000200`, `LPUART1@0x40004000`, `UART0@0x40000000`, `UART1@0x40000100` |
| F002 | `LPUART0@0x40000000`, `LPUART1@0x40004000` |
| F003 | `UART0@0x40000000`, `UART1@0x40000100` |
| F005 | `UART0@0x40000000`, `UART1@0x40000100` |
| F030 | `UART0@0x40000000`, `UART1@0x40000100` |
| F052 | `LPUART0@0x40000000`, `LPUART1@0x40004000`, `USART0@0x40006000`, `USART1@0x40006400` |
| F072 | `LPUART0@0x40000200`, `LPUART1@0x40004000`, `UART0@0x40000000`, `UART1@0x40000100`, `UART2@0x40006000`, `UART3@0x40006400` |
| F17x | `UART0@0x40000000`, `UART1@0x40000100`, `UART2@0x40006000`, `UART3@0x40006400` |
| F19x | `UART0@0x40000000`, `UART1@0x40000100`, `UART2@0x40006000`, `UART3@0x40006400` |
| F420 | `USART0@0x40000000`, `USART1@0x40000400`, `USART2@0x40000800`, `USART3@0x40000C00` |
| L021 | `LPUART0@0x40000000`, `LPUART1@0x40008000` |
| L07x | `LPUART0@0x40000200`, `LPUART1@0x40004000`, `UART0@0x40000000`, `UART1@0x40000100`, `UART2@0x40006000`, `UART3@0x40006400` |
| L110 | `LPUART@0x40000200`, `UART0@0x40000000`, `UART1@0x40000100` |
| L12x | `LPUART0@0x40000000`, `LPUART1@0x40008000`, `LPUART2@0x4000B400`, `USART0@0x4000A000`, `USART1@0x4000A400` |
| L13x | `LPUART0@0x40000200`, `LPUART1@0x40004000`, `UART0@0x40000000`, `UART1@0x40000100` |
| L16x | `LPUART0@0x40000200`, `LPUART1@0x40004000`, `UART0@0x40000000`, `UART1@0x40000100`, `UART2@0x40006000`, `UART3@0x40006400` |
| L17x | `LPUART0@0x40000200`, `LPUART1@0x40004000`, `UART0@0x40000000`, `UART1@0x40000100`, `UART2@0x40006000`, `UART3@0x40006400` |
| L18x | `LPUART0@0x40000200`, `LPUART1@0x40004000`, `UART0@0x40000000`, `UART1@0x40000100`, `UART2@0x40006000`, `UART3@0x40006400` |
| L19x | `LPUART0@0x40000200`, `LPUART1@0x40004000`, `UART0@0x40000000`, `UART1@0x40000100`, `UART2@0x40006000`, `UART3@0x40006400` |
