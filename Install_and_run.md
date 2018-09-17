# Micro-ROS Setup

## Overview

This guide provides you with a quick summary of how to install and configure the ROS2 environment for a micro-ROS workspace.

## Windows

## Linux

### Step 1: Set-up ROS2 environment

Below are two ways to set-up a working environment for ROS2.

#### Using Docker

The easiest option is to run a Docker container with all needed tools. Using Docker containers is preferred to the second option due to the isolation provided by Docker containers to your micro-ROS environment from the local operating system, so our host operating system and tools won't interfere with the micro-ROS compilation processes.

For this step is necessary to have Docker engine installed on the host computer. The official [tutorial](https://runnable.com/docker/install-docker-on-linux) can help you installing and setting-up Docker. Also, if you want to know more about Dockers technology you can visit the [web page](https://docs.docker.com/).

To run the docker environment, you have to execute the next bash line.

```bash
docker run -it --privileged --mount type=bind,source="WORKSPACE_PATH",target="/root/ros2_ws/" --net=host microros
```

Once you have the docker installed, you can execute this bash line to download the needed docker image.

```bash
docker pull boouga/bouncy_ros2_build_sources IMAGE_NAME
```

This Docker image has been pre-built with the ROS2 environment for the Bouncy release and this command will download it. The image will be named as "IMAGE_NAME". In case you don't set the image name value, it will be named as the remote pulled image is.

To run the docker image you have to execute the next bash line.

```bash
docker run -it --privileged --mount type=bind,source="WORKSPACE_PATH",target="/root/ros2_ws/" --net=host microros
```

Note: The "--mount type=bind,source="WORKSPACE_PATH",target="/root/ros2_ws/" argument will link your local directory WORKSPACE_PATH to the '/root/ros2_ws/' placed in the running docker container.  This means, all changes you make inside this directory will take place in you local directory.

## Installing ROS2 environment into host operation system

For this option you will need to install in your host:

- Ros2: This is needed for build, run and manage ROS2 packages. The installation is full documented in this [link](https://github.com/ros2/ros2/wiki/Installation).

- pip: This is a package manager needed to intall the VCS import tool. The installation if full documented in this [link](https://pip.pypa.io/en/stable/installing/)

- VSC import: This tool is required for the ros2/microros packages import. To install it, using pip, you have to execute the next bash line

```bash
sudo pip install vcstool
```

## Step 2: import micro-ROS packages

In this step you are going to import all micro-ROS packages. First you have to create the the micro_ros.repo with the following content and place it into the ROS2 work space "WORKSPACE_PATH".

~~~
repositories:
  ament/ament_cmake:
    type: git
    url: https://github.com/ament/ament_cmake.git
    version: 0.5.0
  ament/ament_index:
    type: git
    url: https://github.com/ament/ament_index.git
    version: 0.5.1
  ament/ament_lint:
    type: git
    url: https://github.com/ament/ament_lint.git
    version: 0.5.2
  ament/ament_package:
    type: git
    url: https://github.com/ament/ament_package.git
    version: 0.5.1
  ament/ament_tools:
    type: git
    url: https://github.com/ament/ament_tools.git
    version: 0.5.0
  ament/googletest:
    type: git
    url: https://github.com/ament/googletest.git
    version: 4b6e624e78ba3d43c1602ffc80478ee7253e0b04
  ament/osrf_pycommon:
    type: git
    url: https://github.com/osrf/osrf_pycommon.git
    version: 0.1.5
  ament/uncrustify:
    type: git
    url: https://github.com/ament/uncrustify.git
    version: 0.66.1
  ros2/rcl:
    type: git
    url: https://github.com/ros2/rcl.git
    version: 0.5.0
  ros2/rcutils:
    type: git
    url: https://github.com/ros2/rcutils.git
    version: 0.5.1
  ros2/rmw:
    type: git
    url: https://github.com/ros2/rmw.git
    version: 0.5.0
  ros2/ament_cmake_ros:
    type: git
    url: https://github.com/ros2/ament_cmake_ros.git
    version: 0.5.0
  osrf/osrf_testing_tools_cpp:
    type: git
    url: https://github.com/osrf/osrf_testing_tools_cpp.git
    version: 1.0.0
  ros2/rcl_interfaces:
    type: git
    url: https://github.com/ros2/rcl_interfaces.git
    version: 0.5.0
  ros2/rosidl_defaults:
    type: git
    url: https://github.com/ros2/rosidl_defaults.git
    version: 0.5.0
  ros2/rosidl:
    type: git
    url: https://github.com/ros2/rosidl.git
    version: 0.5.1
  ros2/rosidl_dds:
    type: git
    url: https://github.com/ros2/rosidl_dds.git
    version: master
  ros2/rosidl_typesupport:
    type: git
    url: https://github.com/ros2/rosidl_typesupport.git
    version: 0.5.0
  ros2/rmw_implementation:
    type: git
    url: https://github.com/ros2/rmw_implementation.git
    version: 0.5.0
  ros2/poco_vendor:
    type: git
    url: https://github.com/ros2/poco_vendor.git
    version: 1.1.1
  ros2/common_interfaces:
    type: git
    url: https://github.com/ros2/common_interfaces.git
    version: 0.5.0
  ros2/rclc:
    type: git
    url: https://github.com/ros2/rclc.git
    version: master
  ros2/libyaml_vendor:
    type: git
    url: https://github.com/ros2/libyaml_vendor.git
    version: 1.0.0
  ros2/ros2cli:
    type: git
    url: https://github.com/ros2/ros2cli.git
    version: 0.5.2
  eProsima/micro-CDR:
    type: git
    url: https://github.com/eProsima/micro-CDR.git
    version: v1.0.0beta2
  eProsima/micrortps-client:
    type: git
    url: https://github.com/eProsima/micro-RTPS-client.git
    version: feature/reference
  eProsima/fastrtps:
    type: git
    url: https://github.com/eProsima/Fast-RTPS.git
    version: feature/attributes
  microROS/rmw_micrortps:
    type: git
    url: https://gitlab.intranet.eprosima.com/eProsima/rmw_micrortps.git
    version: feature/xml
  microROS/micro-ROS-agent:
    type: git
    url: https://gitlab.intranet.eprosima.com/eProsima/micro-ROS-agent.git
    version: developJ
  microROS/rosidl_typesupport_micrortps:
    type: git
    url: https://gitlab.intranet.eprosima.com/eProsima/rosidl_typesupport_micrortps.git
    version: develop
  microROS/micro-ROS-demo:
    type: git
    url: https://gitlab.intranet.eprosima.com/eProsima/micro-ROS-demo.git
    version: develop

~~~

If you are using the docker file you should execute the next bash lines to download all required packages.  

```bash
cd /home/ws_ros2
mkdir src
vcs import src < ros2.repos
```

If you are using the host operation system environment you will have to execute the commad inside the workspace folder.

```bash
cd WORKSPACE_PATH
mkdir src
vcs import src < ros2.repos
```

After this you should have located inside the src folder all the micro-ROS packages.

### Step 3: Build micro-ROS

Colcon is the tool used for building all micro-ROS packages. To build the project, you should execute the next bash line.

```bash
colcon build --cmake-args -DBUILD_SHARED_LIBS=ON
```

This bash execution will:

- Look for all packages in the directory (and its recurrent) starting from where you have call it.
- Build all dependencies and configure the cmake paths so the dependent package can find them.
- Install all packages.
- Generate a running setup script.

After the bash execution Colcon place three new folders into your working dir:

- The build directory is where Colcon stores intermediate files. For each package, Colcon creates a subfolder in which CMake is invoked.
- The install directory is where Colcon installs each package. By default, each package gets installed into a separate subdirectory.
- The log directory contains various logging information about each package build process.

### Step 4: Configure execution environment

To run micro-ROS2 applications, you should configure the environment using a generated script created after build step. For this, you have to execute the next command:

```bash
. ~/ros2_ws/install/local_setup.bash
```

Once you have executed de configuration script, you should be able to execute any ROS2 nodes.

### References documentation

- [Colcon build tool](https://github.com/ROS2/ROS2/wiki/Colcon-Tutorial)

- [Ros2](https://github.com/ROS2/ROS2/wiki)
