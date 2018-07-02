FROM ubuntu:16.04
LABEL maintainer="Dũng Đặng Minh <dungdm93@gmail.com>"

RUN set -eux; \
    apt-get update; \
    # openssh = openssh-client + openssh-server + openssh-sftp-server
    apt-get install --no-install-recommends -y openssh-client openssh-server; \
    # Clean cache at /var/cache/apt/archives
    apt-get clean; \
    rm -rf /var/lib/apt/lists/*;

COPY ssh /root/.ssh

####### INSTRUCTIONS #######
# Build image:
# $ docker build -t dungdm93/openssh:ubuntu16.04 .
# Run image:
# $ docker run -it --rm --name openssh dungdm93/openssh:ubuntu16.04 ssh robots@sample.server
############################
