# CX32L003 HAL/SPL Firmware Library

CX32L003 系列微控制器的硬件抽象层(HAL)库与标准外设库(SPL)。

## 简介

本项目提供 CX32L003 系列 MCU 的完整固件支持，包括：
- **HAL 库**：硬件抽象层，提供高级 API 接口
- **SPL 标准库**：标准外设库，提供底层寄存器访问
- **示例代码**：各外设的使用示例
- **数据手册**：芯片官方文档

## 支持的外设

### 核心外设
- **RCC** - 时钟控制系统 (HIRC/HXT/LIRC/LXT)
- **GPIO** - 通用输入输出 (支持上下拉、开漏、去抖、复用功能)
- **ADC** - 12位模数转换器 (支持单次/连续转换、外部触发)
- **UART** - 串口通信 (支持波特率配置、校验位、收发模式)
- **I2C** - I2C 总线接口
- **SPI** - SPI 总线接口
- **TIM** - 定时器 (支持 PWM、输入捕获、输出比较)
- **PCA** - 可编程计数器阵列

### 系统外设
- **RTC** - 实时时钟
- **WWDG** - 窗口看门狗
- **IWDG** - 独立看门狗
- **DMA** - 直接内存访问
- **NVIC** - 嵌套向量中断控制器

## 项目结构

```
CX32L003-HAL-SPL/
├── CX32L003_HAL_Driver/          # HAL 库
│   ├── Driver/Inc/              # HAL 头文件
│   └── Driver/Src/              # HAL 源文件
├── CX32L003_Firmware Library/   # 标准库
│   ├── Driver/Inc/              # SPL 头文件
│   └── Driver/Src/              # SPL 源文件
├── CX32L003数据手册/            # 官方文档
│   ├── CX32L003_Datasheet_1.0.7.pdf
│   └── CX32L003_User_Manual_V1.0.2_20191129.pdf
└── keil_CX32L003_pack/          # Keil 设备支持包
    └── XMC.CX32L003_DFP.1.0.7.pack
```

## 快速开始

### 1. 安装 Keil 设备支持包

1. 双击 `keil_CX32L003_pack/XMC.CX32L003_DFP.1.0.7.pack`
2. 在 Keil MDK 中通过 Pack Installer 安装
3. 安装完成后在 Device Selector 中选择 CX32L003

### 2. 创建新项目

1. 在 Keil MDK 中创建新项目
2. 选择 CX32L003 芯片型号
3. 添加以下库文件到项目：
   - HAL 库：`CX32L003_HAL_Driver/Driver/Src/*.c`
   - SPL 库：`CX32L003_Firmware Library/Driver/Src/*.c`
   - CMSIS 文件：`CX32L003_Firmware Library/CMSIS/*.c`
   - 启动文件：`CX32L003_Firmware Library/startup/*.s`

### 3. 配置时钟系统

```c
#include "cx32l003_rcc.h"

// 使用内部高速时钟 HIRC (24MHz)
RCC_SysclkSource(RCC_SYSCLKSource_HIRC);
RCC_SetHclkDiv(RCC, RCC_HCLKDiv_1);   // HCLK = 24MHz
RCC_SetPclkDiv(RCC, RCC_PCLKDiv_1);   // PCLK = 24MHz
```

### 4. 配置 GPIO

```c
#include "cx32l003_hal_gpio.h"

GPIO_InitTypeDef GPIO_InitStruct = {0};
GPIO_InitStruct.Pin = GPIO_PIN_5;
GPIO_InitStruct.Mode = GPIO_MODE_OUTPUT;
GPIO_InitStruct.Pull = GPIO_PULLUP;
GPIO_InitStruct.DrvStrength = GPIO_DRVSTRENGTH_STRONGEST;
HAL_GPIO_Init(GPIOA, &GPIO_InitStruct);
```

### 5. 配置 ADC

```c
#include "cx32l003_hal_adc.h"

ADC_InitTypeDef ADC_InitStruct = {0};
ADC_InitStruct.Channel = ADC_CHANNEL_0;
ADC_InitStruct.SamplingTime = ADC_SAMPLETIME_28CYCLES;
ADC_InitStruct.EOCSelection = ADC_EOC_SINGLE_CONV;
HAL_ADC_Init(&ADC_InitStruct);
```

## API 文档

### HAL 库 API

- `HAL_GPIO_Init()` - GPIO 初始化
- `HAL_ADC_Init()` - ADC 初始化
- `HAL_UART_Init()` - UART 初始化
- `HAL_TIM_PWM_Init()` - PWM 初始化
- `HAL_RCC_ClockConfig()` - 系统时钟配置

### SPL 库 API

- `RCC_SysclkSource()` - 选择系统时钟源
- `RCC_SetHclkDiv()` - 设置 HCLK 分频
- `RCC_SetPclkDiv()` - 设置 PCLK 分频
- `GPIOx_WriteBit()` - 端口位操作
- `ADCx_GetData()` - 读取 ADC 数据

详细 API 说明请参考对应头文件。

## 数据手册

- [CX32L003 数据手册 (V1.0.7)](CX32L003数据手册/CX32L003_Datasheet_1.0.7.pdf)
- [CX32L003 用户手册 (V1.0.2)](CX32L003数据手册/CX32L003_User_Manual_V1.0.2_20191129.pdf)

## 技术规格

- **内核**: ARM Cortex-M0+
- **主频**: 最高 48MHz
- **Flash**: 最大 64KB
- **SRAM**: 最大 8KB
- **ADC**: 12位，16通道，最高 1MSPS
- **定时器**: 4个16位定时器
- **通信接口**: UART×3, I2C×2, SPI×2

## 许可证

本项目遵循 CX32L003 芯片厂商的许可证条款。

## 相关链接

- [芯片厂商官网](https://www.cx32l003.com)
- [Keil MDK 下载](https://www.keil.com/download/)
- [CMSIS 文档](https://www.keil.com/pack/doc/CMSIS/General/html/index.html)
