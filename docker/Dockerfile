FROM nvidia/cuda:12.3.2-base-ubuntu20.04

SHELL ["/bin/bash", "-c"]

ARG USERNAME=user
ARG PASSWORD=docker
ARG MY_UID=1000

# Set environment variables
ENV DEBIAN_FRONTEND=noninteractive

# Install system dependencies
RUN apt-get update && \
    apt-get install -y \
        build-essential \
        git \
        python3-pip \
        python3-dev \
        python3-opencv \
        python3-tk \
        libglib2.0-0 \
        libgraphviz-dev \
        language-pack-en-base \
        libcanberra-gtk-module \
        libcanberra-gtk3-module \
        software-properties-common

# add user
RUN useradd -u ${MY_UID} -m ${USERNAME} -s /bin/bash  && yes ${PASSWORD} | passwd ${USERNAME}
RUN usermod -aG sudo ${USERNAME}

# Install any python packages you need
COPY ./requirements.txt requirements.txt

# Upgrade pip
RUN python3 -m pip install --upgrade pip

# Install PyTorch and torchvision
RUN python3 -m pip install -r requirements.txt
RUN pip3 install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118

USER ${USERNAME}
ENV HOME=/home/user

COPY ./docker/entrypoint.sh /entrypoint.sh
ENTRYPOINT [ "/entrypoint.sh" ]

# Set the working directory
WORKDIR /home/user/code/
