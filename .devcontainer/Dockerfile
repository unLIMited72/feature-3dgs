FROM nvidia/cuda:11.8.0-devel-ubuntu22.04

WORKDIR /workspace
ENV DEBIAN_FRONTEND=noninteractive
ENV TORCH_CUDA_ARCH_LIST="7.5"

RUN apt-get update && apt-get install -y \
    software-properties-common \
    tzdata \
    pkg-config libcairo2-dev \
    && ln -fs /usr/share/zoneinfo/America/Vancouver /etc/localtime \
    && dpkg-reconfigure --frontend noninteractive tzdata \
    && add-apt-repository ppa:deadsnakes/ppa -y \
    && apt-get update && apt-get install -y \
    python3.8 python3.8-dev python3-pip python3.8-venv \
    git wget curl unzip build-essential \
    cmake ninja-build \
    libgl1-mesa-glx libglfw3 libglew-dev libassimp-dev libgtk-3-dev \
    libopencv-dev libavdevice-dev libavcodec-dev libavformat-dev \
    libeigen3-dev libxxf86vm-dev libx11-dev libxrandr-dev \
    libxinerama-dev libxcursor-dev libxi-dev \
    libssl-dev libboost-all-dev libglib2.0-0 \
    x11-xserver-utils \
    && apt-get clean && rm -rf /var/lib/apt/lists/*

RUN update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.8 1

RUN pip3 install --upgrade pip

RUN pip3 install torch==2.4.0 torchvision==0.19.0 torchaudio==2.4.0 --index-url https://download.pytorch.org/whl/cu118

COPY requirements.txt .
RUN pip install -r requirements.txt

COPY submodules ./submodules

RUN pip3 install ./submodules/diff-gaussian-rasterization-feature 
RUN pip3 install ./submodules/simple-knn 
RUN pip3 install lpips pycairo

RUN git clone https://github.com/zhanghang1989/PyTorch-Encoding.git /workspace/submodules/torch-encoding \
 && pip3 install ./submodules/torch-encoding --no-use-pep517

CMD ["/bin/bash"]
