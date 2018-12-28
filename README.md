# micro-ROS

[![GitHub license](https://img.shields.io/github/license/microROS/micro-ROS-doc.svg)](https://github.com/microROS/micro-ROS-doc)

micro-ROS puts ROS2 onto microcontrollers, making them first-class participants of the ROS 2 environment.

micro-ROS is composed by multiple repositories:

* [micro-ROS-demos](https://github.com/microROS/micro-ROS-demos):
    Sample code using rclc and rclcpp implementations.
* [rmw-microxrcedds](https://github.com/microROS/rmw-microxrcedds):
    RMW implementation using Micro XRCE-DDS middleware.
* [rosidl_typesupport_microxrcedds](https://github.com/microROS/rosidl_typesupport_microxrcedds):
    Type support for Micro XRCE-DDS
* [micro-ROS-Agent](https://github.com/microROS/micro-ROS-Agent):
    Micro-ROS-Agent package implementation.
* [osrf_testing_tools_cpp](https://github.com/microROS/osrf_testing_tools_cpp):
    Common testing tools for C++ which are used for testing in various OSRF projects.
    It is a fork containing fixes.
* [rclc](https://github.com/microROS/rclc):
    ROS Client Library for the C language.
* [rcl](https://github.com/microROS/rcl):
    Library to support the implementation of language-specific ROS Client Libraries.
* [NuttX](https://github.com/microROS/NuttX):
    Official micro-ROS RTOS.
* [apps](https://github.com/microROS/apps):
    micro-ROS applications to be used along with NuttX.
* [docker](https://github.com/microROS/docker):
    Docker-related material to set up, configure and develop with micro-ROS hardware.
* [HRIM](https://github.com/microROS/HRIM):
    An information model for robot hardware. Facilitates interoperability across manufacturers.

## Documentation

In this repository you can find all the micro-ROS related documentation:

* [Install and run](./docs/install_and_run.md): This is a guide on how to set up environment and run the provided examples.
* [rclc](./docs/rclc.md): User API layer documentation and diagrams.
* [rmw-microxrcedds](./docs/rmw_microxrcedds.md): Documentation related to the middleware layer using Micro XRCE-DDS.
* [type_support](./docs/type_support.md): Type support mechanism documentation.


## Dockers images

### microros/linux

[![Docker Automated build](https://img.shields.io/docker/automated/microros/linux.svg?logo=docker)](https://hub.docker.com/r/microros/linux/)
[![Docker Build Status](https://img.shields.io/docker/build/microros/linux.svg?logo=docker)](https://hub.docker.com/r/microros/linux/)

[![Compare Images](https://images.microbadger.com/badges/image/microros/linux.svg)](https://microbadger.com/images/microros/linux)

### microros/stm32-e407

[![Docker Automated build](https://img.shields.io/docker/automated/microros/stm32-e407.svg?logo=docker)](https://hub.docker.com/r/microros/stm32-e407/)
[![Docker Build Status](https://img.shields.io/docker/build/microros/stm32-e407.svg?logo=docker)](https://hub.docker.com/r/microros/stm32-e407/)

[![Compare Images](https://images.microbadger.com/badges/image/microros/stm32-e407.svg)](https://microbadger.com/images/microros/stm32-e407)
