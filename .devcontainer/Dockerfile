FROM ubuntu:latest AS base

RUN dependUtils="git wget curl zsh" \
    && apt-get update && export DEBIAN_FRONTEND=noninteractive && apt-get install -y $dependUtils \
    # install ohmyzsh
    && sh -c "$(wget -O- https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)" \
    # install zsh plugins
    && git clone https://github.com/zsh-users/zsh-autosuggestions $HOME/.oh-my-zsh/custom/plugins/zsh-autosuggestions \
    && git clone https://github.com/zsh-users/zsh-syntax-highlighting.git $HOME/.oh-my-zsh/custom/plugins/zsh-syntax-highlighting \
    # clear
    && apt-get autoclean && rm -rf /var/lib/apt/lists/*

COPY .zshrc /root

######################
### rust dev image ###
######################
FROM base AS rust-dev

RUN dependUtils="build-essential" \
    && apt-get update && export DEBIAN_FRONTEND=noninteractive && apt-get install -y $dependUtils \
    && apt-get autoclean && rm -rf /var/lib/apt/lists/*

# install rustup
RUN curl --proto '=https' --tlsv1.2 https://sh.rustup.rs -sSf > rust_installer.sh \
    && sh rust_installer.sh -y \
    && \. "$HOME/.cargo/env" \
    && rm rust_installer.sh \
    && rustup component add rustfmt \
    && cargo install mdbook

ENTRYPOINT ["zsh"]

