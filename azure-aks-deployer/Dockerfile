FROM alpine as build

ENV KUBE_VERSION="1.19.9"
ENV KUSTOMIZE_VERSION="3.0.0"

# Install kubectl
RUN apk add --update ca-certificates && \
    apk add --update -t deps curl && \
    curl -L https://storage.googleapis.com/kubernetes-release/release/v$KUBE_VERSION/bin/linux/amd64/kubectl -o /usr/local/bin/kubectl && \
    chmod +x /usr/local/bin/kubectl

# Install Kustomize
RUN curl -L https://github.com/kubernetes-sigs/kustomize/releases/download/v${KUSTOMIZE_VERSION}/kustomize_${KUSTOMIZE_VERSION}_linux_amd64  -o /usr/bin/kustomize \
    && chmod +x /usr/bin/kustomize

# Install Kubeval
RUN wget https://github.com/instrumenta/kubeval/releases/latest/download/kubeval-linux-amd64.tar.gz && \
    tar xf kubeval-linux-amd64.tar.gz && \
    cp kubeval /usr/local/bin

FROM alpine as final

COPY --from=build /usr/local/bin/kubectl /usr/local/bin/kubectl
COPY --from=build /usr/bin/kustomize /usr/local/bin/kustomize
COPY --from=build /usr/local/bin/kubeval /usr/local/bin/kubeval

## Install Azure CLI
RUN apk update && apk add bash py-pip && apk add --virtual=build gcc libffi-dev musl-dev openssl-dev python3-dev make && pip install azure-cli && \
  apk del --purge build 