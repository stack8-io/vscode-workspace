FROM debian:bullseye

# essentials
RUN apt-get update && \
    apt-get install -y vim git curl openssl locales

# zsh
ENV TERM xterm
ENV ZSH_THEME pure
RUN apt-get install -y zsh && \
    sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)" && \
    echo "export HISTFILE=/cache/.zsh_history" >> "/root/.zshrc" && \
    chsh -s $(which zsh)

# locale
RUN localedef -f UTF-8 -i en_US en_US.utf8
ENV LC_ALL=en_US.UTF-8

# git config
RUN git config --global credential.helper "store --file /cache/.git-credentials"

# Docker
RUN curl -fsSL https://get.docker.com -o get-docker.sh && \
    sh get-docker.sh

# Node.js
RUN curl -fsSL https://deb.nodesource.com/setup_lts.x | bash - && \
    apt-get install -y nodejs && \
    npm config set cache /cache/nodejs/.npm --global

# Pulumi
RUN curl -fsSL https://get.pulumi.com | sh
ENV PATH=$PATH:/root/.pulumi/bin

# gcloud CLI
RUN apt-get install -y apt-transport-https ca-certificates gnupg && \
    echo "deb [signed-by=/usr/share/keyrings/cloud.google.gpg] https://packages.cloud.google.com/apt cloud-sdk main" | tee -a /etc/apt/sources.list.d/google-cloud-sdk.list && \
    curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key --keyring /usr/share/keyrings/cloud.google.gpg add - && \
    apt-get update && \
    apt-get install -y google-cloud-cli && \
    apt-get install -y google-cloud-sdk-gke-gcloud-auth-plugin && \
    apt-get install -y kubectl

# AWS CLI
RUN apt-get install -y unzip && \
    curl "https://awscli.amazonaws.com/awscli-exe-linux-aarch64.zip" -o "awscliv2.zip" && \
    unzip awscliv2.zip && \
    ./aws/install && \
    rm -rf aws awscliv2.zip

# lazygit
RUN LAZYGIT_VERSION="$(curl -s https://api.github.com/repos/jesseduffield/lazygit/releases/latest | grep -Po '(?<=tag_name[^0-9.]{5})[0-9.]+')" && \
    curl -Lo lazygit.tar.gz "https://github.com/jesseduffield/lazygit/releases/latest/download/lazygit_${LAZYGIT_VERSION}_Linux_x86_64.tar.gz" && \
    tar xf lazygit.tar.gz -C /usr/local/bin lazygit
