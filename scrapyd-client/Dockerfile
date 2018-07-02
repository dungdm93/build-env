FROM  ubuntu:16.04
LABEL maintainer="Dũng Đặng Minh <dungdm93@gmail.com>"

ARG DEBIAN_FRONTEND=noninteractive
RUN { \
        echo 'APT::Install-Suggests   "false";';   \
        echo 'APT::Install-Recommends "false";';   \
    } > /etc/apt/apt.conf.d/60user

RUN set -eux; \
    apt-get update; \
    echo "===> Installing additional packages..."; \
    \
    apt-get install -y software-properties-common build-essential \
            python-dev python-pip gettext vim curl; \
    pip install -U --no-cache-dir pip setuptools; \
    \
    echo "===> Clean up..."; \
    apt-get clean; \
    rm -rf /var/lib/apt/lists/*

RUN pip install --no-cache-dir scrapyd-client

####### INSTRUCTIONS #######
# Build image:
# $ docker build -t dungdm93/scrapyd-client:ubuntu16.04 .
############################
