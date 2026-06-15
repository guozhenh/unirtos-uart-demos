# UART Demo

## 概述

UART Demo 是一个基于 UNIRTOS 的串口通信示例项目。该项目演示了如何在 UNIRTOS 平台上配置 UART 通信参数、注册串口事件回调、初始化 UART 引脚并进行数据收发测试。通过此示例，开发者可以快速了解 UNIRTOS UART API 的基本使用方法。

**适用平台**：所有支持 UNIRTOS 的平台

## 快速上手

### 1. 开发环境搭建

参考 [UNIRTOS 快速入门](https://docs.quectel.com/zh/UniRTOS/UniRTOS%E6%96%87%E6%A1%A3/%E5%BF%AB%E9%80%9F%E4%B8%8A%E6%89%8B/%E5%BF%AB%E9%80%9F%E4%B8%8A%E6%89%8B.html) 文档，了解如何搭建开发环境,了解开发过程。

#### 2. 获取项目
进入unirtos/qos_applications目录下执行以下命令下载项目：
```bash
git clone https://github.com/UniRTOS/unirtos_uart_demos.git
```

#### 4. 项目结构

```
uart_demos/
├── CMakeLists.txt           # CMake 构建配置
├── demo.manifest.json       # 应用清单文件
├── README.md                # 本文件
├── uart_demo.c              # UART 示例源代码
└── uart_demo.h              # UART 示例头文件和配置项
```

#### 5. 构建项目
unirtos 目录下运行以下命令进行构建：
```bash
unirtos make EG800ZCN_LA EG800ZCNLAR01A01M04_BETA_OCPU_20260511
```

#### 6. 日志展示

初始化阶段可在日志中看到以下输出：

```
[I/UART_API] enter UniRTOS UART DEMO !!!
```

成功运行后，示例会初始化 UART1，并默认每隔约 1 秒通过串口输出以下内容：

```
hello UniRTOS
hello UniRTOS
hello UniRTOS
...
```


## 代码概览

#### 示例工作流程

```
程序启动
	↓
调用 unir_uart_demo_init()
	↓
创建名为 "uart_demo" 的任务
	↓
进入任务主函数 unir_uart_demo_process()
	↓
注册 UART 事件回调 unir_uart_ind()
	↓
配置 UART 参数和引脚复用
	↓
打开 UART 端口
	↓
根据测试用例执行 UART 功能测试：
  ├─ 周期性发送数据
  ├─ 接收数据并通过回调处理
  ├─ 主动读取并回写数据
  ├─ 切换不同波特率
  └─ 切换 UART/AT 命令模式
```

#### 主要 API 接口

##### unir_uart_demo_init
任务初始化函数
- 输出 UART Demo 启动日志
- 检查任务是否已创建
- 创建 UART 示例任务，设置堆栈大小和优先级
- 设置任务名称和入口函数

##### unir_uart_demo_process
任务处理函数
- 注册 UART 事件回调
- 配置 UART 波特率、数据位、停止位、校验位和流控参数
- 配置 UART TX/RX 及流控引脚
- 打开 UART 端口
- 根据当前测试用例执行发送、接收、波特率切换或模式切换测试

##### unir_uart_ind
UART 事件回调函数
- 处理串口接收事件
- 处理发送完成事件
- 处理发送缓冲区不足，超出阈值事件
- 将事件信息通过 UART 输出

##### unir_demo_uart_case_switch
测试用例切换函数
- 切换当前 UART 测试场景
- 支持输出测试、回调接收、主动读取、波特率切换和 UART/AT 模式切换

## 配置说明

默认 UART 配置定义在 `uart_demo.h` 中：

- `UNIR_TEST_UART_PORT`：默认测试端口为 `QOSA_UART_PORT_1`
- `UNIR_TEST_UART_TX_PIN`：默认 TX 引脚为 `UNIR_UART1_TX_PIN`
- `UNIR_TEST_UART_RX_PIN`：默认 RX 引脚为 `UNIR_UART1_RX_PIN`
- `CONFIG_UNIRTOS_UART_DEMO_TASK_STACK_SIZE`：任务栈大小为 4096
- `UNIR_UART_DEMO_TASK_PRIO`：任务优先级为 `QOSA_PRIORITY_NORMAL`

不同平台的 PINMUX 定义可能存在差异，请根据实际平台的 PINMUX 表调整 UART 引脚和复用功能配置。

## 论坛社区

https://forumschinese.quectel.com/c/66-category/66

## 贡献指南

欢迎提交 Issue 和 Pull Request！
