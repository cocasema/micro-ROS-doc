# Overview


This will provide you with a quick summary of how install and configure the ros2 environment for a micro-ros workspace. This steps are tested in ubuntu operation system.


## Step 1: Generate ros2 enciroment

Below are two ways to generate a working environment for ros2.

### Using doker

The essyest option is to build a docker image with all needed tools. This also is de better than the second option due to the doker virtualization environment will isolate the micro-ros enviroment from the local operating system enviroment, so our host operating system won't interfere with the micro-ros compilation processes.

For this step is necessary to have the docker virtualization software installed on the host computer. The linked [tutorial](https://runnable.com/docker/install-docker-on-linux) will guides you to how install docker damemon. Also, if you want to know more about dockers virtualizavion you can visit the [web page](https://docs.docker.com/).

One you have the docker installed you can execute this bash line to download the needed docker image.
```console
$docker pull boouga/bouncy_ros2_build_sources
```

To run the docker environment you have to execute the next bash line.
```console
$docker run -it --privileged --mount type=bind,source="WORKSPACE_PATH",target="/root/ros2_ws/" --net=host microros
```


## Installing ros2 environment into host operation system

For this option you have a full documented tutorial [here](https://github.com/ros2/ros2/wiki/Installation)



## Step 2: import micro-ros packages

In this step you are going to import all micro-ros packages. First you have to create the the micro_ros.repo with the following content and place it into the ros2 work space "WORKSPACE_PATH".

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
    url: https://gitlab.sambaserver.eprosima.com/eProsima/microcdr.git
    version: v1.0.0beta2
  eProsima/micrortps-client:
    type: git
    url: https://gitlab.sambaserver.eprosima.com/eProsima/micrortps-client.git
    version: v1.0.0beta2
~~~


If you are using the docker file you should execute the next bash lines to download all required packages.  

```console
$ cd /home/ws_ros2
$ mkdir src
$ vcs import src < ros2.repos
```

If you are using the host operation system environment you will have to execute the commad inside the workspace folder.

```console
$ cd WORKSPACE_PATH
$ mkdir src
$ vcs import src < ros2.repos
```

After this you should have located inside the src folder all the micro-ros packages.


## Step 3: Build ros2

colcon is the tool used for building all micro-ros packages. For build the project you should execute the next bash line.

```console
$ colcon build --cmake-args -DBUILD_SHARED_LIBS=ON
```

After this, there will be three new folder placed.
- The build directory will be where intermediate files are stored. For each package a subfolder will be created in which e.g. CMake is being invoked.
- The install directory is where each package will be installed to. By default each package will be installed into a separate subdirectory.
- The log directory contains various logging information about each colcon invocation.
	

## Step 4: Configure execution enviroment

For the purpose of running ros2 nodes you should configure the  environment using a generated script created after build step. For this, you have to execute the next bash line:

```console
$ . ~/ros2_ws/install/local_setup.bash
```

Once you have executed de configuration script, you should be able to execute any ros2 nodes.


##  References documentation

- [Colcon build tool](https://github.com/ros2/ros2/wiki/Colcon-Tutorial)

- [Ros2](https://github.com/ros2/ros2/wiki)