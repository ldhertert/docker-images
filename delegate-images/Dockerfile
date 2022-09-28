ARG SOURCE_TAG
FROM harness/delegate-immutable:${SOURCE_TAG}

USER root

RUN microdnf update \
    && microdnf install --nodocs \
        jq \
        wget \
        nano \
        zip \
        unzip \
        git \
        podman \
        which

RUN wget -q -O terraform.zip "https://releases.hashicorp.com/terraform/1.3.0/terraform_1.3.0_linux_amd64.zip" && \
    unzip terraform.zip && \
    rm terraform.zip && \
    mv terraform /usr/local/bin/terraform && \
    chmod +x /usr/local/bin/terraform

RUN wget -q -O /usr/local/bin/yq "https://github.com/mikefarah/yq/releases/download/v4.27.2/yq_linux_amd64" && \
    chmod +x /usr/local/bin/yq

RUN wget -q -O /usr/local/bin/http "https://github.com/httpie/httpie/releases/download/3.2.1/http" && \
    chmod +x /usr/local/bin/http

RUN microdnf install yum \
    && yum install -y yum-utils \
    && yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo \
    && yum install -y docker-ce-cli docker-compose-plugin 

RUN rm -rf /var/cache/yum \
        && microdnf clean all
    
USER 1001

# Leave original CMD script as is
# CMD [ "./start.sh" ]
