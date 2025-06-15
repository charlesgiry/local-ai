FROM ubuntu:22.04

# Refer to https://developer.nvidia.com/cuda-gpus
ENV TORCH_CUDA_ARCH_LIST=8.9

# Install dependencies
RUN --mount=type=cache,target=/var/cache/apt,sharing=locked,rw \
    --mount=type=cache,target=/var/lib/apt/lists \
    apt-get update && \
    apt-get upgrade -y && \
    apt-get install --no-install-recommends -y  \
      git  \
      build-essential  \
      python3-dev  \
      pip  \
      bash  \
      curl  \
      vim && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Clone project
WORKDIR /app/
RUN git clone https://github.com/oobabooga/text-generation-webui.git . && \
    GPU_CHOICE=A LAUNCH_AFTER_INSTALL=FALSE INSTALL_EXTENSIONS=TRUE ./start_linux.sh --verbose

ENTRYPOINT ["./start_linux.sh", "--listen"]
