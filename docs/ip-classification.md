# HC32 DDL 外设 IP 分类

[统一编号规则](layout-versions.md)、[实例 CSV](peripheral-versions.csv)和[显式映射](peripheral-version-map.json)共同描述当前命名。
各专题保留系列差异与 SDK 依据，并从显式映射生成完整布局对照；本索引不再重复维护版本号。

| 外设组 | 核对重点 |
|---|---|
| [ADC、DAC、运放与比较器](explore/analog.md) | 转换序列、结果读出、模拟通道与共享窗口 |
| [时钟、复位与校准](explore/clock-reset.md) | 时钟源、PLL、门控、复位与校准 |
| [CRC、AES、TRNG、RTC、LCD、CAN 与其他通信](explore/crypto-comm.md) | 校验、密钥、日历和通信控制 |
| [DMA](explore/dma.md) | 通道、链表、触发和控制位 |
| [GPIO、中断与调试](explore/gpio-int.md) | 端口架构、复用、调试与 IRQ |
| [Flash 与 RAM 控制](explore/memory.md) | 编程、保护、ECC 与链接地址 |
| [PCA、BT、PCNT 与 EAU](explore/pca-bt-pcnt-aos.md) | 计数扩展或事件路由目标 |
| [电源、低功耗与看门狗](explore/power-wdt.md) | 低功耗、电压检测、看门狗窗口 |
| [UART、LPUART、USART 与 LIN](explore/serial.md) | 协议、流控、超时与实例功能分支 |
| [SPI、I2C 与 I2S](explore/spi-i2c-i2s.md) | 接口架构、地址匹配、DMA 与位域 |
| [高级定时器](explore/timer-adv.md) | 模式、比较/刹车与附属窗口 |
| [通用、基本与低功耗定时器](explore/timer-gen.md) | 计数/比较通道、模式和共享视图 |

[SDK 核验](sdk-verification.md)记录来源与验证边界；[SVD 总览](svd-overview.md)提供 SDK 路径。
