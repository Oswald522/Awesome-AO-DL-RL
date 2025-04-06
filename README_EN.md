# Adaptive Optics Environment Configuration Guide

[简体中文](/README.md) | [English](/README_EN.md)

The code repositories related to adaptive optics combined with deep learning or reinforcement learning collected online are all reproduced using Docker for environment configuration, providing better references for future researchers.

| Project Name                | Introduction       | Type         | Paper |
| :--------------------------: | ------------------------ | ------ | :-----------: |
| [AO-MARL](https://github.com/Tomeu7/AO-MARL)| Multi-Agent AO+RL| SHWFS  | Adaptive Optics control with Multi-Agent Model-Free Reinforcement Learning(2022)[![DOI](https://zenodo.org/badge/doi/10.1364/OE.444099.svg)](https://doi.org/10.1364/OE.444099) |
| [Integrating-SL-and-RL-for-AO](https://github.com/Tomeu7/Integrating-SL-and-RL-for-AO) | UNET + RL| PYWFS   | Integrating supervised and reinforcement learning for predictive control with an unmodulated pyramid wavefront sensor for adaptive optics(2024)[![DOI](https://zenodo.org/badge/doi/10.1364/oe.530254.svg)](https://doi.org/10.1364/oe.530254)|
|[MOSS-DDPG](https://github.com/xgz0921/MOSS-DDPG)| RL|sensorless|Image metric-based multi-observation single-step deep deterministic policy gradient for sensorless adaptive optics(2024)[![DOI](https://zenodo.org/badge/doi/10.1364/BOE.528579.svg)](https://doi.org/10.1364/BOE.528579)|

---

To Be Continued：

- |[MOSS-DDPG](https://github.com/xgz0921/MOSS-DDPG)| sensorless|
- [TelescopeReinforcementLearning](https://github.com/Faruman/TelescopeReinforcementLearning)
- [adaptive_optics_ml](https://github.com/ZoyaV/adaptive_optics_ml)
- [ML4AO](https://github.com/HeSunPU/ML4AO)
- [adaptive_optics_gym](https://github.com/payamparvizi/adaptive_optics_gym)
- [Machine-Learning-and-System-Identification-for-Adaptive-Optics](https://github.com/AleksandarHaber/Machine-Learning-and-System-Identification-for-Adaptive-Optics)
- [MPC-SensorlessAO](https://github.com/jinsungkim96/MPC-SensorlessAO)

---

## Preface

Most deep reinforcement learning environment configurations require GPU support. Considering the superiority of Docker's "build once, run anywhere" philosophy, it is essential to enable the NVIDIA GPU Docker runtime. For detailed installation steps, refer to: [NVIDIA Container Toolkit](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html).

1. **Pre-install NVIDIA GPU support (for Ubuntu or Debian, applicable to APT-based systems)**:
    - **For International Users**:

    ```shell
    curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg && \
    curl -s -L https://nvidia.github.io/libnvidia-container/stable/deb/nvidia-container-toolkit.list | \
        sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' | \
        sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list
    ```

    - **For Users in China (to avoid inaccessible `github.io`)**:

    ```shell
    curl -fsSL https://pan.baos.eu.org/d/Open/Others/gpgkey?sign=zFEqUNjXtIYLGltIQL2cwwBAeV0eOFjNC8KUX6hNNzY=:0 | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg && \
    curl -s -L https://pan.baos.eu.org/d/Open/Others/nvidia-container-toolkit.list?sign=HYpwAvRrD9QgK6LR7Y7RivkUHRuz4hiZxreYfQfCk60=:0 | \
        sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' | \
        sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list
    ```

2. **Restart Docker Service**:

    ```shell
    sudo apt-get update && sudo apt-get install -y nvidia-container-toolkit
    sudo nvidia-ctk runtime configure --runtime=docker
    sudo systemctl restart docker
    ```

## Running Tutorial

1. Navigate to the respective code folder and follow the repository's instructions to run the code.
   For example, with `AO-MARL`, run `cd AO-MARL && docker compose up -d` to start the container. Use `docker exec -it AO-MARL bash` to enter the container and execute the experiment command:

   ```shell
   python main.py --parameters_telescope "production_sh_10x10_2m.py" --world-size 3 --seed 1234 --experiment_name "training" --n_reverse_filtered_from_cmat 5 --n_zernike_start_end 0 80 --port 25003
   ```

   It is recommended to use `tmux` to preserve the state of the running command.

2. **Combined Commands**:

    ```shell
    # Host machine operations
    cd AO-MARL && docker compose up -d
    docker exec -it AO-MARL bash
    # Container operations
    python main.py --parameters_telescope "production_sh_10x10_2m.py" --world-size 3 --seed 1234 --experiment_name "training" --n_reverse_filtered_from_cmat 5 --n_zernike_start_end 0 80 --port 25003
    ```

⚠️ **Note**: The source code is mounted to `/workspace/repository_name` in the container. It is recommended to save results in the current directory for persistent storage.

## Docker-Compose Parameter Explanation

- `-it`: Run the container in interactive mode.
- `--gpus all`: Enable all GPUs (requires installation of [NVIDIA Container Toolkit](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html)).
- `-v /home/hhn/AO-MARL:/workspace/AO-MARL`: Mount the local `/home/hhn/AO-MARL` to `/workspace/AO-MARL` in the container.
- `-v /data:/data`: Mount the local `/data` to `/data` in the container.
- `pytorch/pytorch:1.8.1-cuda10.2-cudnn7-runtime`: Docker image used.

## Reference Repositories

Special thanks to the authors of the following repositories:

1. [Integrating-SL-and-RL-for-AO](https://github.com/Tomeu7/Integrating-SL-and-RL-for-AO)
2. [AO-MARL](https://github.com/Tomeu7/AO-MARL)

---
