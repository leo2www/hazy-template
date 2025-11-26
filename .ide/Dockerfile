FROM nvidia/cuda:13.0.2-devel-ubuntu24.04

# 基础工具
RUN apt-get update && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y \
        git \
        wget \
        curl \
        vim \
        openssh-server \
        unzip && \
    rm -rf /var/lib/apt/lists/*

# 安装 code-server（IDE 环境需要）
RUN curl -fsSL https://code-server.dev/install.sh | sh

# 设置字符集（保证中文显示正常）
ENV LANG C.UTF-8
ENV LANGUAGE C.UTF-8

# 默认工作目录
WORKDIR /workspace