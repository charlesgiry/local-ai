FROM nvidia/cuda:12.6.0-runtime-ubuntu22.04 AS base

# Set environment variables
ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1 \
    PORT=7865 \
    HF_HUB_ENABLE_HF_TRANSFER=1 \
    DEBIAN_FRONTEND=noninteractive \
    ACE_OUTPUT_DIR=/app/outputs

# Install Python and system dependencies
RUN --mount=type=cache,target=/var/cache/apt \
    --mount=type=cache,target=/var/lib/apt/lists \
    apt-get update && \
    apt-get upgrade -y && \
    apt-get install --no-install-recommends -y python3.10 python3-pip python3-venv python3-dev build-essential git curl wget && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* && \
    python3 -m venv /opt/venv


ENV PATH="/opt/venv/bin:$PATH"

RUN --mount=type=cache,target=/root/.cache/pip \
    pip3 install --no-cache-dir --upgrade pip setuptools wheel && \
    pip3 install hf_transfer peft

# Set working directory
WORKDIR /app

# Clone the repository
RUN --mount=type=cache,target=/root/.cache/pip \
    git clone https://github.com/ace-step/ACE-Step.git . && \
    pip3 install -r requirements.txt --extra-index-url https://download.pytorch.org/whl/cu126 && \
    pip3 install .

# Ensure target directories for volumes exist and have correct initial ownership
RUN useradd -m -u 1000 user && \
    mkdir -p /app/outputs && \
    mkdir -p /app/checkpoints && \
    mkdir -p /app/logs && \
    chown -R user:user /app

# Switch to non-root user
USER user

CMD ["python3", "./acestep/gui.py", "--server_name", "0.0.0.0", "--bf16", "true"]
