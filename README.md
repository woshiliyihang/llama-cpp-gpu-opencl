# 这是一个基于8095 芯片优化编译的llama.cpp 端侧框架。
# This is a llama.cpp end-to-end framework optimized and compiled based on the 8095 chip.

车机部署与性能测试
1. 推送文件到车机
# 在车机上创建目录
adb shell "mkdir -p /data/local/tmp/llama"
# 推送可执行文件、依赖库和模型
adb push build-android/bin/llama-cli       /data/local/tmp/llama/
adb push build-android/bin/llama-bench     /data/local/tmp/llama/
adb push build-android/bin/libOpenCL.so    /data/local/tmp/llama/
adb push model-Q4_0.gguf                   /data/local/tmp/llama/
# 赋予执行权限
adb shell chmod +x /data/local/tmp/llama/llama-cli
adb shell chmod +x /data/local/tmp/llama/llama-bench
2. 运行性能验证
# 进入车机 shell
adb shell
cd /data/local/tmp/llama
# 设置库搜索路径（车机若无原生 OpenCL 库时必须）
export LD_LIBRARY_PATH=.
# 测试 1：纯 CPU 基准（ngl=0）
./llama-bench -m model-Q4_0.gguf -p 512 -n 128 -ngl 0
