# Source from https://hub.docker.com/r/wernight/kubectl/
FROM  alpine:3.6
LABEL maintainer="Dũng Đặng Minh <dungdm93@gmail.com>"

RUN  apk add --update --no-cache bash curl openssl ca-certificates gettext

# Install kubectl
# Note: Kubernetes version may be found on:
#    https://github.com/kubernetes/kubernetes/releases
ARG  KUBE_VERSION=v1.6.6
ADD  https://storage.googleapis.com/kubernetes-release/release/${KUBE_VERSION}/bin/linux/amd64/kubectl /usr/local/bin/kubectl
COPY kube_authen /usr/local/bin/

RUN chmod +x    /usr/local/bin/kubectl        && \
    chmod +x    /usr/local/bin/kube_authen    && \
    echo "source <(kubectl completion bash)" >> /etc/profile.d/kubectl_autocomplete.sh && \
    # Basic check it works.
    kubectl version --client

CMD ["/bin/bash"]

####### INSTRUCTIONS #######
# Build image:
# $ docker build -t dungdm93/kubectl:1.6-alpine .
# Run image:
# $ docker run -it --rm --name kubectl dungdm93/kubectl:1.6-alpine bash
############################
