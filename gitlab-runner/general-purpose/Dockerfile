ARG TF_VERSION=1.7.5
ARG GITLAB_RUNNER_IMAGE_TYPE=gitlab-runner
ARG GITLAB_RUNNER_IMAGE_TAG=alpine-v16.1.0

FROM alpine:latest as terraform

ARG TF_VERSION
ARG HASHI_CORP_PRODUCT=terraform

RUN apk add --update --virtual .deps --no-cache gnupg && \
    cd /tmp && \
    ARCH=$(uname -m) \
    && case "$ARCH" in \
    x86_64) PLATFORM="linux_amd64";; \
    *) echo "Unsupported architecture"; exit 1;; \
    esac && \
    wget https://releases.hashicorp.com/${HASHI_CORP_PRODUCT}/${TF_VERSION}/${HASHI_CORP_PRODUCT}_${TF_VERSION}_${PLATFORM}.zip && \
    wget https://releases.hashicorp.com/${HASHI_CORP_PRODUCT}/${TF_VERSION}/${HASHI_CORP_PRODUCT}_${TF_VERSION}_SHA256SUMS && \
    wget https://releases.hashicorp.com/${HASHI_CORP_PRODUCT}/${TF_VERSION}/${HASHI_CORP_PRODUCT}_${TF_VERSION}_SHA256SUMS.sig && \
    wget -qO- https://www.hashicorp.com/.well-known/pgp-key.txt | gpg --import && \
    gpg --verify ${HASHI_CORP_PRODUCT}_${TF_VERSION}_SHA256SUMS.sig ${HASHI_CORP_PRODUCT}_${TF_VERSION}_SHA256SUMS && \
    grep ${HASHI_CORP_PRODUCT}_${TF_VERSION}_${PLATFORM}.zip ${HASHI_CORP_PRODUCT}_${TF_VERSION}_SHA256SUMS | sha256sum -c && \
    unzip /tmp/${HASHI_CORP_PRODUCT}_${TF_VERSION}_${PLATFORM}.zip -d /tmp && \
    mv /tmp/${HASHI_CORP_PRODUCT} /usr/local/bin/${HASHI_CORP_PRODUCT} && \
    rm -f /tmp/${HASHI_CORP_PRODUCT}_${TF_VERSION}_${PLATFORM}.zip ${HASHI_CORP_PRODUCT}_${TF_VERSION}_SHA256SUMS ${TF_VERSION}/${HASHI_CORP_PRODUCT}_${TF_VERSION}_SHA256SUMS.sig && \
    apk del .deps

FROM gitlab/${GITLAB_RUNNER_IMAGE_TYPE}:${GITLAB_RUNNER_IMAGE_TAG}

RUN apk update --no-cache && apk upgrade --no-cache && rm -rf /var/cache/apk/*

# Install Ansible
RUN apk add ansible

COPY --from=terraform /usr/local/bin/terraform /usr/local/bin/terraform