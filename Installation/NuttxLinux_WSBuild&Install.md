# ROS2/Micro-ROS Workspace build and install

1. **Create work spaces**

    As in regular ROS2, for Micro-ROS there is a list of repositories and versions that could be obtained using the vcs tool.
    The micro-ROS repositories in this case are slightly different from those needed for a Linux/Windows compilation.
    In the case of NuttX the repositories are split in two different groups:

    * [uros_mcu.repos](repos/mcu/uros_mcu.repos) These are the ROS" build system.
    A list of repos to be cross compiled for the micro controller.
    These are the actual executing code and have to be compiled into the Nuttx build system.
    * [uros_build.repos](repos/mcu/uros_build.repos) These are the Micro-ROS source code.
    Repositories needed during compile time but not for execution. 
    These are not needed to be compiled for the target platform so it can be compiled in the host operation system.

    To create the work spaces execute the below commands:

    Download, build and install Micro-ROS build system
    ```bash
    mkdir -p ~/uros_build_ws/src
    wget https://raw.githubusercontent.com/microROS/micro-ROS-doc/master/repos/mcu/uros_build.repos -O ~/uros_build_ws/uros_build.repos
    vcs import ~/uros_build_ws/src < ~/uros_build_ws/uros_build.repos
    (cd ~/uros_build_ws && colcon build --symlink-install --cmake-args -DBUILD_TESTING=OFF)
    ```

    Download Micro-ROS source, ignore micro-ROS-demos and change to serial mode
    ```bash
    mkdir -p ~/uros_ws/src 
    wget https://raw.githubusercontent.com/microROS/micro-ROS-doc/master/repos/mcu/uros_mcu.repos -O ~/uros_ws/uros_mcu.repos 
    vcs import ~/uros_ws/src < ~/uros_ws/uros_mcu.repos
    touch ~/uros_ws/src/uros/micro-ROS-demo/COLCON_IGNORE
    sed -i s/"CONFIG_MICRO_XRCEDDS_TRANSPORT=.*"/"CONFIG_MICRO_XRCEDDS_TRANSPORT=serial"/g ~/uros_ws/src/uros/rmw-microxrcedds/rmw_microxrcedds_c/rmw_microxrcedds.config
    ```

    > **Note:** Micro-ROS-demos are not supported yet.

    > **Note:** The docker already contains a uros_build precompiled in ament_ws. You just need to execute the local_install.bash.

1. **Build Nuttx binary**

    Build the binary file that is going to be flashed on the micro-controller board

    ```bash
        (cd ~/nuttx && tools/configure.sh ../apps/configs/olimex-stm32-e407/uros/)
        (. ~/uros_build_ws/install/local_setup.sh && cd ~/nuttx && make)
    ```

    > **Note:** At this moment, the Olimex E407 board is the only board tested.

1. **Flashing binary into the stm32-e407**

    To write the created binary into the [stm32-e407 board](https://www.olimex.com/Products/ARM/ST/STM32-E407/open-source-hardware) connect the [JTAG ARM-USB-TINY-H](https://www.olimex.com/Products/ARM/JTAG/ARM-USB-TINY-H/) with the board and the pc and execute the below command.

    ```bash
    (cd ~/nuttx && openocd -f interface/ftdi/olimex-arm-usb-tiny-h.cfg -f target/stm32f4x.cfg -c init -c "reset halt" -c "flash write_image erase nuttx.bin 0x08000000")
    ```

    > **Note:** You may have to hold press the reset button for a little while in order to start the board bootloader and make possible the flash process.
    Read [stm32-e407 board manual](https://www.olimex.com/Products/ARM/ST/STM32-E407/resources/STM32-E407.pdf) to have more detailed information about the flashing process.

    >**Note:** After the stdout: "wrote *X* bytes from file nuttx.bin in *X*s (*X* KiB/s)" is shown, a debugger server is started.
    The Debugger server is not necessary so you may close it (press ctrl + c).

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
Files are chip dependent and you can find yours in:
http://www.keil.com/dd2/stmicroelectronics/stm32f407zgtx/eula-container

If you download the "Device Family Pack" and open it as an archive you could find the SVD files of the family of chips along with other helpful files like drivers and multiple examples of using the board.
