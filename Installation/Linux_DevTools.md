# Development tools installation

## Option A: Host system installation

1. **Upgrade apt**
    ``` 
    apt update
    apt upgrade -y
    ```

1. **Install first required packages**
    ```shell
    apt install -y  wget \
                    curl \
                    gnupg \
                    gnupg2 \
                    lsb-core \
                    locales
    ```

1. **Add ROS2 repo key**
    ```
    wget http://repo.ros2.org/repos.key 
    apt-key add repos.key 
    rm repos.key 
    sh -c 'echo "deb [arch=amd64,arm64] http://repo.ros2.org/ubuntu/main `lsb_release -cs` main" > /etc/apt/sources.list.d/ros2-latest.list'
    ```

1. **Configure locale**
    ```
    locale-gen en_US en_US.UTF-8 
    update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8 
    export  LANG='en_US.UTF-8' \
            LANGUAGE='en_US:en' \
            LC_ALL='en_US.UTF-8' \
            DEBIAN_FRONTEND=noninteractive
    ```

1. **Install essential required apt packages for ROS2**
    ```bash
    apt update
    apt install -y  build-essential \
                    cmake \
                    git \
                    python3-colcon-common-extensions \
                    python3-pip \
                    python-rosdep \
                    python3-vcstool \
                    clang-tidy
    ```

1. **Install essential required python packages for ROS2**
    ```bash 
    python3 -m pip install --upgrade pip
    python3 -m pip install -U   argcomplete \
                                flake8 \
                                flake8-blind-except \
                                flake8-builtins \
                                flake8-class-newline \
                                flake8-comprehensions \
                                flake8-deprecated \
                                flake8-docstrings \
                                flake8-import-order \
                                flake8-quotes \
                                pytest \
                                pytest-cov \
                                pytest-runner \
                                pytest-repeat \
                                pytest-rerunfailures \
                                setuptools \
                                mock \
                                git+https://github.com/lark-parser/lark.git@0.7b
    ```



1. **Install asio (for fastrtps)**
    ```
    apt install --no-install-recommends -y \
        libasio-dev \
        libtinyxml2-dev
    ```

1. **Install miscellanius required apt packages for ROS2 last release repos**
    ```
    apt install -y  libpcre3 \
                    libpcre3-dev \
                    zlibc \
                    zlib1g \
                    zlib1g-dev \
                    xorg \
                    openbox \
                    libxaw7-dev \
                    libfreetype6-dev \
                    libglu1-mesa-dev freeglut3-dev mesa-common-dev \
                    qt5-default \
                    libogre-1.9-dev \
                    libxrandr-dev \
                    libopencv-dev \
                    libcurl4-openssl-dev \
                    libcurlpp-dev
    ```

1. **Install eigen lib**
    ```
    wget http://bitbucket.org/eigen/eigen/get/3.3.5.tar.bz2 -O eigen.tar.bz2
    tar -jxvf  eigen.tar.bz2 --one-top-level=eigen
    (cd eigen/* && mkdir build && cd build && cmake .. && make && make install)
    rm eigen eigen.tar.bz2 -rf
    ```

## Option B: Using docker image with development tools installed.

As an alternative of install all development tools in the host system, the user can pull a [docker](https://docs.docker.com/install/linux/docker-ce/ubuntu/) image direcly from [dockerhub](https://hub.docker.com/) with all tools installed.
Docker container offers an isolated operation sytem with only required tools.


```bash
sudo docker pull microros/linux
```
[![Docker Automated build](https://img.shields.io/docker/automated/microros/linux.svg?logo=docker)](https://hub.docker.com/microros/linux/)
[![Docker Build Status](https://img.shields.io/docker/build/microros/linux.svg?label=Last%20build)](https://hub.docker.com/microros/linux/)
[![Compare Images](https://images.microbadger.com/badges/image/microros/linux.svg)](hhttps://hub.docker.com/microros/linux/)


> **Note:** The only requirement for use docker images is to have installed [docker CE](https://docs.docker.com/install/).

> **Note:** This action may require superuser privileges.

> **Note:** Docker images built over latest Ubuntu, Ubuntu 16.04 and Ubuntu 18.04 docker images are available.
