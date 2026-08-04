# SIG_COND - 高精度频率计前端双通道信号调理电路模块

[![Build Status](https://img.shields.io/badge/build-passing-brightgreen)]()
[![Version](https://img.shields.io/badge/version-1.0.0-blue)]()
[![License](https://img.shields.io/badge/license-MIT-green)]()

## 项目概述

SIG_COND 是一款专为高精度频率计设计的前端双通道信号调理电路模块，提供卓越的信号采集和处理能力。该模块支持灵活的阻抗切换和 AC/DC 耦合切换，能够处理从微伏级到大幅度的宽范围信号，并配备标准的 SPI 和 I2C 双接口通信方式。

### 核心特性

- ✅ **双通道独立信号调理** - 两个完全独立的信号通道，可同时采集两路信号
- ✅ **阻抗自动/手动切换** - 支持高阻抗和低阻抗模式的灵活切换，适配不同信号源
- ✅ **AC/DC 耦合切换** - 支持交流耦合和直流耦合模式切换，适应多种应用场景
- ✅ **宽动态范围** - 从小幅度微弱信号到大幅度强信号，全覆盖处理
- ✅ **双接口通信** - 同时支持 SPI 和 I2C 接口，灵活选择通信方式
- ✅ **高精度信号采集** - 低噪声、低失真设计，确保测量精度
- ✅ **模块化设计** - 分层架构，易于集成和扩展

## 目录结构

```
SIG_COND-master/
├── firmware/          # 固件代码（MCU 控制程序）
│   ├── boards/        # 板级配置文件
│   ├── include/       # 头文件
│   ├── lib/           # 第三方库
│   ├── src/           # 源代码
│   │   ├── app/       # 应用层业务逻辑
│   │   ├── drivers/   # 外设驱动（SPI/I2C 等）
│   │   ├── hal/       # 硬件抽象层
│   │   └── services/  # 服务层（通信、配置管理等）
│   └── tests/         # 单元测试
│
├── hardware/          # 硬件设计文件
│   ├── schematic/     # 原理图
│   ├── pcb/           # PCB 布局文件
│   ├── bom/           # 物料清单
│   ├── gerber/        # 生产 Gerber 文件
│   ├── 3d/            # 3D 结构模型
│   └── pinout/        # 引脚定义
│
├── software/          # 配套软件
│   ├── desktop/       # PC 桌面应用
│   ├── mobile/        # 移动应用
│   └── cloud/         # 云端服务
│
├── docs/              # 技术文档
│   ├── architecture/  # 架构设计文档
│   ├── api/           # API 接口文档
│   ├── hardware/      # 硬件说明
│   └── user-guide/    # 用户手册
│
├── tests/             # 系统测试
│   ├── unit/          # 单元测试
│   ├── integration/   # 集成测试
│   └── hwt/           # 硬件在环测试
│
├── tools/             # 工具链
│   ├── scripts/       # 构建脚本
│   └── utilities/     # 辅助工具
│
└── ci/                # CI/CD 流水线配置
```

## 硬件规格

### 通道特性

| 参数 | 指标 |
|------|------|
| 通道数 | 2 路独立通道 |
| 输入阻抗 | 可编程切换（高阻/低阻） |
| 耦合方式 | AC/DC 可选 |
| 输入范围 | 微伏级～大幅度信号 |
| 信噪比 | 高信噪比设计 |
| 带宽 | 宽频带响应 |

### 接口规范

#### SPI 接口
- 时钟频率：最高可达 XX MHz
- 模式：支持 Mode 0/1/2/3
- 数据位：8/16/32 位可选
- 引脚定义：见 `hardware/pinout/pinout.md`

#### I2C 接口
- 速率：标准模式 (100kHz)、快速模式 (400kHz)、高速模式 (3.4MHz)
- 地址：可配置 I2C 地址
- 支持多主机和从机模式

## 软件架构

本项目采用**分层模块化架构**，核心设计原则：

- **更换 MCU 只改 HAL 层，业务逻辑零修改**
- **各层解耦，独立编译和测试**
- **支持多板级并行开发**

### 层次结构

```
┌─────────────────┐
│     App Layer   │  ← 业务逻辑、状态机、用户交互
├─────────────────┤
│  Services Layer │  ← 通信协议、配置管理、数据存储
├─────────────────┤
│   Drivers Layer │  ← SPI/I2C 驱动、外设控制
├─────────────────┤
│     HAL Layer   │  ← GPIO、时钟、中断抽象
└─────────────────┘
```

## 快速开始

### 环境要求

- CMake 3.16+
- ARM GCC 工具链 (10-bit-32bit)
- Python 3.8+ (用于构建脚本)
- Git + Git LFS (大文件支持)

### 构建固件

```bash
# 克隆仓库
git clone --recurse-submodules https://github.com/your-repo/SIG_COND.git
cd SIG_COND

# Debug 版本构建
cmake -B firmware/build -DBOARD=default
cmake --build firmware/build

# Release 版本构建
cmake -B firmware/build -DBOARD=default -DCMAKE_BUILD_TYPE=Release
cmake --build firmware/build --config Release

# 使用构建脚本（推荐）
python tools/scripts/build.py --board=default
python tools/scripts/build.py --board=default --release
```

### 烧录固件

```bash
# 烧录到目标板
python tools/scripts/build.py --flash
```

## 使用说明

### 初始化配置

1. **接口选择**：通过软件配置选择 SPI 或 I2C 通信接口
2. **阻抗设置**：配置通道输入阻抗模式（高阻/低阻）
3. **耦合方式**：选择 AC 或 DC 耦合
4. **增益设置**：根据信号幅度调整放大倍数

### 典型应用场景

- 🔬 实验室精密测量
- 📡 通信信号分析
- ⚡ 电力电子监测
- 🎵 音频信号测试
- 🏭 工业过程控制

## 开发指南

### 新增功能

```bash
# 创建功能分支
git checkout -b feat/new-feature

# 开发完成后
git add .
git commit -m "feat: 添加新功能"
git push origin feat/new-feature
```

### 硬件改版

当需要修改硬件设计时：

```bash
# 1. 更新原理图和 PCB（在 EDA 工具中）
# 2. 更新引脚定义
cp -r hardware/schematic/v1.0 hardware/schematic/v2.0

# 3. 更新固件板级配置
cp -r firmware/boards/default firmware/boards/v2.0

# 4. 编译验证
cmake -B firmware/build -DBOARD=v2.0
```

## 测试

```bash
# 运行单元测试
python tools/scripts/build.py --test=unit

# 运行集成测试
python tools/scripts/build.py --test=integration

# 完整测试套件
python tools/scripts/build.py --test=all
```

## 版本历史

### v1.0.0 (2026-08-04)
- ✨ 初始版本发布
- ✅ 实现双通道信号调理
- ✅ 完成阻抗切换功能
- ✅ 完成 AC/DC 切换功能
- ✅ 支持 SPI 和 I2C 接口
- ✅ 实现宽范围信号处理能力

## 技术支持

如需技术支持或遇到问题，请：

1. 查阅 [用户手册](docs/user-guide/)
2. 查看 [API 文档](docs/api/)
3. 提交 [GitHub Issue](https://github.com/your-repo/SIG_COND/issues)

## 贡献指南

欢迎贡献代码！请阅读 [贡献指南](CONTRIBUTING.md) 后提交 PR。

## 许可证

本项目采用 MIT 许可证。详见 [LICENSE](LICENSE) 文件。

---

**注意**：本产品仅供教育和专业用途，使用前请确保安全操作规范。
