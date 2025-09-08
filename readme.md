# NS-3 3.44 网络仿真技术指南

## 目录
- [NS-3 简介](#ns-3-简介)
- [安装部署教程](#安装部署教程)
- [目录结构解析](#目录结构解析)
- [常用命令和参数](#常用命令和参数)
- [NetAnim 可视化](#netanim-可视化)
- [示例程序](#示例程序)
- [常见问题](#常见问题)

## NS-3 简介
<!--
作者：snowcrescenter
-->
NS-3（Network Simulator 3）是一个离散事件网络仿真器，主要用于研究和教育目的。它支持多种网络协议和技术，包括有线网络、无线网络、移动网络等。

## 安装部署教程

### 1. 更新系统包

```bash
sudo apt update && sudo apt upgrade -y
```

### 2. 一键安装所有依赖包

```bash
# 基础核心依赖（必须）
sudo apt install -y gcc g++ python3 python3-dev python3-setuptools cmake ninja-build git mercurial

# Qt 和 GUI 相关依赖
sudo apt install -y qt5-qmake qtbase5-dev qtchooser qtbase5-dev-tools

# 网络和系统依赖
sudo apt install -y libxml2-dev libgtk-3-dev libgsl-dev libgcrypt20-dev

# 调试和分析工具
sudo apt install -y gdb valgrind

# Python 绑定和可视化
sudo apt install -y gir1.2-goocanvas-2.0 python3-gi python3-gi-cairo python3-pygraphviz python3-kiwisolver python3-seaborn gir1.2-gtk-3.0 ipython3

# 网络虚拟化工具（可选，如果包不存在会跳过）
sudo apt install -y lxc-templates uml-utilities bridge-utils ebtables

# Boost 库
sudo apt install -y libboost-all-dev
```

**简化版一键安装命令（推荐）：**
```bash
sudo apt install -y gcc g++ python3 python3-dev cmake ninja-build git qt5-qmake qtbase5-dev qtbase5-dev-tools libxml2-dev libgtk-3-dev libgsl-dev gdb valgrind gir1.2-goocanvas-2.0 python3-gi python3-gi-cairo python3-pygraphviz libboost-all-dev
```

### 3. 下载 NS-3 3.44

```bash
# 创建工作目录
mkdir ~/ns3-workspace
cd ~/ns3-workspace

# 下载 NS-3 3.44 完整包
wget https://www.nsnam.org/releases/ns-allinone-3.44.tar.bz2
tar -xjf ns-allinone-3.44.tar.bz2
cd ns-allinone-3.44
```

### 4. 编译安装

```bash
# 使用 build.py 脚本进行完整安装（推荐）
./build.py --enable-examples --enable-tests

# 或者单独编译 NS-3 核心
cd ns-3.44
./ns3 configure --enable-examples --enable-tests
./ns3 build
```

### 5. 验证安装

```bash
# 进入 ns-3.44 目录
cd ns-3.44

# 运行测试程序
./ns3 run hello-simulator

# 如果成功，将输出: Hello Simulator
```

### 6. 设置环境变量（可选）

```bash
# 添加到 ~/.bashrc
echo 'export NS3_HOME=~/ns3-workspace/ns-allinone-3.44/ns-3.44' >> ~/.bashrc
echo 'export PATH=$NS3_HOME:$PATH' >> ~/.bashrc
source ~/.bashrc
```

## 目录结构解析

### ns-allinone-3.44 根目录结构

```
ns-allinone-3.44/
├── bake/                 # Bake 构建系统目录
├── build.py              # 自动构建脚本
├── constants.py          # 构建常量定义
├── netanim-3.109/        # NetAnim 网络动画工具
├── ns-3.44/              # NS-3 核心仿真器目录
├── __pycache__/          # Python 缓存目录
├── README.md             # 说明文档
└── util.py               # 构建工具脚本
```

### NS-3 核心目录（ns-3.44/）结构详解

```
ns-3.44/
├── AUTHORS               # 作者和贡献者信息
├── bindings/            # Python/Java 语言绑定
├── build/               # 编译输出目录
├── build-support/       # 构建支持文件
├── CHANGES.md           # 版本更新日志
├── cmake-cache/         # CMake 缓存目录
├── CMakeLists.txt       # CMake 构建配置文件
├── CODE_OF_CONDUCT.md   # 社区行为准则
├── contrib/             # 第三方贡献模块目录
├── CONTRIBUTING.md      # 贡献指南
├── doc/                 # 文档目录
├── examples/            # 示例程序目录
├── LICENSE              # 开源许可证
├── LICENSES/            # 许可证文件夹
├── ns3*                 # 主要构建和运行脚本
├── __pycache__/         # Python 缓存
├── pyproject.toml       # Python 项目配置
├── README.md            # 项目说明
├── RELEASE_NOTES.md     # 发行说明
├── scratch/             # 用户自定义脚本目录
├── setup.cfg            # Python 安装配置
├── setup.py             # Python 安装脚本
├── src/                 # 核心源代码目录
├── test.py              # 测试脚本
├── testpy-output/       # Python 测试输出
├── utils/               # 实用工具目录
├── utils.py             # 工具脚本
└── VERSION              # 版本信息文件
```

### 核心目录详解

#### 1. `src/` 目录 - 核心模块
```
src/
├── antenna/          # 天线模型和方向图
├── applications/     # 应用层协议（HTTP, FTP, UDP Echo等）
├── bridge/          # 网桥和交换机模块
├── buildings/       # 建筑物传播模型
├── config-store/    # 配置存储和管理
├── core/           # 核心功能（事件调度、日志、随机数等）
├── csma/           # CSMA/CD 以太网协议
├── dsdv/           # DSDV 路由协议
├── dsr/            # DSR 动态源路由协议
├── energy/         # 能耗模型和电池模型
├── fd-net-device/  # 文件描述符网络设备
├── flow-monitor/   # 流量监控和统计
├── internet/       # TCP/IP 协议栈实现
├── internet-apps/  # 网络应用（Ping, DHCPv4等）
├── lr-wpan/        # IEEE 802.15.4 低速率无线个域网
├── lte/            # LTE/4G 蜂窝网络模块
├── mesh/           # 网状网络协议
├── mobility/       # 移动性模型（随机游走、恒速等）
├── netanim/        # NetAnim 动画接口
├── network/        # 网络基础设施（节点、设备、包等）
├── nix-vector-routing/ # Nix-Vector 路由算法
├── olsr/           # OLSR 链路状态路由协议
├── point-to-point/ # 点对点连接模块
├── propagation/    # 信号传播模型（自由空间、对数距离等）
├── sixlowpan/      # 6LoWPAN IPv6 over 802.15.4
├── spectrum/       # 频谱模型和干扰计算
├── stats/          # 统计收集和分析
├── tap-bridge/     # TAP 设备桥接
├── test/           # 单元测试框架
├── topology-read/  # 拓扑文件读取
├── traffic-control/ # 流量控制和QoS
├── uan/            # 水下声学网络
├── virtual-net-device/ # 虚拟网络设备
├── wave/           # WAVE/DSRC 车联网协议
├── wifi/           # IEEE 802.11 WiFi模块
└── wimax/          # IEEE 802.16 WiMAX模块
```

#### 2. `examples/` 目录 - 示例程序
```
examples/
├── energy/         # 能耗模型使用示例
├── matrix-topology/ # 矩阵拓扑网络示例
├── naming/         # 对象命名示例
├── realtime/       # 实时仿真示例
├── routing/        # 各种路由协议示例
├── stats/          # 统计数据收集示例
├── tcp/            # TCP协议变种比较示例
├── tutorial/       # 入门教程示例（first.cc, second.cc, third.cc）
├── udp/            # UDP应用示例
├── wireless/       # 无线网络拓扑示例
└── ...
```

#### 3. `scratch/` 目录 - 用户脚本
用户自定义仿真脚本存放目录，所有放在此目录的 `.cc` 文件都会被自动编译。这是开发和测试新仿真脚本的主要工作区域。

#### 4. `contrib/` 目录 - 第三方模块
存放第三方贡献的扩展模块，如特定的协议实现或应用。

#### 5. `utils/` 目录 - 实用工具
```
utils/
├── bench-packets.cc      # 包处理性能测试
├── bench-simulator.cc    # 仿真器性能测试
├── perf/                # 性能分析工具
├── print-introspected-doxygen.cc # 文档生成工具
└── ...
```


## 常用命令和参数

### 1. 基本构建命令

```bash
# 配置构建环境
./ns3 configure

# 清理构建
./ns3 clean

# 完整构建
./ns3 build

# 增量构建
./ns3

# 配置并启用调试模式
./ns3 configure --build-profile=debug

# 配置并启用优化模式
./ns3 configure --build-profile=optimized

# 启用示例和测试
./ns3 configure --enable-examples --enable-tests
```

### 2. 运行仿真程序

```bash
# 运行示例程序
./ns3 run "program-name"

# 运行带参数的程序
./ns3 run "program-name --arg1=value1 --arg2=value2"

# 运行 scratch 目录中的程序
./ns3 run "scratch/my-simulation"

# 运行时显示详细输出
./ns3 run "program-name" --verbose

# 在调试器中运行
./ns3 run "program-name" --gdb

# 在 Valgrind 中运行（内存检查）
./ns3 run "program-name" --valgrind
```

### 3. 常用运行时参数

```bash
# 设置仿真时间
./ns3 run "program-name --duration=10.0"

# 设置随机种子
./ns3 run "program-name --RngSeed=1"

# 启用 PCap 跟踪
./ns3 run "program-name --pcap=true"

# 启用 ASCII 跟踪
./ns3 run "program-name --ascii=true"

# 设置详细日志级别
NS_LOG=UdpEchoClientApplication=level_all ./ns3 run "program-name"

# 设置多个模块的日志
NS_LOG="UdpEchoClientApplication=level_all:UdpEchoServerApplication=level_all" ./ns3 run "program-name"
```

### 4. 日志和调试

```bash
# 查看可用的日志组件
./ns3 run "program-name" --PrintGroups

# 设置日志级别
export NS_LOG=ComponentName=level_info

# 常用日志级别
# level_error, level_warn, level_info, level_debug, level_logic, level_function, level_all
```

### 5. 测试命令

```bash
# 运行所有测试
./test.py

# 运行特定测试套件
./test.py -s test-suite-name

# 运行测试并生成报告
./test.py -w results.html

# 快速测试
./test.py -q
```

### 6. 文档生成

```bash
# 生成 API 文档
./ns3 docs doxygen

# 生成手册
./ns3 docs manual

# 生成教程
./ns3 docs tutorial
```

## NetAnim 可视化

NetAnim 是 NS-3 的动画可视化工具，可以将仿真结果以动画形式展示。

### 1. 在仿真中启用 NetAnim

在您的仿真脚本中添加：

```cpp
#include "ns3/netanim-module.h"

int main() {
    // ... 仿真设置代码 ...
    
    // 启用 NetAnim 动画
    AnimationInterface anim("animation.xml");
    
    // 设置节点位置（可选）
    anim.SetConstantPosition(nodes.Get(0), 10.0, 10.0);
    anim.SetConstantPosition(nodes.Get(1), 20.0, 10.0);
    
    // 运行仿真
    Simulator::Run();
    Simulator::Destroy();
    
    return 0;
}
```

### 2. 查看动画

```bash
# 运行仿真生成动画文件
./ns3 run "your-simulation"

# 启动 NetAnim 查看动画
./netanim
# 然后在 NetAnim 中打开生成的 animation.xml 文件
```

### 3. NetAnim 功能

- **网络拓扑可视化**: 显示节点和链路
- **数据包传输动画**: 实时显示数据包流动
- **节点移动轨迹**: 显示移动节点的路径
- **统计信息**: 显示吞吐量、时延等统计数据
- **时间控制**: 控制动画播放速度和进度

## 示例程序

### 1. 简单的点对点网络

```bash
./ns3 run "first"
```

### 2. CSMA 网络

```bash
./ns3 run "second"
```

### 3. 无线网络

```bash
./ns3 run "third"
```

### 4. 混合网络拓扑

```bash
./ns3 run "mixed-wireless"
```

### 5. TCP 示例

```bash
./ns3 run "tcp-variants-comparison"
```

## 常见问题

### 1. 编译错误

**问题**: 缺少依赖包
```bash
# 解决方案：安装完整依赖
sudo apt install build-essential autotools-dev
```

**问题**: Python 版本不兼容
```bash
# 解决方案：确保使用 Python 3
./ns3 configure --enable-examples --enable-tests --with-python=/usr/bin/python3
```

### 2. 运行错误

**问题**: 找不到程序
```bash
# 解决方案：确保程序已编译
./ns3 build
```

**问题**: 权限问题
```bash
# 解决方案：检查文件权限
chmod +x ns3
```

### 3. 性能优化

```bash
# 使用优化构建配置
./ns3 configure --build-profile=optimized

# 使用多线程编译
./ns3 build -j$(nproc)
```

### 4. 获取帮助

```bash
# 查看命令帮助
./ns3 --help

# 查看程序特定帮助
./ns3 run "program-name --help"

# 在线文档
# https://www.nsnam.org/documentation/
```

## 结语

本指南涵盖了 NS-3 3.44 的基本安装、配置和使用方法。更多高级功能和详细API文档，请参考官方文档：https://www.nsnam.org/documentation/
<!--
作者：snowcrescenter
-->