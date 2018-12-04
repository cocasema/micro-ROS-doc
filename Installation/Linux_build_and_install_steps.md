# Linux build and install steps

## Step 1 (Option A): Install ROS2 development environment

The development environment for Micro-ROS in Linux platforms is the same as the one for [ROS2](https://github.com/ros2/ros2/wiki), so the same set-up is needed.
All required tool that is going to be intalled in this step is been extracted from the [ROS2 istallation guide](https://index.ros.org/doc/ros2/Linux-Install-Debians/).

1. Upgrade apt.
    ``` 
    apt update
    apt upgrade -y
    ```

1. Install first required packages.
    ```shell
    apt install -y  wget \
                    curl \
                    gnupg \
                    gnupg2 \
                    lsb-core \
                    locales
    ```

1. Add ROS2 repo key.
    ```
    wget http://repo.ros2.org/repos.key 
    apt-key add repos.key 
    rm repos.key 
    sh -c 'echo "deb [arch=amd64,arm64] http://repo.ros2.org/ubuntu/main `lsb_release -cs` main" > /etc/apt/sources.list.d/ros2-latest.list'
    ```

1. Configure locale.
    ```
    locale-gen en_US en_US.UTF-8 
    update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8 
    export  LANG='en_US.UTF-8' \
            LANGUAGE='en_US:en' \
            LC_ALL='en_US.UTF-8' \
            DEBIAN_FRONTEND=noninteractive
    ```

1. Install essential required apt packages for ROS2.
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

1. Install essential required python packages for ROS2.
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



1. Install asio (for fastrtps)
    ```
    apt install --no-install-recommends -y \
        libasio-dev \
        libtinyxml2-dev
    ```

1. Install miscellanius required apt packages for ROS2 last release repos.
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
                    libcurl4-openssl-dev
    ```

1. Install eigen lib
    ```
    wget http://bitbucket.org/eigen/eigen/get/3.3.5.tar.bz2 -O eigen.tar.bz2
    tar -jxvf  eigen.tar.bz2 --one-top-level=eigen
    (cd eigen/* && mkdir build && cd build && cmake .. && make && make install)
    rm eigen eigen.tar.bz2 -rf
    ```

## Step 1 (Option B): Using docker container with a ROS2 development environment.

As an alternative to the installation of the ROS2 development environment on Linux, the user can pull a [docker](https://docs.docker.com/install/linux/docker-ce/ubuntu/) image direcly from [dockerhub](https://hub.docker.com/).
Docker container offers an isolated ROS2 development environment.

```bash
docker pull microros/linux
```
[![Docker Automated build](https://img.shields.io/docker/automated/microros/linux.svg?logo=docker)](https://hub.docker.com/r/microros/linux/)
[![Docker Build Status](https://img.shields.io/docker/build/microros/linux.svg?label=Last%20build)](https://hub.docker.com/r/microros/linux/)
[![Compare Images](https://images.microbadger.com/badges/image/microros/linux.svg)](hhttps://hub.docker.com/r/microros/linux/)

## Step 2: Create work space

This step will create two work-spaces.
One for the client-side and other for the agent-side.
Client workspace will contain all Micro nodes and Agent workspace will contain all ROS2 nodes.
For now, we have to keep separate client-side packages from Agent-side packages.

For client-side workspace create an empty folder and inside it, you have to create an empty folder for the sources files named 'src'.

```bash
mkdir ~/client_ws
mkdir ~/client_ws/src
```

For agent-side workspace create an empty folder and inside it, you have to create an empty folder for the sources files named 'src'.

```bash
mkdir ~/agent_ws
mkdir ~/agent_ws/src
```

## Step 3: Import Micro-ROS packages

For using Micro-ROS, You need a predefined group of ROS2 packages.
A [YAML](http://yaml.org/) file for [vcstool](https://github.com/dirk-thomas/vcstool) is provided to sync all the needed sources easily.

For this step you have to download a basic group of packages from.

Download all client-side repo package list and place it in the client workspace.

```bash
cd ~/client_ws
wget https://raw.githubusercontent.com/microROS/micro-ROS-doc/master/Installation/repos/client_minimum.repos -O micro_ros.repos
```

Then, clone all repos.

```bash
cd ~/client_ws
vcs-import src < micro_ros.repos
```

Download all agent-side repo package list and place it in the client workspace.

```bash
cd ~/agent_ws
wget https://raw.githubusercontent.com/microROS/micro-ROS-doc/master/Installation/repos/agent_minimum.repos -O micro_ros.repos
```

Then, clone all repos.

```bash
cd ~/agent_ws
vcs-import src < micro_ros.repos
```


## Step 4: Build Micro-ROS

Build all packages for the client workspace.

```bash
cd ~/client_ws
colcon build --cmake-args -DBUILD_SHARED_LIBS=ON
```

Build all packages for the agent workspace.

```bash
cd ~/agent_ws
colcon build --cmake-args -DBUILD_SHARED_LIBS=ON
```


## Step 5: Configure execution environment

Once colcon has finished building Micro-ROS, you need to set up the environment, so it finds all the required .os to run applications.
For doing so, colcon has copied for us the following batch files.

```bash
. ~/client_ws/install/./local_setup.bash
. ~/agent_ws/install/./local_setup.bash
```

The previous batch, sets up the environment for the current shell session.
From now on we can run our [sample nodes](https://github.com/microROS/micro-ROS-demos/blob/master/README.md).
