# Nuttx (Linux) quick start

For this quick start an example of how a publisher Micro node running in an olimex may communicate with a Micro node running on a linux host.

## Micro-ROS Agent (Linux side)

1. **Pull ROS2 - Quick start Agent docker image**

    ```shell
    sudo docker pull microros/agent_linux
    ```

    > **Note:** Skip this step if you already have installed the 'ROS2 - Quick start Agent' from a previous start guide.

    > **Note:** For this step superuser privileges are required.

1. **Run 'ROS2  - Quick start Agent' docker container and execute Micro-ROS Agent for a serial transport**

    ```shell
    sudo docker run -it --rm --privileged --net=host ros2_quickstart_agent
    (LAST_PLUGGED_DEV=$(dmesg | grep "converter now attached" | tail -c 8) && uros_agent serial /dev/$LAST_PLUGGED_DEV)
    ```

    > **Note:** For this step you are going to need to plug the [USB-serial converter device](https://www.olimex.com/Products/Components/Cables/USB-Serial-Cable/USB-Serial-Cable-F/).

    > **Important:** Unplug and plug the mini-USB connector device before execute this step.

    > **Note:** For this step superuser privileges are required.

    > **Note:** After execute these steps you will need to leave the terminal opened and open a new one for the below steps.

1. **Run 'ROS2  - Quick start Agent' docker container and execute Micro-ROS Agent for a udp transport**

    ```shell
    sudo docker run -it --rm --privileged --net=host ros2_quickstart_agent
    uros_agent udp 8888
    ```

    > **Note:** After execute these steps you will need to leave the terminal opened and open a new one for the below steps.

## Micro-ROS Client (Linux side)

1. **Pull ROS2 - Quick start Client docker image**

    ```shell
    sudo docker pull microros/client_linux
    ```

    > **Note:** Skip this step if you already have installed the 'ROS2 - Quick start client' from a previous start guide.

    > **Note:** For this step superuser privileges are required.

1. **Run 'ROS2  - Quick start Client' docker container and run an Subscriber**

    ```shell
    sudo docker run -it --rm --net=host ros2_quickstart_client
    int32_subscriber_c
    ```

     > **Note:** After execute these steps you will need to leave the terminal opened and open a new one for the below steps.

## Micro-ROS Client (Nuttx side)

1. **Pull ROS2&Nuttx - Quick start stm32-e407 docker image**

    ```shell
    sudo docker pull microros/client_stm32-e407
    ```
    > **Note:** For this step superuser privileges are required.

1. **Run docker container for agent-side applications**

    ```shell
    sudo docker run --rm -it --net=host -p 4444 -p 3333 -v /dev/bus/usb:/dev/bus/usb --privileged ros2-nuttx_quickstart_stm32-e407
    ```
     >**Note:** For this step superuser privileges are required.*

1. **Flash binary file into olimex board**

    To write the created binary into the [stm32-e407 board](https://www.olimex.com/Products/ARM/ST/STM32-E407/open-source-hardware) connect the [JTAG ARM-USB-TINY-H](https://www.olimex.com/Products/ARM/JTAG/ARM-USB-TINY-H/) with the board and the pc and execute the below command.

    ```shell
    (cd ~/nuttx && openocd -f interface/ftdi/olimex-arm-usb-tiny-h.cfg -f target/stm32f4x.cfg -c init -c "reset halt" -c "flash write_image erase nuttx.bin 0x08000000")
    ```
    >**Note:** You may have to hold press the reset button for a little while in order to start the board boot-loader and make possible the flash process. 
    Read [stm32-e407 board manual](https://www.olimex.com/Products/ARM/ST/STM32-E407/resources/STM32-E407.pdf) to have more detailed information about the flashing process.
    
    >**Note:** After you get the output: "wrote *X* bytes from file nuttx.bin in *X*s (*X* KiB/s)", a debugger server is started. 
    The Debugger server is not necessary so you may close it (press ctrl + c).

1. **Open serial communication terminal and start publisher example**

    ```shell
    (LAST_PLUGGED_DEV=$(dmesg | grep "USB ACM device" | tail -c 24 | head -c 7) && minicom -D /dev/$LAST_PLUGGED_DEV -b 115200 --color=on)
    ```

    ```shell
    nsh> publisher
    ```
    >**Important:** Unplug and plug the mini-USB connector device before execute this step.

    >**Note:** To exit minicom press "ctrl + a" release them and press "x"

After this steps you will see how publisher (running in Olimex board) and subscriber (running in the Linux system) share messages.
