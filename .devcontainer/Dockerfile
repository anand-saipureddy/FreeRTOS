#
# Copyright © 2025 pyjamabrah.com. All Rights Reserved.
# Author: Piyush Itankar <piyush@pyjamabrah.com>
#

FROM mcr.microsoft.com/vscode/devcontainers/base:ubuntu

ENV DEBIAN_FRONTEND=noninteractive

RUN sudo apt-get update
RUN sudo apt-get install --no-install-recommends -y \
  sudo \
  binutils-common \
  make \
  git-core \
  wget \
  tree \
  build-essential \
  python3 \
  python3-dev \
  python3-pip \
  binutils \
  make \
  qemu-system-arm \
  gcc-arm-none-eabi \
  gdb-multiarch \
  libnewlib-arm-none-eabi

# Fix locales
RUN locale-gen en_US.UTF-8 && update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8 && \
    echo 'dash dash/sh boolean false' | debconf-set-selections && \
    DEBIAN_FRONTEND=noninteractive dpkg-reconfigure dash

# Add user and group anandje with the same id as the user
# defined in the laptop.
ARG USER_ID=1000
ARG GROUP_ID=1000

# Remove any existing user/group with the same UID/GID (usually 'ubuntu')
RUN if getent passwd ${USER_ID} > /dev/null; then \
        userdel -r $(getent passwd ${USER_ID} | cut -d: -f1); \
    fi && \
    if getent group ${GROUP_ID} > /dev/null; then \
        groupdel $(getent group ${GROUP_ID} | cut -d: -f1); \
    fi && \
    groupadd -g ${GROUP_ID} anandje && \
    useradd -m -u ${USER_ID} -g ${GROUP_ID} -s /bin/bash anandje && \
    usermod -aG sudo anandje && \
    echo 'anandje ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers

# Change the user from root to anandje
USER anandje

# Change the workdir
WORKDIR /home/anandje

# Install Rust and Install git-delta as required by vscode
RUN curl https://sh.rustup.rs -sSf | sh -s -- -y
RUN echo 'source $HOME/.cargo/env' >> $HOME/.bashrc
RUN /home/anandje/.cargo/bin/cargo install git-delta

# terminal colors with xterm
ENV TERM=xterm