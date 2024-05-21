FROM osrf/ros:iron-desktop-full

ARG PASSWORD=1
ARG USERNAME=container
ARG USER_UID=1000
ARG USER_GID=1000
ARG ZSH_CUSTOM=/home/${USERNAME}/.oh-my-zsh/custom

COPY sources.list /etc/apt/
COPY ros2-latest.list /etc/apt/sources.list.d/
COPY .zshrc_d /home/${USERNAME}/

RUN apt-get update && apt-get install -y --no-install-recommends \
    sudo busybox curl wget zsh git tmux vim clangd clang-format \
    python-is-python3 python3-pip

RUN groupadd --gid ${USER_GID} ${USERNAME} \
    && useradd --uid ${USER_UID} --gid ${USER_GID} -m ${USERNAME} \
    && chown -R $USERNAME:$USERNAME /home/$USERNAME \
    && echo "${USERNAME} ALL=(ALL) ALL" > /etc/sudoers.d/${USERNAME} \
    && chmod 0440 /etc/sudoers.d/${USERNAME} \
    && echo "${USERNAME}:${PASSWORD}" | chpasswd

USER ${USERNAME}

## zsh and oh-my-zsh and plugins
RUN git clone https://mirrors.tuna.tsinghua.edu.cn/git/ohmyzsh.git /home/${USERNAME}/tmp \
    && REMOTE=https://mirrors.tuna.tsinghua.edu.cn/git/ohmyzsh.git bash /home/${USERNAME}/tmp/tools/install.sh \
    && rm -r /home/${USERNAME}/tmp \
    ## && chsh -s $(which zsh) \
    && git clone --depth=1 https://github.com/zsh-users/zsh-autosuggestions $ZSH_CUSTOM/plugins/zsh-autosuggestions \
    && git clone --depth=1 https://github.com/zsh-users/zsh-syntax-highlighting.git $ZSH_CUSTOM/plugins/zsh-syntax-highlighting \
    && sed -i 's/plugins=(git)/plugins=(git zsh-syntax-highlighting zsh-autosuggestions)/' ~/.zshrc \
    && echo 'source ~/.zshrc_d' >> ~/.zshrc

# pip3
RUN pip3 config set global.index-url https://mirrors.cernet.edu.cn/pypi/web/simple

SHELL ["/bin/zsh", "-c"]
