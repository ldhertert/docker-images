FROM ubuntu:22.04 as slim

LABEL maintainer="Luke Hertert"

SHELL ["/bin/bash", "-exo", "pipefail", "-c"]

ENV DEBIAN_FRONTEND=noninteractive \
    TERM=dumb \
    PAGER=cat

# Configure environment
RUN echo 'APT::Get::Assume-Yes "true";' > /etc/apt/apt.conf.d/90harness && \
    echo 'DPkg::Options "--force-confnew";' >> /etc/apt/apt.conf.d/90harness && \
    apt-get update && \
    apt-get install -y --no-install-recommends  \
        locales \
        sudo \
        build-essential \
        ca-certificates \
        curl \
        wget && \
        rm -rf /var/lib/apt/lists/*

RUN locale-gen en_US.UTF-8 && \
    useradd --uid=1000 --user-group --create-home harness && \
    echo 'harness ALL=NOPASSWD: ALL' >> /etc/sudoers.d/50-harness && \
    echo 'Defaults    env_keep += "DEBIAN_FRONTEND"' >> /etc/sudoers.d/env_keep && \
    sudo -u harness mkdir /home/harness/.bin

ENV PATH=/home/harness/.bin:$PATH \
    LANG=en_US.UTF-8 \
    LANGUAGE=en_US:en \
    LC_ALL=en_US.UTF-8

USER harness

WORKDIR /home/harness

FROM slim

RUN sudo apt-get update && \
    sudo apt-get install --no-install-recommends \
    gzip \
    tar \
    zip \
    unzip \
    jq \
    tzdata \
    nano \
    net-tools \
    git \
    software-properties-common && \
    sudo rm -rf /var/lib/apt/lists/*

    #### Leaving things out for now until I find a need
    # autoconf \
    # cmake \
    # make \
    # netcat \
    # gnupg \
    # libcurl4-openssl-dev \
    # libssl-dev \
    # libsqlite3-dev \
    # openssh-client \
    # parallel \
    # pkg-config \
    # postgresql-client \
    # shellcheck \
    #  \
    # vim \

# install powershell
RUN wget -O /tmp/powershell.deb https://github.com/PowerShell/PowerShell/releases/download/v7.2.5/powershell-lts_7.2.5-1.deb_amd64.deb && \
    sudo dpkg -i /tmp/powershell.deb && \
    sudo apt-get install -f && \
    rm /tmp/powershell.deb

RUN wget -O ${HOME}/.bin/yq "https://github.com/mikefarah/yq/releases/download/v4.27.2/yq_linux_amd64" && \
    chmod +x $HOME/.bin/yq

RUN wget -O $HOME/.bin/http "https://github.com/httpie/httpie/releases/download/3.2.1/http" && \
    chmod +x $HOME/.bin/http

RUN wget -O $HOME/.bin/kubectl "https://dl.k8s.io/release/v1.24.0/bin/linux/amd64/kubectl" && \
    chmod +x $HOME/.bin/kubectl

# Install helm
RUN wget -qO- "https://get.helm.sh/helm-v3.9.2-linux-amd64.tar.gz" | tar --strip=1 -C ${HOME}/.bin -zxvf - "linux-amd64/helm"

# Install github cli
RUN wget -qO- "https://github.com/cli/cli/releases/download/v2.14.3/gh_2.14.3_linux_amd64.tar.gz" | tar --strip=2 --wildcards -C ${HOME}/.bin -zxvf - "*/*/gh"

# install go-template
RUN wget -O ${HOME}/.bin/go-template "https://app.harness.io/public/shared/tools/go-template/release/v0.4/bin/linux/amd64/go-template"  && \
    chmod +x ${HOME}/.bin/go-template 

# Install golang
RUN wget -qO- "https://go.dev/dl/go1.19.linux-amd64.tar.gz" | tar -C ${HOME}/.bin -zxv 
ENV PATH=$PATH:/usr/local/go/bin
