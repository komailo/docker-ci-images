ARG ALPINE_VERSION=3
ARG ANSIBLE_LINT_VERSION=6.22.0-r1
ARG ANSIBLE_VERSION=8.6.1-r0
ARG GITLAB_RUNNER_IMAGE_TAG=alpine-v16.1.0
ARG GITLAB_RUNNER_IMAGE_TYPE=gitlab-runner
ARG JQ_VERSION=1.7.1-r0
ARG TF_VERSION=1.7.5
ARG OPENSSH_VERSION=9.6_p1-r0

FROM alpine:${ALPINE_VERSION} as terraform

ARG TF_VERSION
ARG HASHI_CORP_PRODUCT=terraform

RUN apk add --update --virtual .deps --no-cache gnupg && \
    cd /tmp && \
    ARCH=$(uname -m) \
    && case "$ARCH" in \
    x86_64) PLATFORM="linux_amd64";; \
    aarch64) PLATFORM="linux_arm64";; \
    *) echo "** Unsupported architecture: $ARCH"; exit 1;; \
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

FROM alpine:${ALPINE_VERSION}

ARG ANSIBLE_LINT_VERSION
ARG ANSIBLE_VERSION
ARG JQ_VERSION
ARG OPENSSH_VERSION

RUN apk update --no-cache && apk upgrade --no-cache && rm -rf /var/cache/apk/*

# Install Ansible
RUN apk add --no-cache \
    ansible-lint=${ANSIBLE_LINT_VERSION} \
    ansible=${ANSIBLE_VERSION} \
    jq=${JQ_VERSION} \
    openssh=${OPENSSH_VERSION}

COPY --from=terraform /usr/local/bin/terraform /usr/local/bin/terraform