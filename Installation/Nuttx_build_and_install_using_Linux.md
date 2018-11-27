# Nuttx build and install steps (Using Linux)

In this section we will document all the steps and tools used to compile micro-ROS stack for NuttX platforms.
At the moment it has been tested on Olimex E407 board.

## Repositories

As in regular ROS 2. For micro-ROS there is a list of repositories and versions that could be obtained using the vcs tool.
The micro-ROS repositories in this case are slightly different from those needed for a Linux/Windows compilation.
In the case of NuttX the repositories are split in two different groups:

1. [uros_mcu.repos](repos/mcu/uros_mcu.repos) A list of repos to be crosscompiled for the micro controller. These are the actual executing code.
1. [uros_build.repos](repos/mcu/uros_build.repos) Repositories needed during compile time but not for execution. These are not needed to be compiled for the target platform.

For cloning all the required repositories:

```zsh
mkdir build_repos
vcs import build_repos < micro-ros-internal-doc/uros_build.repos
```

```zsh
mkdir mcu_repos
vcs import mcu_repos < micro-ros-internal-doc/uros_mcu.repos
```

## NuttX Apps

We will be using our personalized applications which contains sample code using micro-ROS.

```zsh
git clone git@github.com:julianbermudez/uros_apps.git
```

## Compile uros_build ws first

First step would be to compile the uros_build workspace using ```colcon build```

The docker already contains a uros_build precompiled in ament_ws. You just need to execute the local_install.bash.

We recommend to add COLCON_IGNORE on osrf packages as those are used for testing purposes.

## Docker for building NuttX

In this repository you can find the docker file used to generate the image. [docker](Dockerfile)
Once you have built the image you can run:

```bash
docker run --rm -it -v ~/.docker_history:/root/.bash_history -v $PWD/uros_apps:/root/apps -v $PWD/NuttX:/root/nuttx -v $PWD/uros_mcu:/root/ros2_ws/src --net=host -p 4444 -p 3333 -v /dev/bus/usb:/dev/bus/usb --privileged microros_nuttx
```

### Building

Once inside the docker container you should setup amment workspace.

```bash
cd ament_ws/
. install/local_setup.bash
```

```bash
cd nuttx
tools/configure.sh ../apps/configs/olimex-stm32-e407/uros/
make
```

### Flashing

```bash
cd nuttx
openocd -f interface/ftdi/olimex-arm-usb-tiny-h.cfg -f target/stm32f4x.cfg -c init -c "reset halt" -c "flash write_image erase nuttx.bin 0x08000000"
```

## Atomic support on ARM

Some of the Cortex-MX we use does not have support for atomic operations on 64bits atomic variables:

https://stackoverflow.com/questions/35776372/atomic-int64-t-on-arm-cortex-m3#35777259
https://answers.launchpad.net/gcc-arm-embedded/+question/616213

GCC implementation:
https://gcc.gnu.org/wiki/Atomic/GCCMM?action=AttachFile&do=view&target=libatomic.c

We have work around this issue implementing the atomic operations on 64Bits as regular memory read/writes.
A discrimination of the usage of the workaround is still to be done. This should be used ONLY on those architectures not supporting that kind of atomic operations.

## TODO

remove uClibcxx from config.ls

## Notes

rcl_interfaces needs to be on master, test_msgs installs on root. and it works on docker but not in real environment.
This has a patch on the ros2 repos and needs to be upgraded to later version.
The symptoms of this issue is issues while installing those messages as a non-elevated user.

https://github.com/ros2/rcl_interfaces/commit/db27f0e8619460848d80c1442f7fec0c56ee63e5

## SVD files

Some debuggers interfaces could benefit from the usage of SVD files.
Files are chip dependant and you can find yours in:
http://www.keil.com/dd2/stmicroelectronics/stm32f407zgtx/eula-container

If you download the "Device Family PAck" and open it as an archive you could find the SVD files of the family of chips along with other helpful files like drivers and multiple examples of using the board.