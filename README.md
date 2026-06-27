# STM32F407VET6 智慧集合项目

## 项目概述

基于 STM32F407VET6 微控制器的智慧集合系统，集成多种传感器和执行器，适用于智能家居、安防监控等应用场景。

## 硬件配置

### MCU 参数

| 参数 | 值 |
|------|-----|
| 型号 | STM32F407VET6 |
| 封装 | LQFP100 |
| 主频 | 168 MHz |
| Flash | 512 KB |
| RAM | 192 KB |

### 时钟配置

| 时钟源 | 频率 |
|--------|------|
| HSE (外部高速) | 8 MHz |
| LSE (外部低速) | 32.768 kHz |
| SYSCLK | 168 MHz |
| AHB (HCLK) | 168 MHz |
| APB1 | 42 MHz |
| APB2 | 84 MHz |

## 外设配置

### ADC (模数转换)

- **ADC1**: 2通道连续采样 + DMA传输
  - 通道1 (PA1): ADC1_IN1
  - 通道2 (PC2): ADC1_IN12
  - 采样时间: 480 cycles
  - DMA: DMA2_Stream0, 循环模式

### I2C 总线

| 总线 | SCL | SDA | 速率 |
|------|-----|-----|------|
| I2C1 | PB6 | PB7 | 100 kHz |
| I2C2 | PB10 | PB11 | 100 kHz |

### 定时器 (PWM)

- **TIM3**: PWM输出
  - 通道: CH1 (PA6)
  - 预分频器: 839
  - 周期: 1999
  - PWM频率: 约 50 Hz

### 串口

- **USART2**: 异步通信
  - TX: PA2
  - RX: PA3

## GPIO 配置

### 输入引脚

| 引脚 | 标签 | 功能 | 上拉 |
|------|------|------|------|
| PA0 | PIR | 人体红外传感器 | ✓ |
| PA8 | SMOKE | 烟雾传感器 | ✓ |
| PC4 | DOOR_MAG | 门磁传感器 | ✓ |
| PE0 | KEY1 | 按键1 | ✓ |
| PE1 | KEY2 | 按键2 | ✓ |
| PE2 | KEY3 | 按键3 | ✓ |
| PE3 | KEY4 | 按键4 | ✓ |

### 输出引脚

| 引脚 | 标签 | 功能 | 初始状态 |
|------|------|------|----------|
| PB0 | RELAY1 | 继电器1 | 高电平 |
| PB1 | RELAY2 | 继电器2 | 高电平 |
| PB2 | RELAY3 | 继电器3 | 低电平 |
| PB3 | RELAY4 | 继电器4 | 高电平 |
| PB4 | BUZZER | 蜂鸣器 | 高电平 |
| PC0 | LED_RED | 红色LED | 低电平 |
| PC1 | LED_WHITE | 白色LED | 低电平 |

### 调试接口

- **SWD**: PA13 (SWDIO), PA14 (SWCLK)

## 项目结构

```
stm32f407vet6/
├── Core/
│   ├── Inc/          # 头文件
│   │   ├── main.h
│   │   ├── adc.h
│   │   ├── dma.h
│   │   ├── gpio.h
│   │   ├── i2c.h
│   │   ├── tim.h
│   │   └── usart.h
│   └── Src/          # 源文件
│       ├── main.c    # 主程序
│       ├── adc.c
│       ├── dma.c
│       ├── gpio.c
│       ├── i2c.c
│       ├── tim.c
│       └── usart.c
├── Drivers/          # HAL驱动库
│   ├── CMSIS/
│   └── STM32F4xx_HAL_Driver/
├── cmake/            # CMake配置
├── CMakeLists.txt    # CMake主文件
├── stm32f407vet6.ioc # STM32CubeMX配置
└── README.md
```

## 编译环境

### 工具链

- **编译器**: GCC ARM None-EABI
- **构建系统**: CMake
- **IDE**: STM32CubeIDE / VS Code + Cortex-Debug

### 编译命令

```bash
# 创建构建目录
mkdir build && cd build

# 配置CMake
cmake -DCMAKE_TOOLCHAIN_FILE=../cmake/gcc-arm-none-eabi.cmake ..

# 编译
make -j4
```

### 烧录

```bash
# 使用 ST-Link
st-flash write build/stm32f407vet6.bin 0x08000000

# 或使用 OpenOCD
openocd -f interface/stlink.cfg -f target/stm32f4x.cfg -c "program build/stm32f407vet6.elf verify reset exit"
```

## 功能扩展

### 待实现功能

- [ ] 传感器数据采集与处理
- [ ] 继电器控制逻辑
- [ ] 蜂鸣器报警功能
- [ ] LED状态指示
- [ ] 按键中断处理
- [ ] USART通信协议
- [ ] I2C设备驱动 (OLED显示屏、温湿度传感器等)

## 注意事项

1. **电源要求**: 3.3V供电，注意电流需求
2. **晶振**: 使用8MHz外部晶振，确保负载电容匹配
3. **上拉电阻**: I2C总线需要外部上拉电阻 (4.7kΩ)
4. **ADC参考电压**: 使用VDDA作为参考电压 (3.3V)

## 许可证

Copyright (c) 2026 STMicroelectronics. All rights reserved.
