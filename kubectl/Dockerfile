FROM  alpine:3.9
LABEL maintainer="Dũng Đặng Minh <dungdm93@gmail.com>"

RUN  apk add --update --no-cache curl openssl ca-certificates bash bash-completion gettext

# Install kubectl
# Note: Kubernetes version may be found on:
#    https://github.com/kubernetes/kubernetes/releases
ENV KUBE_VERSION=1.14.1
RUN set -eux; \
    curl -L -o /usr/local/bin/kubectl \
        https://storage.googleapis.com/kubernetes-release/release/v${KUBE_VERSION}/bin/linux/amd64/kubectl; \
    chmod +x /usr/local/bin/kubectl; \
    echo 'source <(kubectl completion bash)' >> /etc/profile.d/autocomplete.sh; \
    # Basic check it works.
    kubectl version --client
COPY kube_authen /usr/local/bin/

CMD ["/bin/bash", "-l"]

####### INSTRUCTIONS #######
# Build image:
# $ docker build -t dungdm93/kubectl:alpine -t dungdm93/kubectl:1.14-alpine .
# Run image:
# $ docker run -it --rm --name kubectl dungdm93/kubectl:alpine
############################
