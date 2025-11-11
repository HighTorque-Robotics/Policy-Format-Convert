经过一番仿真训练，机器人终于练会了专属 “技能包”（策略模型）～ 但光有模型还不够，得给它做个 “适配改造”，才能在真机上顺畅 “施展拳脚”！
接下来就进入关键的 “策略转换环节”—— 我们准备了两套 “适配方案”：一套是 Ubuntu 环境下的 ONNX 转 RKNN，为本地部署量身定制；另一套是 Pi Plus Pro 机器人（NVIDIA Jetson Orin Nx 版）专属的 ONNX 转 TensorRT，直接调用硬件算力拉满效率～ 跟着步骤操作，让训练成果快速落地！
一、onnx转换为rknn
操作平台：Ubuntu20.04
为了避免环境冲突，在自己电脑中单独创建rknn转换环境：
conda create -n rknn_model  python=3.8
conda activate rknn_model
pip install rknn-toolkit2
pip install --upgrade pillow
修改脚本文件中加载、保存文件路径：
# 修改加载路径
print("--> Loading model")
    ret = rknn.load_onnx("{your_path_to_load}/your_policy.onnx")

# 修改输出路径
OUT_DIR = "{your_path_to_save}"
    RKNN_MODEL_PATH = "{}/policy_from_onnx.rknn".format(OUT_DIR)
运行转换脚本：
python onnx2rknn.py
二、onnx转 TensorRT 
操作平台：Pi Plus Pro机器人NVIDIA Jetson Orin Nx版本已默认安装CUDA、TensoRT，无需配置环境
执行以下命令定位系统中trtexec可执行文件的位置：
find /usr -name "trtexec"
使用找到的trtexec工具执行转换命令，示例如下：
/usr/src/tensorrt/bin/trtexec --onnx=/home/nvidia/sim2real_master/sim2real_master/install/share/sim2real/policy/up/policy_0326_12dof_4000_pitch.onnx --saveEngine=/home/nvidia/sim2real_master/sim2real_master/install/share/sim2real/policy/up/policy_1023.trt 
