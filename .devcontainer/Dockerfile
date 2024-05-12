FROM osrf/ros:iron-desktop-full

ARG ZSH_CUSTOM=/root/.oh-my-zsh/custom

COPY sources.list /etc/apt/
COPY ros2-latest.list /etc/apt/sources.list.d/
COPY .zshrc_d /root/

RUN apt-get update && apt-get install -y --no-install-recommends \
    busybox curl wget zsh git tmux vim clangd clang-format python-is-python3 python3-pip && \
    ## pip config
    pip3 config set global.index-url https://mirrors.cernet.edu.cn/pypi/web/simple && \
    ## zsh and oh-my-zsh and plugins
    git clone https://mirrors.cernet.edu.cn/ohmyzsh.git && \
    REMOTE=https://mirrors.cernet.edu.cn/ohmyzsh.git sh ohmyzsh/tools/install.sh --unattended && \
    rm -r ohmyzsh && \
    chsh -s $(which zsh) && \
    git clone --depth=1 https://github.com/zsh-users/zsh-autosuggestions $ZSH_CUSTOM/plugins/zsh-autosuggestions && \
    git clone --depth=1 https://github.com/zsh-users/zsh-syntax-highlighting.git $ZSH_CUSTOM/plugins/zsh-syntax-highlighting && \
    sed -i 's/plugins=(git)/plugins=(git zsh-syntax-highlighting zsh-autosuggestions)/' ~/.zshrc && \
    # zsh config
    echo 'source ~/.zshrc_d' >> /root/.zshrc

SHELL ["/bin/zsh", "-c"]
