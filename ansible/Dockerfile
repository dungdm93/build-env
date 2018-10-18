# Source from https://github.com/William-Yeh/docker-ansible/blob/master/ubuntu16.04/Dockerfile
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
    apt-get install -y software-properties-common python-pip \
            openssh-client sshpass gettext curl vim; \
    pip install -U --no-cache-dir pip; \
    \
    echo "===> Clean up..."; \
    apt-get clean; \
    rm -rf /var/lib/apt/lists/*

RUN set -eux; \
    echo "===> Installing Ansible..."; \
    add-apt-repository -y ppa:ansible/ansible; \
    apt-get update; \
    apt-get install -y ansible; \
    \
    echo "===> Installing Apache Libcloud..."; \
    pip install -U --no-cache-dir Jinja2 apache-libcloud; \
    \
    echo "===> Clean up..."; \
    apt-get clean; \
    rm -rf /var/lib/apt/lists/*

COPY contrib /etc/ansible/contrib
ENV  ANSIBLE_HOST_KEY_CHECKING=False                            \
     ANSIBLE_VAULT=/etc/ansible/contrib/vault                   \
     ANSIBLE_DYNAMIC_INVENTORY=/etc/ansible/contrib/inventory

# default command: display Ansible version
CMD [ "ansible-playbook", "--version" ]

####### INSTRUCTIONS #######
# Build image:
# $ docker build -t dungdm93/ansible:ubuntu16.04 .
# Run image:
# $ docker run -it --rm --name ansible dungdm93/ansible:ubuntu16.04 sh
############################
