ARG DEBIAN_VERSION=bookworm

FROM rust:$DEBIAN_VERSION

ENV DEBIAN_FRONTEND=noninteractive

COPY ./etc/apt/sources.list.d/llmv.sources /etc/apt/sources.list.d/llvm.sources

RUN wget -q https://apt.llvm.org/llvm-snapshot.gpg.key -O /etc/apt/trusted.gpg.d/llvm-archive-keyring.asc

RUN apt-get update && apt-get upgrade -y && \
    apt-get purge -y imagemagick imagemagick-6-common

RUN apt-get install -y \
      apt-utils \
      build-essential \
      curl \
      gcc-aarch64-linux-gnu \
      g++-aarch64-linux-gnu \
      git \
      gnupg \
      libz-dev \
      wget \
      zip

ARG LLVM_VERSION=16

RUN apt-get install -y \
	    clang-$LLVM_VERSION \
	    clangd-$LLVM_VERSION \
      liblld-$LLVM_VERSION-dev \
      libpolly-$LLVM_VERSION-dev \
      lld-$LLVM_VERSION \
      lldb-$LLVM_VERSION \
	    llvm-$LLVM_VERSION-dev

RUN apt-get clean

RUN rustup component add clippy llvm-tools-preview rust-analyzer

WORKDIR /build
ENTRYPOINT ["/bin/sh"]
CMD ["-c", "trap : TERM INT; (while true; do sleep 1000; done) & wait"]

ENV DEBIAN_FRONTEND=dialog
