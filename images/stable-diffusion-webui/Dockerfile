FROM nvidia/cuda:12.6.3-runtime-ubuntu22.04 AS base

# Set environment variables
ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1 \
    DEBIAN_FRONTEND=noninteractive

# Install Python and system dependencies
RUN --mount=type=cache,target=/var/cache/apt \
    --mount=type=cache,target=/var/lib/apt/lists \
    apt-get update && \
    apt-get upgrade -y && \
    apt-get install --no-install-recommends -y \
        python3.10  \
        python3-pip  \
        python3-venv  \
        python3-dev  \
        libgoogle-perftools-dev  \
        libglfw3-dev  \
        libgles2-mesa-dev  \
        ffmpeg  \
        pkg-config  \
        libcairo2  \
        libcairo2-dev  \
        build-essential \
        git && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* && \
    python3 -m venv /opt/venv


ENV PATH="/opt/venv/bin:$PATH"

RUN --mount=type=cache,target=/root/.cache/pip \
    pip3 install --no-cache-dir --upgrade pip setuptools wheel && \
    pip3 install clip pyngrok xformers gfpgan torchvision torchaudio


# Set working directory
WORKDIR /app

# Clone the repository
RUN --mount=type=cache,target=/root/.cache/pip \
    git clone https://github.com/AUTOMATIC1111/stable-diffusion-webui.git . && \
    pip3 install -r requirements_versions.txt


RUN mkdir repositories && \
    cd repositories && \
    git clone https://github.com/AUTOMATIC1111/stable-diffusion-webui-assets.git && \
    git -C stable-diffusion-webui-assets checkout $(sed -n "s/.*ASSETS_COMMIT_HASH', \"\([^\"]*\)\".*/\1/p" ../modules/launch_utils.py) && \
    git clone https://github.com/salesforce/BLIP.git && \
    git -C BLIP checkout $(sed -n "s/.*BLIP_COMMIT_HASH', \"\([^\"]*\)\".*/\1/p" ../modules/launch_utils.py) && \
    git clone https://github.com/Stability-AI/stablediffusion.git stable-diffusion-stability-ai && \
    git -C stable-diffusion-stability-ai checkout $(sed -n "s/.*STABLE_DIFFUSION_COMMIT_HASH', \"\([^\"]*\)\".*/\1/p" ../modules/launch_utils.py) && \
    git clone https://github.com/Stability-AI/generative-models && \
    git -C generative-models checkout $(sed -n "s/.*STABLE_DIFFUSION_XL_COMMIT_HASH', \"\([^\"]*\)\".*/\1/p" ../modules/launch_utils.py) && \
    git clone https://github.com/crowsonkb/k-diffusion.git && \
    git -C k-diffusion checkout $(sed -n "s/.*K_DIFFUSION_COMMIT_HASH', \"\([^\"]*\)\".*/\1/p" ../modules/launch_utils.py) && \
    git clone https://github.com/pharmapsychotic/clip-interrogator



RUN useradd -m -u 1000 user && \
    chown -R user:user /app

# Switch to non-root user
USER user
ENTRYPOINT ["python3", "webui.py", "--listen", "--port", "8080"]