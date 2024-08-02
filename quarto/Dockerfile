ARG QUARTO_VERSION="1.5.56"

FROM ubuntu

RUN set -e -x && \
    apt-get update --yes && \
    apt-get upgrade --yes && \
    apt-get install --yes --no-install-recommends \
    pandoc \
    curl \
    wget \
    ca-certificates \
    gdebi-core \
    librsvg2-bin \
    perl && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

ARG QUARTO_VERSION

# To generate $ARCH correctly with sed below
SHELL ["bin/bash", "-o", "pipefail", "-c"]

RUN set -e -x && \
    ARCH=$(arch | sed s/aarch64/arm64/ | sed s/x86_64/amd64/) && \
    curl -o quarto-linux-${ARCH}.deb -L https://github.com/quarto-dev/quarto-cli/releases/download/v${QUARTO_VERSION}/quarto-${QUARTO_VERSION}-linux-${ARCH}.deb && \
    gdebi --non-interactive quarto-linux-${ARCH}.deb && \
    rm -f quarto-linux-${ARCH}.deb

RUN set -e -x && \
    wget -qO- https://yihui.org/tinytex/install-bin-unix.sh | sh && \
    rm -f install-bin-unix.sh

VOLUME ["inputs", "outputs"]

WORKDIR /inputs
COPY . /inputs
