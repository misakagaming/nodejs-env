#FROM aaii/cuda:9.2-cudnn7-devel-ubuntu18.04
#FROM nvidia/cuda:11.2.2-devel-ubuntu20.04
FROM nvidia/cuda:12.2.0-devel-ubuntu22.04

RUN rm /bin/sh && ln -s /bin/bash /bin/sh


ENV LANG=C.UTF-8 LC_ALL=C.UTF-8

# Python Version x.y
ARG PYTHON_VERSION=3.8
ARG PYLUCENE_VERSION=7.4.0
ENV NODE_VERSION=20.10.0

RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
    && apt-get -y install --no-install-recommends \
        git \
        wget \
        cmake \
        ninja-build \
        build-essential \
		checkinstall \
		software-properties-common \
		dpkg \
    && apt-get autoremove -y && apt-get clean -y && rm -rf /var/lib/apt/lists/* 
	
RUN add-apt-repository ppa:deadsnakes/ppa

#RUN export DEBIAN_FRONTEND=noninteractive && apt-get --purge remove "*cublas*" "cuda*" "nsight*" 

#RUN rm -rf /usr/local/cuda*

#COPY cuda-repo-ubuntu1604_9.2.148-1_amd64.deb .
#COPY 7fa2af80.pub .


#RUN wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/cuda-ubuntu1604.pin
#RUN mv cuda-ubuntu1604.pin /etc/apt/preferences.d/cuda-repository-pin-600
#RUN apt-key add 7fa2af80.pub
#RUN dpkg -i cuda-repo-ubuntu1604_9.2.148-1_amd64.deb
#RUN apt-get update
#RUN apt-get -y install cuda --allow-unauthenticated

RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
    && apt-get -y install --no-install-recommends \
		python2.7 \
        "python$PYTHON_VERSION-dev" \
		"python$PYTHON_VERSION-distutils" \
		python3-setuptools \
        python3-pip \
        "python$PYTHON_VERSION-venv" \
		libstdc++6 \
		gcc-multilib\
		libc6 \
		nodejs \
		npm \
		 && apt-get autoremove -y && apt-get clean -y && rm -rf /var/lib/apt/lists/* 

RUN ls /usr/bin | grep "python"
RUN python3 -m pip install --user virtualenv
RUN python3 -m virtualenv --help



#RUN python3.6 -m pip install --upgrade Pillow




# ======================== START OF ADDITIONAL INSTALLATION ========================

# Install Java
RUN apt-get update && \
	apt-get install -y --no-install-recommends \
    build-essential \
    ant \
    curl \
    git \
    default-jdk
  
RUN node -v

RUN curl https://raw.githubusercontent.com/creationix/nvm/master/install.sh | bash

RUN source ~/.bashrc

ENV NVM_DIR=/root/.nvm
RUN . "$NVM_DIR/nvm.sh" && nvm install ${NODE_VERSION}
RUN . "$NVM_DIR/nvm.sh" && nvm use v${NODE_VERSION}
RUN . "$NVM_DIR/nvm.sh" && nvm alias default v${NODE_VERSION}
ENV PATH="/root/.nvm/versions/node/v${NODE_VERSION}/bin/:${PATH}"
RUN node --version
RUN npm --version

RUN npm install -g tree-sitter-cli@0.24.7


RUN ln -sf $(which python$PYTHON_VERSION) /usr/bin/python

#RUN python --version

RUN python -m pip install --upgrade pip

#RUN which python$PYTHON_VERSION && which python && python --version

WORKDIR /usr/lib/jvm/default-java/jre/lib
RUN ln -s ../../lib amd64



WORKDIR /app
COPY . .

WORKDIR /



RUN python -m pip install --upgrade pip \
    && python -m venv --system-site-packages /opt/python/venv/base
	
COPY requirements.txt /opt/python/venv/base/

RUN python2.7 --version

RUN wget https://bootstrap.pypa.io/pip/2.7/get-pip.py
RUN python2.7 get-pip.py

RUN rm get-pip.py

RUN python2.7 -m pip install -U virtualenv

RUN python2.7 -m virtualenv --system-site-packages temp-python

RUN ls temp-python/bin
	
#RUN /opt/python/venv/base/bin/python -m pip install -r /opt/python/venv/base/requirements.txt




COPY parsers .
COPY requirements.txt .


COPY entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh

# Set entrypoint to bash
ENTRYPOINT ["/entrypoint.sh"]