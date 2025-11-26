FROM nvidia/cuda:13.0.2-devel-ubuntu24.04

# 基础工具
RUN apt-get update && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y \
        git \
        wget \
        curl \
        vim \
        openssh-server \
        zsh \
        unzip \
        locales && \
    rm -rf /var/lib/apt/lists/*


# 设置字符集（保证中文显示正常）
RUN locale-gen C.UTF-8
ENV LANG C.UTF-8
ENV LANGUAGE C.UTF-8
ENV LC_ALL=C.UTF-8


# 安装 code-server（IDE 环境需要）
RUN curl -fsSL https://code-server.dev/install.sh | sh \
    && code-server --install-extension cnbcool.cnb-welcome \
    && code-server --install-extension redhat.vscode-yaml \
    && code-server --install-extension dbaeumer.vscode-eslint \
    && code-server --install-extension waderyan.gitblame \
    && code-server --install-extension mhutchie.git-graph \
    && code-server --install-extension donjayamanne.githistory \
    && code-server --install-extension ms-python.python \
    && code-server --install-extension ms-python.vscode-python-envs \
    && code-server --install-extension ms-python.debugpy \
    && code-server --install-extension ms-toolsai.jupyter \
    && code-server --install-extension ms-toolsai.vscode-jupyter-cell-tags \
    && code-server --install-extension ms-toolsai.jupyter-keymap \
    && code-server --install-extension ms-toolsai.jupyter-renderers \
    && echo done

# Python
ARG PYTHON_VERSION=3.13
ENV PYTHON_VERSION=${PYTHON_VERSION}

# install uv
RUN  curl -LsSf https://astral.sh/uv/install.sh | sh && \
    echo 'export PATH="$HOME/.local/bin:$PATH"' >> /etc/zsh/zshenv && \
    echo 'export PATH="$HOME/.local/bin:$PATH"' >> /etc/profile && \
    echo 'export PATH="$HOME/.local/bin:$PATH"' >> /root/.zshrc
# 预装 Python 3.13
RUN /root/.local/bin/uv python install  ${PYTHON_VERSION}
# 设置全局默认 Python 不依赖 bash
RUN PY=$( /root/.local/bin/uv python find ${PYTHON_VERSION} ) && \
    ln -sf $PY /usr/local/bin/python && \
    ln -sf /usr/local/bin/python /usr/local/bin/python3


# 安装 Oh My Zsh（无人机模式）
RUN sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)" "" --unattended

# 使用 Oh My Zsh 的默认主题（robbyrussell）
# 注：默认就是 robbyrussell，这里只是确保写入
RUN sed -i 's/ZSH_THEME=".*"/ZSH_THEME="robbyrussell"/g' /root/.zshrc


# 安装 zsh 增强插件：自动补全 + 语法高亮
RUN git clone --depth=1 https://github.com/zsh-users/zsh-autosuggestions \
      /root/.oh-my-zsh/custom/plugins/zsh-autosuggestions && \
    git clone --depth=1 https://github.com/zsh-users/zsh-syntax-highlighting \
      /root/.oh-my-zsh/custom/plugins/zsh-syntax-highlighting

# 启用插件（默认: git → git zsh-autosuggestions zsh-syntax-highlighting）
RUN sed -i 's/plugins=(git)/plugins=(git zsh-autosuggestions zsh-syntax-highlighting)/g' /root/.zshrc

# 默认 shell 设置为 zsh
RUN chsh -s /usr/bin/zsh

# 默认工作目录
WORKDIR /workspace