FROM ubuntu:22.04

LABEL maintainer="Luke Hertert"

SHELL ["/bin/bash", "-exo", "pipefail", "-c"]

ENV DEBIAN_FRONTEND=noninteractive \
    TERM=dumb \
    PAGER=cat

# Configure environment
RUN echo 'APT::Get::Assume-Yes "true";' > /etc/apt/apt.conf.d/90harness && \
    echo 'DPkg::Options "--force-confnew";' >> /etc/apt/apt.conf.d/90harness && \
    apt-get update && apt-get install -y locales sudo && \
    locale-gen en_US.UTF-8 && \
    rm -rf /var/lib/apt/lists/* && \
    useradd --uid=3434 --user-group --create-home harness && \
    echo 'harness ALL=NOPASSWD: ALL' >> /etc/sudoers.d/50-harness && \
    echo 'Defaults    env_keep += "DEBIAN_FRONTEND"' >> /etc/sudoers.d/env_keep && \
    sudo -u harness mkdir /home/harness/project && \
    sudo -u harness mkdir /home/harness/bin

ENV PATH=/home/harness/bin:/home/harness/.local/bin:$PATH \
    LANG=en_US.UTF-8 \
    LANGUAGE=en_US:en \
    LC_ALL=en_US.UTF-8

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    autoconf \
    build-essential \
    ca-certificates \
    cmake \
    curl \
    gnupg \
    gzip \
    jq \
    libcurl4-openssl-dev \
    libssl-dev \
    libsqlite3-dev \
    make \
    nano \
    net-tools \
    netcat \
    openssh-client \
    parallel \
    pkg-config \
    postgresql-client \
    shellcheck \
    software-properties-common \
    tar \
    tzdata \
    unzip \
    vim \
    wget \
    zip \
    git && \
    rm -rf /var/lib/apt/lists/*

# Install docker
RUN apt-get update && \
    apt-get install \
        ca-certificates \
        curl \
        gnupg \
        lsb-release && \
    mkdir -p /etc/apt/keyrings && \
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | gpg --dearmor -o /etc/apt/keyrings/docker.gpg && \
    echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
        $(lsb_release -cs) stable" | tee /etc/apt/sources.list.d/docker.list > /dev/null && \
    apt-get update && \
    apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin && \
    rm -rf /var/lib/apt/lists/*

RUN wget "https://github.com/mikefarah/yq/releases/download/v4.27.2/yq_linux_amd64" -O /usr/local/bin/yq && \
    chmod +x /usr/local/bin/yq

RUN wget -O /usr/local/bin/http "https://github.com/httpie/httpie/releases/download/3.2.1/http" && \
    chmod +x /usr/local/bin/http

RUN wget -O /usr/local/bin/kubectl "https://dl.k8s.io/release/v1.24.0/bin/linux/amd64/kubectl" && \
    chmod +x /usr/local/bin/kubectl

RUN mkdir -p /opt/helm && \
    wget -qO- "https://get.helm.sh/helm-v3.9.2-linux-amd64.tar.gz" | tar xvz -C /opt/helm && \
    ln -s /opt/helm/linux-amd64/helm /usr/local/bin/helm

# install github cli
RUN http -d GET https://github.com/cli/cli/releases/download/v2.14.3/gh_2.14.3_linux_amd64.tar.gz | tar zxf - -C /opt && \
    ln -s /opt/gh_2.14.3_linux_amd64/bin/gh  /usr/local/bin/gh

# install go-template
RUN http -d GET https://app.harness.io/public/shared/tools/go-template/release/v0.4/bin/linux/amd64/go-template -o /usr/local/bin/go-template && \
    chmod +x /usr/local/bin/go-template 

# install powershell
RUN wget https://github.com/PowerShell/PowerShell/releases/download/v7.2.5/powershell-lts_7.2.5-1.deb_amd64.deb && \
    dpkg -i powershell-lts_7.2.5-1.deb_amd64.deb  && \
    apt-get install -f

# Install golang
RUN http -d GET https://go.dev/dl/go1.19.linux-amd64.tar.gz | tar zxf - -C /usr/local
ENV PATH=$PATH:/usr/local/go/bin

USER harness

RUN whoami && \
    # opt-out of the new security feature, not needed in a CI environment
    git config --global --add safe.directory '*'

WORKDIR /home/harness