# 8095车机端侧LLM推理框架
## 项目概述
本项目是基于8095芯片深度优化的端侧大语言模型推理框架，基于llama.cpp进行定制化开发，专为车载信息娱乐系统(IVI)环境设计。
## 技术特点
- **硬件针对性优化**：针对8095芯片架构进行编译优化
- **端侧部署**：支持车载系统本地化推理
- **高性能**：利用OpenCL加速GPU计算
- **轻量化**：优化的内存管理和资源分配
## 部署指南
### 环境准备
确保车机系统已root，并安装ADB工具
### 文件部署
```bash
# 创建目标目录
adb shell "mkdir -p /data/local/tmp/llama"
# 推送核心文件
adb push build-android/bin/llama-cli /data/local/tmp/llama/
adb push build-android/bin/llama-bench /data/local/tmp/llama/
adb push build-android/bin/libOpenCL.so /data/local/tmp/llama/
adb push model-Q4_0.gguf /data/local/tmp/llama/
# 设置执行权限
adb shell chmod +x /data/local/tmp/llama/llama-cli
adb shell chmod +x /data/local/tmp/llama/llama-bench
环境配置
# 进入车机shell
adb shell
# 导航至部署目录
cd /data/local/tmp/llama
# 设置库路径（如车机缺少原生OpenCL库）
export LD_LIBRARY_PATH=.
性能测试
基准测试
# 纯CPU基准测试
./llama-bench -m model-Q4_0.gguf -p 512 -n 128 -ngl 0
# GPU加速测试（如支持）
./llama-bench -m model-Q4_0.gguf -p 512 -n 128 -ngl 999
项目文件说明
llama-cli：命令行接口程序
llama-bench：性能基准测试工具
libOpenCL.so：OpenCL运行时库
model-Q4_0.gguf：量化模型文件
技术栈
核心框架：llama.cpp
硬件平台：8095芯片
加速技术：OpenCL GPU加速
模型格式：GGUF量化模型
贡献指南
欢迎提交Issue和Pull Request，共同完善8095端侧推理框架。