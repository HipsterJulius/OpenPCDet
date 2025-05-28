FROM nvidia/cuda:11.6.2-cudnn8-devel-ubuntu20.04

RUN echo 'debconf debconf/frontend select Noninteractive' | debconf-set-selections

# Install basics
#RUN apt-get update \
#    && apt-get install build-essential \
#    && apt-get install -y apt-utils git curl ca-certificates bzip2 tree htop wget \
#    && apt-get install -y libglib2.0-0 libsm6 libxext6 libxrender-dev bmon iotop g++ python3 python3-dev python3.7-distutils
RUN apt-get update
RUN apt-get install -y build-essential
RUN apt-get install -y apt-utils git curl ca-certificates bzip2 tree htop wget
RUN apt-get install -y libglib2.0-0 libsm6 libxext6 libxrender-dev bmon iotop g++ python3 python3-dev python3-distutils

# Install cmake v3.13.2
RUN apt-get purge -y cmake && \
    mkdir /root/temp && \
    cd /root/temp && \
    wget https://github.com/Kitware/CMake/releases/download/v3.13.2/cmake-3.13.2.tar.gz && \
    tar -xzvf cmake-3.13.2.tar.gz && \
    cd cmake-3.13.2 && \
    bash ./bootstrap && \
    make && \
    make install && \
    cmake --version && \
    rm -rf /root/temp

# Install python
RUN ln -sv /usr/bin/python3.8/usr/bin/python
#RUN wget https://bootstrap.pypa.io/get-pip.py && \
#	python get-pip.py && \
#	rm get-pip.py
RUN wget https://bootstrap.pypa.io/pip/3.8/get-pip.py
RUN python3 get-pip.py
RUN rm get-pip.py

# Install python packages
RUN PIP_INSTALL="python3 -m pip --no-cache-dir install" && \
    $PIP_INSTALL numpy==1.19.3 llvmlite numba 

# Install torch and torchvision
# See https://pytorch.org/ for other options if you use a different version of CUDA
RUN pip install --user torch==1.6 torchvision==0.7.0 -f https://download.pytorch.org/whl/cu102/torch_stable.html

# Install python packages
RUN PIP_INSTALL="python3 -m pip --no-cache-dir install" && \
    $PIP_INSTALL tensorboardX easydict pyyaml scikit-image tqdm SharedArray six

WORKDIR /root

# Install Boost geometry
RUN wget https://jaist.dl.sourceforge.net/project/boost/boost/1.68.0/boost_1_68_0.tar.gz && \
    tar xzvf boost_1_68_0.tar.gz && \
    cp -r ./boost_1_68_0/boost /usr/include && \
    rm -rf ./boost_1_68_0 && \
    rm -rf ./boost_1_68_0.tar.gz 

# A weired problem that hasn't been solved yet
RUN pip uninstall -y SharedArray && \
    pip install SharedArray

RUN pip install spconv-cu102