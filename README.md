# 8295 车机端侧 LLM 推理框架

## 项目简介

本项目基于 `llama.cpp` 深度定制开发，面向 8295 芯片的车载信息娱乐系统（IVI），提供高性能、低延迟的本地端侧大语言模型推理能力。

## 主要特性

- **8295 芯片优化**：针对 8295 架构进行编译和运行时优化。
- **OpenCL GPU 加速**：利用 OpenCL 实现 GPU 计算加速，提升推理性能。
- **端侧部署**：支持车机系统本地推理，减少对网络的依赖。
- **轻量化设计**：优化资源占用，适配内存和存储受限的车载环境。

## 适用场景

- 车载语音助手
- 信息娱乐系统自然语言交互
- 离线语义理解与生成
- 智能车机推荐与导航辅助

## 准备工作

1. 确保车机系统已获取 `root` 权限。
2. 安装并配置 `ADB` 工具。
3. 准备好以下文件：
   - `llama-cli`
   - `llama-bench`
   - `libOpenCL.so`
   - `model-Q4_0.gguf`

## 部署步骤

### 1. 创建部署目录

```bash
adb shell "mkdir -p /data/local/tmp/llama"
```

### 2. 传输文件到车机

```bash
adb push llama-cli /data/local/tmp/llama/
adb push llama-bench /data/local/tmp/llama/
adb push libOpenCL.so /data/local/tmp/llama/
adb push model-Q4_0.gguf /data/local/tmp/llama/
```

### 3. 设置可执行权限

```bash
adb shell chmod +x /data/local/tmp/llama/llama-cli
adb shell chmod +x /data/local/tmp/llama/llama-bench
```

### 4. 配置运行环境

```bash
adb shell
cd /data/local/tmp/llama
export LD_LIBRARY_PATH=.
```

> 如果车机系统缺少原生 OpenCL 库，`LD_LIBRARY_PATH=.` 可确保运行时优先加载当前目录下的 `libOpenCL.so`。

## 运行与测试

### 1. 使用 `llama-cli` 启动交互式推理

```bash
./llama-cli -m model-Q4_0.gguf
```

### 2. 执行基准测试

- 纯 CPU 测试：

```bash
./llama-bench -m model-Q4_0.gguf -p 512 -n 128 -ngl 0
```

- GPU 加速测试：

```bash
./llama-bench -m model-Q4_0.gguf -p 512 -n 128 -ngl 999
```

## 文件说明

- `llama-cli`：命令行推理客户端。
- `llama-bench`：性能基准测试工具。
- `libOpenCL.so`：OpenCL 运行时库，用于 GPU 加速。
- `model-Q4_0.gguf`：量化模型文件。

## 技术栈

- 核心框架：`llama.cpp`
- 硬件平台：8295 芯片
- 加速方案：OpenCL GPU 加速
- 模型格式：GGUF 量化模型

## 贡献与反馈

欢迎提交 Issue 或 Pull Request，共同提升 8295 端侧推理框架的稳定性、兼容性与性能。

## 许可协议

本项目遵循仓库根目录中的 `LICENSE` 约定。