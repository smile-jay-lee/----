
# 面向轻量化定位与地图构建方法研究

项目基于视觉-惯性 SLAM（VI-SLAM）研究轻量化定位与地图构建策略，目标是在资源受限平台（如 Jetson Nano）上降低计算与内存开销，同时保持可接受的定位精度与地图一致性。本工程以复现 VINS-Fusion 为基线，并基于 OpenVINS 引入前端 KLT 跟踪、分步边缘化与滑动窗口地图稀疏化（SWMS）等轻量化技术。

**目录概览**
- `开题报告.md`：开题报告正文（已包含背景、研究内容、技术路线与进度）。
- `readme.md`：项目说明（当前文件）。

**关键点与贡献**
- 问题定位：强调 VI-SLAM 在嵌入式平台上面临的“计算量 + 内存消耗”双重挑战，以及回环检测带来的额外负载。 
- 轻量化策略：前端采用 Shi-Tomasi/GFTT + 金字塔 KLT 跟踪；后端探索分步边缘化以降低峰值计算；引入 SWMS 控制长期运行的内存增长。
- 评测口径：使用 `evo` 评测 ATE / RPE（RMSE），并记录 FPS、CPU 占用与内存峰值。

**环境与依赖（推荐）**
- Ubuntu 20.04 或 22.04（开发与部署）
- Python 3.8+（用于评测脚本与数据处理）
- ROS（可选）或无 ROS 的 VINS/OpenVINS 实现
- OpenCV, Eigen, CMake, g++
- `evo`（轨迹评估工具）

安装示例（Ubuntu）：
```bash
sudo apt update
sudo apt install -y build-essential cmake git python3-pip
pip3 install evo
# 额外依赖请参见具体实现目录的 README
```

**快速开始（复现实验流程）**
1. 下载 EuRoC/TUM-VI 数据集并解压。
2. 复现 VINS-Fusion：编译并在样例序列上运行，保存输出轨迹（TUM 格式）。
3. 基线评测：使用 `evo` 计算 ATE/RPE，并同时记录 FPS、CPU/内存数据。
4. 在 OpenVINS 上实现轻量化模块（KLT、分步边缘化、SWMS），重复以上评测并比对结果。

**评测与复现建议**
- 指标：ATE（RMSE）、RPE（RMSE）、帧率（FPS）、CPU 平均/峰值占用、内存峰值。
- 对比方式：相同数据集、相同硬件（或 Docker 模拟），固定测量口径与记录脚本，输出可复现的表格与图像。

**实验结果记录**
- 建议在 `experiments/` 目录下为每次对比建立子目录，包含：原始轨迹、处理脚本、评测结果（CSV/PNG）。

**联系方式**
- 学生：李杰（学号 226002618）
- 指导教师：彭成斌

如果你希望，我可以：
- 把 README 扩写为英文版并生成实验模板脚本（运行与记录自动化）。
- 在仓库中创建 `experiments/` 模板和评测脚本（`run_eval.sh`）。

