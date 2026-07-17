FROM alpine:3.23.3 AS prepare

WORKDIR /tools

ENV HELM_VERSION="3.14.2"

RUN apk add --no-cache wget tar \
    # Helm-sops под капотом использует утилиту helm, поэтому её тоже необходимо установить
    && wget -qO- https://get.helm.sh/helm-v${HELM_VERSION}-linux-amd64.tar.gz | tar -xzO linux-amd64/helm > ./_helm \
    && chmod +x ./_helm \
    ## Установка Helm-sops
    && wget https://github.com/camptocamp/helm-sops/releases/download/20260107-1/helm-sops_20260107-1_linux_amd64.tar.gz \
    && tar -xzvf helm-sops_20260107-1_linux_amd64.tar.gz helm-sops \
    && mv helm-sops helm

FROM quay.io/argoproj/argocd:v3.2.5 AS final

# Копируем все ранее установленные утилиты в финальный
# образ с ArgoCD
COPY --from=prepare /tools/helm  /usr/local/bin/helm
COPY --from=prepare /tools/_helm /usr/local/bin/_helm