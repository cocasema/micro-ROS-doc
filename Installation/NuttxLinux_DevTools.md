# Development tools installation

## Host system installation

### ROS2 development tools

Follow the steps to install ROS2 development tools located in the [Developmet tools installation for Linux](Linux_DevTools.md) section.

### Nuttx development tools

To setup a Nuttx develop environment execute the below commands. 

1. **Install essential required for Nuttx complilation**

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

1. **Install utils and tools**
    ```bash 
    apt install -y  remake \
                    vim \
                    screen \
                    ranger \
                    minicom
    ```

1. **Clone nuttx repo**
    ```bash
    git clone https://bitbucket.com/nuttx/nuttx.git ~/nuttx
    ```

1. **Clone nuttx apps repo**
    ```bash
    git clone https://github.com/microROS/apps.git ~/apps 
    git -C ~/apps checkout feature/Ubuntu18Update
    ```

1. **Clone, build and install kconfig-frontends**
    ```bash
    git clone https://bitbucket.org/nuttx/tools.git ~/tools
    (cd tools/kconfig-frontends && \
        ./configure --enable-mconf --disable-nconf --disable-gconf --disable-qconf && \
        LD_RUN_PATH=/usr/local/lib && make && make install && ldconfig)
    ```

1. **Clone and install uclibc**
    ```bash
    git clone https://bitbucket.org/nuttx/uclibc.git ~/uclibc
    (cd ~/uclibc/ && alias more='' && echo -e "Y\nY\nY\nY\n" | . ./install.sh ~/nuttx)
    ```
