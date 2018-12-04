# Nuttx build and install steps (Using Linux)

## Step 1 (Option A): Set-up ROS2 and Nuttx development environment

### ROS2 development environment

The commands for setup ROS2 development environment are the same as [Install ROS2 development environment for Linux](./Linux_build_and_install_steps.md#step-1-(option-a):-install-ros2-development-environment) section.

### Nuttx development environment

To setup a Nuttx develop environment execute the below commands. 

1. Install essential required for Nuttx complilation.

    ```bash
    apt install -y  bison \
                    flex \
                    gperf \
                    libgmp-dev \
                    libmpc-dev \
                    libmpfr-dev \
                    libisl-dev \
                    binutils-dev \
                    libelf-dev \
                    libexpat1-dev \
                    zlib1g-dev \
                    libncurses5-dev \
                    gettext \
                    texinfo \
                    gcc-arm-none-eabi \
                    gdb-multiarch \
                    openocd \
                    libusb-1.0-0-dev \
                    automake \
                    autotools-dev
    ```

1. Install utils and tools.
    ```bash 
    apt install -y  remake \
                    vim \
                    screen \
                    ranger
    ```

1. Clone nuttx repo
    ```bash
    git clone https://bitbucket.com/nuttx/nuttx.git ~/nuttx
    ```

1. Clone nuttx apps repo.
    ```bash
    git clone https://github.com/microROS/apps.git ~/apps 
    git -C ~/apps checkout feature/Ubuntu18Update
    ```

1. Clone, build and install kconfig-frontends 
    ```bash
    git clone https://bitbucket.org/nuttx/tools.git ~/tools
    (cd tools/kconfig-frontends && \
        ./configure --enable-mconf --disable-nconf --disable-gconf --disable-qconf && \
        LD_RUN_PATH=/usr/local/lib && make && make install && ldconfig)
    ```

1. Clone and install uclibc
    ```bash
    git clone https://bitbucket.org/nuttx/uclibc.git ~/uclibc
    (cd ~/uclibc/ && alias more='' && echo -e "Y\nY\nY\nY\n" | . ./install.sh ~/nuttx)
    ```

## Step 2 (Option A): Create work spaces

As in regular ROS2, for Micro-ROS there is a list of repositories and versions that could be obtained using the vcs tool.
The micro-ROS repositories in this case are slightly different from those needed for a Linux/Windows compilation.
In the case of NuttX the repositories are split in two different groups:

1. [uros_mcu.repos](repos/mcu/uros_mcu.repos) These are the ROS" build system.
A list of repos to be cross compiled for the micro controller.
These are the actual executing code and have to be compiled into the Nuttx build system.
1. [uros_build.repos](repos/mcu/uros_build.repos) These are the Micro-ROS source code.
Repositories needed during compile time but not for execution. 
These are not needed to be compiled for the target platform so it can be compiled in the host operation system.

To create the work spaces execute the below commands:

1. Download, build and install `Micro-ROS build system`.
    ```bash
    mkdir -p ~/uros_build_ws/src
    wget https://raw.githubusercontent.com/microROS/micro-ROS-doc/master/repos/mcu/uros_build.repos -O ~/uros_build_ws/uros_build.repos
    vcs import ~/uros_build_ws/src < ~/uros_build_ws/uros_build.repos
    (cd ~/uros_build_ws && colcon build --symlink-install --cmake-args -DBUILD_TESTING=OFF)
    ```

1. Download `Micro-ROS` source and ignore micro-ROS-demos.
    ```bash
    mkdir -p ~/uros_ws/src 
    wget https://raw.githubusercontent.com/microROS/micro-ROS-doc/master/repos/mcu/uros_mcu.repos -O ~/uros_ws/uros_mcu.repos 
    vcs import ~/uros_ws/src < ~/uros_ws/uros_mcu.repos
    touch ~/uros_ws/src/uros/micro-ROS-demo/COLCON_IGNORE
    ```

> **Note:** micro-ROS-demos are not supported yet.

> **Note:** The docker already contains a uros_build precompiled in ament_ws. You just need to execute the local_install.bash.

## Step 3 (Option A): Build Nuttx binary

To build a binary file that is going to be flashed on the microcontroller board execute:

```bash
    (cd ~/nuttx && tools/configure.sh ../apps/configs/olimex-stm32-e407/uros/)
    (. ~/uros_build_ws/install/local_setup.sh && cd ~/nuttx && make)
```

> **Note:** At this moment, the Olimex E407 board is the only board tested.


## Steps 1,2 and 3 (Option B): Using docker container with a ROS2-Nuttx development environment.

As an alternative to the installation of the ROS2 and Nuttx development environments on Linux, the creation of the workspaces and the binary build process the user can pull a [docker](https://docs.docker.com/install/linux/docker-ce/ubuntu/) image direcly from [dockerhub](https://hub.docker.com/r/microros/stm32-e407/) and use it to execute only the flashing to board step.

```bash
docker pull microros/stm32-e407
```

[![Docker Automated build](https://img.shields.io/docker/automated/microros/stm32-e407.svg?logo=docker)](https://hub.docker.com/r/microros/stm32-e407/)
[![Docker Build Status](https://img.shields.io/docker/build/microros/stm32-e407.svg?label=Last%20build)](https://hub.docker.com/r/microros/stm32-e407/)
[![Compare Images](https://images.microbadger.com/badges/image/microros/stm32-e407.svg)](https://microbadger.com/images/microros/stm32-e407)


> **Note:** In order to flash the binary file directly from the docker container map the usb program device into the container.

```bash
docker run --rm -it --net=host -p 4444 -p 3333 -v /dev/bus/usb:/dev/bus/usb --privileged microros/stm32-e407
```

## step 4: Flashing binary into the stm32-e407

To write the created binary into the stm32-e407 board connect the JTAG Debugger with the board and the pc and execute the below command.

```bash
cd ~/nuttx
openocd -f interface/ftdi/olimex-arm-usb-tiny-h.cfg -f target/stm32f4x.cfg -c init -c "reset halt" -c "flash write_image erase nuttx.bin 0x08000000"
```

## Notes

### Atomic support on ARM

Some of the Cortex-MX we use does not have support for atomic operations on 64bits atomic variables:

https://stackoverflow.com/questions/35776372/atomic-int64-t-on-arm-cortex-m3#35777259
https://answers.launchpad.net/gcc-arm-embedded/+question/616213

GCC implementation:
https://gcc.gnu.org/wiki/Atomic/GCCMM?action=AttachFile&do=view&target=libatomic.c

We have work around this issue implementing the atomic operations on 64Bits as regular memory read/writes.
A discrimination of the usage of the workaround is still to be done. This should be used ONLY on those architectures not supporting that kind of atomic operations.

## SVD files

Some debuggers interfaces could benefit from the usage of SVD files.
Files are chip dependant and you can find yours in:
http://www.keil.com/dd2/stmicroelectronics/stm32f407zgtx/eula-container

If you download the "Device Family Pack" and open it as an archive you could find the SVD files of the family of chips along with other helpful files like drivers and multiple examples of using the board.
