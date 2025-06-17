FROM nvidia/cuda:12.8.1-runtime-ubuntu24.04

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
        python3.12  \
        python3-pip  \
        python3-venv  \
        python3-dev  \
        build-essential \
        git && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* && \
    python3 -m venv /opt/venv && \
    chown -R 1000:1000 /opt/venv


ENV PATH="/opt/venv/bin:$PATH"

RUN --mount=type=cache,target=/root/.cache/pip \
    pip3 install --no-cache-dir --upgrade pip setuptools wheel && \
    pip3 install --pre torch torchvision torchaudio --index-url https://download.pytorch.org/whl/nightly/cu128


# Set working directory
WORKDIR /app

# Clone the repository
RUN --mount=type=cache,target=/root/.cache/pip \
    git clone https://github.com/comfyanonymous/ComfyUI.git . && \
    pip3 install -r requirements.txt


RUN chown -R 1000:1000 /app

# Switch to non-root user
USER ubuntu
ENTRYPOINT ["python3", "main.py", "--port", "8080", "--listen"]
