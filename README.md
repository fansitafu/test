# STM32-FreeRTOS 多任务LED按键控制实验
基于STM32 HAL库 + FreeRTOS（CMSIS-RTOS）实现的多任务嵌入式实验，通过按键切换两种LED闪烁模式，使用FreeRTOS任务调度、邮箱队列、软件定时器核心功能。

## 一、项目简介
本项目是STM32 FreeRTOS入门实战，实现**多任务独立运行**、**按键中断式扫描**、**队列数据传输**、**软件定时器定时控制**，完成LED闪烁模式的动态切换，适合学习FreeRTOS基础组件。

## 二、硬件连接
- **LED1**：GPIOD_PIN_0（推挽输出）
- **LED2**：GPIOD_PIN_1（推挽输出）
- **按键KEY**：GPIOA_PIN_4（上拉输入，低电平触发）
- 开发板：STM32任意系列（F1/F4/L4通用，基于HAL库）

## 三、核心功能
1. **FreeRTOS多任务调度**
   - 默认空闲任务：`StartDefaultTask`
   - LED1控制任务：`LED1_Task_Entry`
   - LED2控制任务：`LED2_Task_Entry`
   - 按键扫描任务：`KEY_Task_Entry`

2. **双模式LED闪烁**
   - 模式0（默认）：LED2单独500ms闪烁
   - 模式1：LED1+LED2同步200ms闪烁
   - 按**PA4按键**一键切换模式

3. **FreeRTOS核心组件应用**
   - 邮箱队列：按键任务发送数据，LED2任务接收
   - 软件定时器：定时触发LED电平翻转
   - 任务延时：`osDelay` 实现任务调度

## 四、代码模块说明
| 函数名 | 功能 |
|--------|------|
| `StartDefaultTask` | FreeRTOS默认空闲任务，无业务逻辑 |
| `LED1_Task_Entry` | LED1初始化置高，等待调度 |
| `LED2_Task_Entry` | LED2初始化置高，接收邮箱队列数据 |
| `KEY_Task_Entry` | 按键扫描、消抖、发送队列数据、切换工作模式 |
| `Callback01` | 软件定时器回调函数，根据模式翻转LED电平 |

## 五、快速使用
1. 将代码编译后烧录到STM32开发板
2. 上电后默认进入**模式0**：LED2 500ms闪烁
3. 按下**PA4按键**，切换至**模式1**：LED1+LED2 200ms同步闪烁
4. 再次按键，切回模式0，循环切换

## 六、技术栈
- 主控：STM32（HAL库）
- 操作系统：FreeRTOS（CMSIS-RTOS封装）
- 外设：GPIO输入/输出、软件定时器
- IPC机制：FreeRTOS邮箱队列

## 七、项目特点
- 任务解耦：按键、LED、定时器分独立任务，逻辑清晰
- 动态控制：实时响应按键，无缝切换LED工作模式
- 入门友好：覆盖FreeRTOS最常用核心功能，适合新手学习
