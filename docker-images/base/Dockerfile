FROM registry.access.redhat.com/ubi7:latest
ARG JAVA_VERSION=11
ARG TARGETPLATFORM

RUN curl ftp://partners.redhat.com/1c5d859a/IBM-917ca288c24859a81f35b894e0b77589/RHEL-7.4-GA/RHEL-7-server.repo | sed -e "s@3919ee9d338684de51265ffce2111542/RHEL-7.4-RC@IBM-917ca288c24859a81f35b894e0b77589/RHEL-7.9-GA@g" >  /etc/yum.repos.d/rhel7-server.repo
RUN yum -y update \
    && yum -y install java-${JAVA_VERSION}-openjdk-headless openssl bind-utils hostname \
    && yum -y clean all

#####
# Add Tini
#####
ENV TINI_VERSION v0.18.0
ENV TINI_SHA256_AMD64=12d20136605531b09a2c2dac02ccee85e1b874eb322ef6baf7561cd93f93c855
ENV TINI_SHA256_ARM64=7c5463f55393985ee22357d976758aaaecd08defb3c5294d353732018169b019
ENV TINI_SHA256_S390X=c8aaa618ea7897f26979ea10920373e06f3e6dfeb41ef95342eda2eb5672f24d

RUN set -ex; \
    if [[ ${TARGETPLATFORM} = "linux/arm64" ]]; then \
        curl -s -L https://github.com/krallin/tini/releases/download/${TINI_VERSION}/tini-arm64 -o /usr/bin/tini; \
        echo "${TINI_SHA256_ARM64} */usr/bin/tini" | sha256sum -c; \
        chmod +x /usr/bin/tini; \
    elif [[ ${TARGETPLATFORM} = "linux/s390x" ]]; then \
        curl -s -L https://github.com/krallin/tini/releases/download/${TINI_VERSION}/tini-s390x -o /usr/bin/tini; \
        echo "${TINI_SHA256_S390X} */usr/bin/tini" | sha256sum -c; \
        chmod +x /usr/bin/tini; \
    else \
        curl -s -L https://github.com/krallin/tini/releases/download/${TINI_VERSION}/tini -o /usr/bin/tini; \
        echo "${TINI_SHA256_AMD64} */usr/bin/tini" | sha256sum -c; \
        chmod +x /usr/bin/tini; \
    fi
