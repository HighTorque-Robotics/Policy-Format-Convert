# Policy Model Adaptation and Transformation: ONNX to RKNN/TensorRT

[![Ubuntu](https://img.shields.io/badge/Ubuntu-20.04-orange.svg)](https://releases.ubuntu.com/20.04/)
[![Python](https://img.shields.io/badge/python-3.8-blue.svg)](https://docs.python.org/3/whatsnew/3.8.html)
[![TensorRT](https://img.shields.io/badge/TensorRT-CUDA%20Compatible-silver.svg)](https://developer.nvidia.com/tensorrt)
[![RKNN](https://img.shields.io/badge/RKNN-Toolkit2-silver.svg)](https://github.com/rockchip-linux/rknn-toolkit2)
[![License](https://img.shields.io/badge/license-MIT-yellow.svg)](https://opensource.org/license/mit)

## Introduction

Robot policy models trained in simulation (in ONNX format) require adaptation and transformation for efficient deployment on physical hardware. This project provides two targeted conversion solutions:
- ONNX to RKNN on Ubuntu: Suitable for local deployment scenarios
- ONNX to TensorRT exclusive for Pi Plus Pro Robot (NVIDIA Jetson Orin Nx version): Leverages hardware computing power to maximize operational efficiency
No complex configurations are needed—follow the steps to quickly implement the deployment of trained models.

## I. ONNX to RKNN (Local Deployment on Ubuntu)

### Environment Requirements
- Operating System: Ubuntu 20.04
- Python Version: 3.8
- Dependent Tool: Conda (recommended)

### Operation Steps
#### 1. Create and Activate Conda Environment
To avoid environment conflicts, create a dedicated environment for RKNN conversion:
```bash
conda create -n rknn_model python=3.8
conda activate rknn_model
```

#### 2. Install Dependent Packages
```bash
pip install rknn-toolkit2
pip install --upgrade pillow
```

#### 3. Modify Conversion Script Paths
Edit the `onnx2rknn.py` script and replace placeholders with actual file paths:
```python
# Modify ONNX model loading path
print("--> Loading model")
ret = rknn.load_onnx("{your_path_to_load}/your_policy.onnx")  # Replace with your ONNX file path

# Modify RKNN model output path
OUT_DIR = "{your_path_to_save}"  # Replace with RKNN model save directory
RKNN_MODEL_PATH = "{}/policy_from_onnx.rknn".format(OUT_DIR)
```

#### 4. Execute Conversion
```bash
python onnx2rknn.py
```

## II. ONNX to TensorRT (Exclusive for Pi Plus Pro Robot)

### Environment Requirements
- Hardware Device: Pi Plus Pro Robot (NVIDIA Jetson Orin Nx version)
- Preinstalled Environment: The device comes with CUDA and TensorRT by default—no additional configuration required

### Operation Steps
#### 1. Locate the trtexec Tool
Find the `trtexec` executable file included with TensorRT in the system:
```bash
find /usr -name "trtexec"
```

#### 2. Execute ONNX to TensorRT Conversion
Use the `trtexec` path obtained in the previous step to run the conversion command (replace with actual paths):
```bash
# Example command (replace ONNX input path and TRT output path)
/usr/src/tensorrt/bin/trtexec \
--onnx=/home/nvidia/sim2real_master/sim2real_master/install/share/sim2real/policy/up/policy_0326_12dof_4000_pitch.onnx \
--saveEngine=/home/nvidia/sim2real_master/sim2real_master/install/share/sim2real/policy/up/policy_1023.trt
```

### Command Parameter Explanation
- `--onnx`: Specifies the full path to the input ONNX model file
- `--saveEngine`: Specifies the full path to the output TensorRT engine file (.trt format)

## Deployment Instructions
After conversion, the following format models can be directly loaded on the corresponding hardware devices:
- RKNN format: `.rknn` file (suitable for local deployment devices)
- TensorRT format: `.trt` file (suitable for Pi Plus Pro Robot)




# （中文翻译）策略模型适配改造：ONNX 转 RKNN/TensorRT

[![Ubuntu](https://img.shields.io/badge/Ubuntu-20.04-orange.svg)](https://releases.ubuntu.com/20.04/)
[![Python](https://img.shields.io/badge/python-3.8-blue.svg)](https://docs.python.org/3/whatsnew/3.8.html)
[![TensorRT](https://img.shields.io/badge/TensorRT-CUDA%20Compatible-silver.svg)](https://developer.nvidia.com/tensorrt)
[![RKNN](https://img.shields.io/badge/RKNN-Toolkit2-silver.svg)](https://github.com/rockchip-linux/rknn-toolkit2)
[![License](https://img.shields.io/badge/license-MIT-yellow.svg)](https://opensource.org/license/mit)

## 介绍

经过仿真训练的机器人策略模型（ONNX 格式），需通过适配转换才能在真机上高效部署。本项目提供两套针对性转换方案：
- Ubuntu 环境下 ONNX 转 RKNN：适配本地部署场景
- Pi Plus Pro 机器人（NVIDIA Jetson Orin Nx 版）专属 ONNX 转 TensorRT：调用硬件算力，最大化运行效率
无需复杂配置，跟随步骤即可快速实现训练成果落地。

## 一、ONNX 转 RKNN（Ubuntu 本地部署）

### 环境要求
- 操作系统：Ubuntu 20.04
- Python 版本：3.8
- 依赖工具：Conda（推荐）

### 操作步骤
#### 1. 创建并激活 Conda 环境
避免环境冲突，单独创建 RKNN 转换专属环境：
```bash
conda create -n rknn_model python=3.8
conda activate rknn_model
```

#### 2. 安装依赖包
```bash
pip install rknn-toolkit2
pip install --upgrade pillow
```

#### 3. 修改转换脚本路径
编辑 `onnx2rknn.py` 脚本，替换占位符为实际文件路径：
```python
# 修改 ONNX 模型加载路径
print("--> Loading model")
ret = rknn.load_onnx("{your_path_to_load}/your_policy.onnx")  # 替换为你的 ONNX 文件路径

# 修改 RKNN 模型输出路径
OUT_DIR = "{your_path_to_save}"  # 替换为 RKNN 模型保存目录
RKNN_MODEL_PATH = "{}/policy_from_onnx.rknn".format(OUT_DIR)
```

#### 4. 执行转换
```bash
python onnx2rknn.py
```

## 二、ONNX 转 TensorRT（Pi Plus Pro 机器人专属）

### 环境要求
- 硬件设备：Pi Plus Pro 机器人（NVIDIA Jetson Orin Nx 版）
- 预装环境：设备默认搭载 CUDA、TensorRT，无需额外配置

### 操作步骤
#### 1. 定位 trtexec 工具
查找系统中 TensorRT 自带的 `trtexec` 可执行文件：
```bash
find /usr -name "trtexec"
```

#### 2. 执行 ONNX 转 TensorRT 转换
使用上一步获取的 `trtexec` 路径，执行转换命令（替换为实际路径）：
```bash
# 示例命令（需替换 ONNX 输入路径和 TRT 输出路径）
/usr/src/tensorrt/bin/trtexec \
--onnx=/home/nvidia/sim2real_master/sim2real_master/install/share/sim2real/policy/up/policy_0326_12dof_4000_pitch.onnx \
--saveEngine=/home/nvidia/sim2real_master/sim2real_master/install/share/sim2real/policy/up/policy_1023.trt
```

### 命令参数说明
- `--onnx`：指定输入的 ONNX 模型文件完整路径
- `--saveEngine`：指定输出的 TensorRT 引擎文件（.trt 格式）完整路径

## 部署说明
转换完成后，可在对应硬件设备上直接加载以下格式模型：
- RKNN 格式：`.rknn` 文件（适用于本地部署设备）
- TensorRT 格式：`.trt` 文件（适用于 Pi Plus Pro 机器人）


