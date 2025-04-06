# 基于 PyTorch 官方镜像
# FROM pytorch/pytorch:1.8.1-cuda10.2-cudnn7-runtime
FROM pytorch/pytorch:1.9.0-cuda10.2-cudnn7-runtime
# FROM pytorch/pytorch:1.10.0-cuda11.3-cudnn8-runtime

# 设置工作目录
WORKDIR /workspace/AO-MARL

# 安装系统依赖
RUN apt-get update && \
    apt-get install -y \
    wget \
    && rm -rf /var/lib/apt/lists/*
# 安装 Python 依赖
RUN pip install --no-cache-dir -i https://mirrors.tuna.tsinghua.edu.cn/pypi/web/simple \
    tensorboardX \
    matplotlib \
    scipy \
    gym \
    pandas\
    h5py \
    astropy

# 下载并安装 compass
RUN wget "https://anaconda.org/compass/compass/5.0.0/download/linux-64/compass-5.0.0-py3.7_cuda102.tar.bz2" && \
    conda install -y ./compass-5.0.0-py3.7_cuda102.tar.bz2 && \
    rm ./compass-5.0.0-py3.7_cuda102.tar.bz2

# 复制代码到容器中
# COPY . .

# 设置默认命令
CMD ["bash"]
