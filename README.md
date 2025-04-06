# 自适应光学环境配置指南

[简体中文](/README.md) | [English](/README_EN.md)

网络上收集的相关自适应光学与深度学习或强化学习的仓库运行的相关代码，复现均使用docker进行环境配置，给后来者更好的借鉴复现。

|项目名称| 简要介绍    | 类型|   论文    |
| :--------------------------: | ------------------------ | ------ | :-----------: |
| [AO-MARL](https://github.com/Tomeu7/AO-MARL)| Multi-Agent AO+RL| SHWFS  | Adaptive Optics control with Multi-Agent Model-Free Reinforcement Learning(2022)[![DOI](https://zenodo.org/badge/doi/10.1364/OE.444099.svg)](https://doi.org/10.1364/OE.444099) |
| [Integrating-SL-and-RL-for-AO](https://github.com/Tomeu7/Integrating-SL-and-RL-for-AO) | UNET + RL| PYWFS   | Integrating supervised and reinforcement learning for predictive control with an unmodulated pyramid wavefront sensor for adaptive optics(2024)[![DOI](https://zenodo.org/badge/doi/10.1364/oe.530254.svg)](https://doi.org/10.1364/oe.530254)|
|[MOSS-DDPG](https://github.com/xgz0921/MOSS-DDPG)| RL|sensorless|Image metric-based multi-observation single-step deep deterministic policy gradient for sensorless adaptive optics(2024)[![DOI](https://zenodo.org/badge/doi/10.1364/BOE.528579.svg)](https://doi.org/10.1364/BOE.528579)|

---
未完待续：

- ~~|[MOSS-DDPG](https://github.com/xgz0921/MOSS-DDPG)| sensorless|~~
- [TelescopeReinforcementLearning](https://github.com/Faruman/TelescopeReinforcementLearning)
- [adaptive_optics_ml](https://github.com/ZoyaV/adaptive_optics_ml)
- [ML4AO](https://github.com/HeSunPU/ML4AO)
- [adaptive_optics_gym](https://github.com/payamparvizi/adaptive_optics_gym)
- [Machine-Learning-and-System-Identification-for-Adaptive-Optics](https://github.com/AleksandarHaber/Machine-Learning-and-System-Identification-for-Adaptive-Optics)
- [MPC-SensorlessAO](https://github.com/jinsungkim96/MPC-SensorlessAO)

---

## 前言

大多数的深度强化学习环境配置需要启用GPU支持，考虑的docker的优越性`build once，run anywhere`，必须要启用NVIDIA GPU的docker runtime。具体安装步骤参考：[NVIDIA Container Toolkit](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html)

1. 提前安装NVIDIA GPU支持(以Ubuntu或者Debian为例，适用于APT-based的操作系统)

    - 国外操作：

    ```shell
    curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg && \   
    curl -s -L https://nvidia.github.io/libnvidia-container/stable/deb/nvidia-container-toolkit.list | \
        sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' | \
        sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list
    ```

    - 国内操作(主要是使用可连接的文件，避免`github.io`无法访问的问题)：

    ```shell
    curl -fsSL https://pan.baos.eu.org/d/Open/Others/gpgkey?sign=zFEqUNjXtIYLGltIQL2cwwBAeV0eOFjNC8KUX6hNNzY=:0 | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg && \   
    curl -s -L https://pan.baos.eu.org/d/Open/Others/nvidia-container-toolkit.list?sign=HYpwAvRrD9QgK6LR7Y7RivkUHRuz4hiZxreYfQfCk60=:0 | \
        sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' | \
        sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list
    ```

1. 重启docker服务

```shell
sudo apt-get update && sudo apt-get install -y nvidia-container-toolkit
sudo nvidia-ctk runtime configure --runtime=docker
sudo systemctl restart docker
```

## 运行教程

1. 直接进入各自的代码文件夹，根据各自仓库的说明运行相关的代码。
以`AO-MARL`为例，直接运行`cd AO-MARL && docker compose up -d`启动容器，使用`docker exec -it AO-MARL bash`进入容器当中，运行具体的实验命令`python main.py --parameters_telescope "production_sh_10x10_2m.py" --world-size 3 --seed 1234 --experiment_name "training" --n_reverse_filtered_from_cmat 5 --n_zernike_start_end 0  80 --port 25003`。建议提前使用`tmux`便于保存当前命令的运行状态。
1. 合并命令如下：

```shell
# 宿主机内操作
cd AO-MARL && docker compose up -d
docker exec -it AO-MARL bash
# 容器内部操作
python main.py --parameters_telescope "production_sh_10x10_2m.py" --world-size 3 --seed 1234 --experiment_name "training" --n_reverse_filtered_from_cmat 5 --n_zernike_start_end 0  80 --port 25003
```

  ⚠️需要注意的是：当前容器挂载源码的路径为`/workspace/仓库名`，因此建议运行结果保存在当前目录下实现持久化保存。

## Docker-Compose参数说明

- `-it`：以交互模式运行容器。
- `--gpus all`：启用所有 GPU（需要安装 (<https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html)）。>
- `-v /home/hhn/AO-MARL:/workspace/AO-MARL`：将本地的 `/home/hhn/AO-MARL` 挂载到容器内的 `/workspace/AO-MARL`。
- `-v /data:/data`：将本地的 `/data` 挂载到容器内的 `/data`。
- `pytorch/pytorch:1.8.1-cuda10.2-cudnn7-runtime`：使用的 Docker 镜像。

## 参考仓库

对以下仓库作者表示感谢。

1. [Integrating-SL-and-RL-for-AO](https://github.com/Tomeu7/Integrating-SL-and-RL-for-AO)
1. [AO-MARL](https://github.com/Tomeu7/AO-MARL)

---
