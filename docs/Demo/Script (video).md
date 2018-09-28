# Video script

## Download Docker image

- Download the image from the Docker repo.

```bash
docker pull boouga/bouncy_ros2_build_sources "ros2"
```

## Build and run Ros2

- Run new Docker image.

```bash
docker run -it --rm --privileged -v "$HOME/.ssh/":"/root/.ssh/" --net=host microros
```

- Create 'ros2.repo' file

```
echo "
repositories:
  src/ament/ament_cmake:
    type: git
    url: https://github.com/ament/ament_cmake.git
    version: master
  src/ament/ament_index:
    type: git
    url: https://github.com/ament/ament_index.git
    version: master
  src/ament/ament_lint:
    type: git
    url: https://github.com/ament/ament_lint.git
    version: master
  src/ament/ament_package:
    type: git
    url: https://github.com/ament/ament_package.git
    version: master
  src/ament/googletest:
    type: git
    url: https://github.com/ament/googletest.git
    version: ros2
  src/ament/osrf_pycommon:
    type: git
    url: https://github.com/osrf/osrf_pycommon.git
    version: master
  src/ament/uncrustify_vendor:
    type: git
    url: https://github.com/ament/uncrustify_vendor.git
    version: master
  src/eProsima/Fast-CDR:
    type: git
    url: https://github.com/eProsima/Fast-CDR.git
    version: master
  src/eProsima/Fast-RTPS:
    type: git
    url: https://github.com/eProsima/Fast-RTPS.git
    version: master
  src/osrf/osrf_testing_tools_cpp:
    type: git
    url: https://github.com/osrf/osrf_testing_tools_cpp.git
    version: master
  src/ros/class_loader:
    type: git
    url: https://github.com/ros/class_loader.git
    version: ros2
  src/ros/pluginlib:
    type: git
    url: https://github.com/ros/pluginlib.git
    version: ros2
  src/ros/resource_retriever:
    type: git
    url: https://github.com/ros/resource_retriever.git
    version: ros2
  src/ros/ros_environment:
    type: git
    url: https://github.com/ros/ros_environment.git
    version: bouncy
  src/ros/urdfdom_headers:
    type: git
    url: https://github.com/ros/urdfdom_headers.git
    version: master
  src/ros-perception/laser_geometry:
    type: git
    url: https://github.com/ros-perception/laser_geometry.git
    version: ros2
  src/ros-planning/navigation_msgs:
    type: git
    url: https://github.com/ros-planning/navigation_msgs.git
    version: ros2
  src/ros2/ament_cmake_ros:
    type: git
    url: https://github.com/ros2/ament_cmake_ros.git
    version: master
  src/ros2/common_interfaces:
    type: git
    url: https://github.com/ros2/common_interfaces.git
    version: master
  src/ros2/console_bridge_vendor:
    type: git
    url: https://github.com/ros2/console_bridge_vendor.git
    version: master
  src/ros2/demos:
    type: git
    url: https://github.com/ros2/demos.git
    version: master
  src/ros2/examples:
    type: git
    url: https://github.com/ros2/examples.git
    version: master
  src/ros2/example_interfaces:
    type: git
    url: https://github.com/ros2/example_interfaces.git
    version: master
  src/ros2/geometry2:
    type: git
    url: https://github.com/ros2/geometry2.git
    version: ros2
  src/ros2/kdl_parser:
    type: git
    url: https://github.com/ros2/kdl_parser.git
    version: ros2
  src/ros2/launch:
    type: git
    url: https://github.com/ros2/launch.git
    version: master
  src/ros2/libyaml_vendor:
    type: git
    url: https://github.com/ros2/libyaml_vendor.git
    version: master
  src/ros2/orocos_kinematics_dynamics:
    type: git
    url: https://github.com/ros2/orocos_kinematics_dynamics.git
    version: ros2
  src/ros2/poco_vendor:
    type: git
    url: https://github.com/ros2/poco_vendor.git
    version: master
  src/ros2/rcl:
    type: git
    url: https://github.com/ros2/rcl.git
    version: master
  src/ros2/rcl_interfaces:
    type: git
    url: https://github.com/ros2/rcl_interfaces.git
    version: master
  src/ros2/rclcpp:
    type: git
    url: https://github.com/ros2/rclcpp.git
    version: master
  src/ros2/rclpy:
    type: git
    url: https://github.com/ros2/rclpy.git
    version: master
  src/ros2/rcutils:
    type: git
    url: https://github.com/ros2/rcutils.git
    version: master
  src/ros2/realtime_support:
    type: git
    url: https://github.com/ros2/realtime_support.git
    version: master
  src/ros2/rmw:
    type: git
    url: https://github.com/ros2/rmw.git
    version: master
  src/ros2/rmw_connext:
    type: git
    url: https://github.com/ros2/rmw_connext.git
    version: master
  src/ros2/rmw_fastrtps:
    type: git
    url: https://github.com/ros2/rmw_fastrtps.git
    version: master
  src/ros2/rmw_implementation:
    type: git
    url: https://github.com/ros2/rmw_implementation.git
    version: master
  src/ros2/rmw_opensplice:
    type: git
    url: https://github.com/ros2/rmw_opensplice.git
    version: master
  src/ros2/robot_state_publisher:
    type: git
    url: https://github.com/ros2/robot_state_publisher.git
    version: ros2
  src/ros2/ros1_bridge:
    type: git
    url: https://github.com/ros2/ros1_bridge.git
    version: master
  src/ros2/ros2cli:
    type: git
    url: https://github.com/ros2/ros2cli.git
    version: master
  src/ros2/rosidl:
    type: git
    url: https://github.com/ros2/rosidl.git
    version: master
  src/ros2/rosidl_dds:
    type: git
    url: https://github.com/ros2/rosidl_dds.git
    version: master
  src/ros2/rosidl_defaults:
    type: git
    url: https://github.com/ros2/rosidl_defaults.git
    version: master
  src/ros2/rosidl_python:
    type: git
    url: https://github.com/ros2/rosidl_python.git
    version: master
  src/ros2/rosidl_typesupport:
    type: git
    url: https://github.com/ros2/rosidl_typesupport.git
    version: master
  src/ros2/rosidl_typesupport_connext:
    type: git
    url: https://github.com/ros2/rosidl_typesupport_connext.git
    version: master
  src/ros2/rosidl_typesupport_fastrtps:
    type: git
    url: https://github.com/ros2/rosidl_typesupport_fastrtps.git
    version: master
  src/ros2/rosidl_typesupport_opensplice:
    type: git
    url: https://github.com/ros2/rosidl_typesupport_opensplice.git
    version: master
  src/ros2/rviz:
    type: git
    url: https://github.com/ros2/rviz.git
    version: ros2
  src/ros2/sros2:
    type: git
    url: https://github.com/ros2/sros2.git
    version: master
  src/ros2/system_tests:
    type: git
    url: https://github.com/ros2/system_tests.git
    version: master
  src/ros2/tinyxml_vendor:
    type: git
    url: https://github.com/ros2/tinyxml_vendor.git
    version: master
  src/ros2/tinyxml2_vendor:
    type: git
    url: https://github.com/ros2/tinyxml2_vendor.git
    version: master
  src/ros2/tlsf:
    type: git
    url: https://github.com/ros2/tlsf.git
    version: master
  src/ros2/urdf:
    type: git
    url: https://github.com/ros2/urdf.git
    version: ros2
  src/ros2/urdfdom:
    type: git
    url: https://github.com/ros2/urdfdom.git
    version: ros2
  src/micro-ros-demo:
    type: git
    url: git@gitlab.intranet.eprosima.com:eProsima/micro-ros-demo.git
    version: Demo_Beta_Ros2
    " > /root/ros2_ws/ros2.repo
```


- Download Ros2 packages.

```bash
cd /root/ros2_ws/
vcs import . < ros2.repos
```

- Build all packages downloaded in the workspace (this step may take a while, be pacient). 

```bash
colcon build
```

- Run setting sctipt in order to set enviroment variables.

```bash
. ~/ros2_ws/install/local_setup.bash
```

- Run background node.

```bash
ros2 run control control &
```

- Check running nodes

```bash
jobs
```



## Build and run Micro-Ros

- Run new Docker image.

```bash
docker run -it --rm --privileged -v "$HOME/.ssh/":"/root/.ssh/" --net=host microros
```

- Create 'MicroRos_demo.repo' file

```
echo "
repositories:
  src/ament/ament_cmake:
    type: git
    url: https://github.com/ament/ament_cmake.git
    version: 409ee0403d8d1eba0dc5a58cf52dcec5eadc91d2
  src/ament/ament_index:
    type: git
    url: https://github.com/ament/ament_index.git
    version: 2ae14036e2c4f9db6fce022a47e300f71c47ea64
  src/ament/ament_lint:
    type: git
    url: https://github.com/ament/ament_lint.git
    version: 50f9bb46d6350b1709c76c3a819dd52b64d062c1
  src/ament/ament_package:
    type: git
    url: https://github.com/ament/ament_package.git
    version: a6c73aeff77c188597211fee9f9e34508487b531
  src/ament/ament_tools:
    type: git
    url: https://github.com/ament/ament_tools.git
    version: d238e0ed4f0adf529a330ae01ce1132b19e45ba4
  src/ament/googletest:
    type: git
    url: https://github.com/ament/googletest.git
    version: 4b6e624e78ba3d43c1602ffc80478ee7253e0b04
  src/ament/osrf_pycommon:
    type: git
    url: https://github.com/osrf/osrf_pycommon.git
    version: 7aa7f39b02073a25c97e82fece72dc0d74ecc4db
  src/ament/uncrustify:
    type: git
    url: https://github.com/ament/uncrustify.git
    version: 3cf6a5a9f8e731c97a8f8e9f03003d0d1357df51
  src/osrf/osrf_testing_tools_cpp:
    type: git
    url: https://github.com/osrf/osrf_testing_tools_cpp.git
    version: 2a560408633c0fb0c3fcf546df14e94868587a84
  src/ros2/ament_cmake_ros:
    type: git
    url: https://github.com/ros2/ament_cmake_ros.git
    version: 012ff0c460cb0455dc76569b23aaad92037d9fc2
  src/ros2/common_interfaces:
    type: git
    url: https://github.com/ros2/common_interfaces.git
    version: 1d80aae3de9b0c03052e13ec04a0c32693b38f61
  src/ros2/libyaml_vendor:
    type: git
    url: https://github.com/ros2/libyaml_vendor.git
    version: c69f385ebb8c971b9bdd2a87fc983c5a12b12811
  src/ros2/poco_vendor:
    type: git
    url: https://github.com/ros2/poco_vendor.git
    version: a574802c172da107721bda5462f12576ce4e5c70
  src/ros2/rcl:
    type: git
    url: https://github.com/ros2/rcl.git
    version: d13a902d9ad7d5cc9f2b5bce084f823a2e66589d
  src/ros2/rcl_interfaces:
    type: git
    url: https://github.com/ros2/rcl_interfaces.git
    version: d209af6a5d3c631c7059469f4f30983a57d8b950
  src/ros2/rcutils:
    type: git
    url: https://github.com/ros2/rcutils.git
    version: 244fa98ad6421ed3d6295bd157ec4b73400fab52
  src/ros2/rmw:
    type: git
    url: https://github.com/ros2/rmw.git
    version: b4e6596161f122d520e5085742dd2e48dcdca82b
  src/ros2/rmw_implementation:
    type: git
    url: https://github.com/ros2/rmw_implementation.git
    version: 060af05f581bd0937701f73fcda0a4375fc12652
  src/ros2/ros2cli:
    type: git
    url: https://github.com/ros2/ros2cli.git
    version: 4059a2f84addbbaab121b7f92b872dd5acffa4b3
  src/ros2/rosidl:
    type: git
    url: https://github.com/ros2/rosidl.git
    version: 5cdd36e02e0ad66a7652feed03bede33466413cf
  src/ros2/rosidl_defaults:
    type: git
    url: https://github.com/ros2/rosidl_defaults.git
    version: b342573e4b7719e88c24e39de081518051414dbb
  src/ros2/rosidl_typesupport:
    type: git
    url: https://github.com/ros2/rosidl_typesupport.git
    version: 45f5a0ad4d9eaa5fc9303be2633a1e306b84026f
  src/ros2/rclc:
    type: git
    url: git@gitlab.intranet.eprosima.com:eProsima/rclc.git
    version: 51f0743b83f04081dc486f79b22dfbe8b761e7e2
  src/ros2/rosidl_dds:
    type: git
    url: https://github.com/ros2/rosidl_dds.git
    version: 0.5.0
  src/eProsima/Fast-CDR:
    type: git
    url: https://github.com/eProsima/Fast-CDR.git
    version: 3a99f4cd02b136756cee6bf7ef2650c809369f1f
  src/eProsima/Fast-RTPS:
    type: git
    url: https://github.com/eProsima/Fast-RTPS.git
    version: a02c069b6f8067f335f588c3e11a359492b31587
  src/eProsima/micro-CDR:
    type: git
    url: https://github.com/eProsima/micro-CDR.git
    version: da4c5583668127013e6d52fca56eb0cf8e0474f7
  src/eProsima/micro-RTPS-agent:
    type: git
    url: https://github.com/eProsima/micro-RTPS-agent.git
    version: b3d10731321bd077956737842073554457bc1e09
  src/eProsima/micro-RTPS-client:
    type: git
    url: https://github.com/eProsima/micro-RTPS-client.git
    version: 212d8ca9fb071b71339c2bc7ac9d8596ad9b1843
  src/ros2/micro-ros-agent:
    type: git
    url: git@gitlab.intranet.eprosima.com:eProsima/micro-ros-agent.git
    version: Demo_Beta
  src/ros2/rosidl_typesupport_micrortps:
    type: git
    url: git@gitlab.intranet.eprosima.com:eProsima/rosidl_typesupport_micrortps.git
    version: Demo_Beta
  src/ros2/rmw_micrortps:
    type: git
    url: git@gitlab.intranet.eprosima.com:eProsima/rmw_micrortps.git
    version: Demo_Beta
  src/micro-ros-demo:
    type: git
    url: git@gitlab.intranet.eprosima.com:eProsima/micro-ros-demo.git
    version: Demo_Beta_MicroRos
    " > /root/ros2_ws/MicroRos_demo.repo
```

- Download Micro-Ros packages.

```bash
cd /root/ros2_ws/
vcs import . < MicroRos.repos
```

- Build all packages downloaded in the workspace (this step may take a while, be pacient). 

```bash
colcon build --cmake-args -DBUILD_SHARED_LIBS=ON
```

- Run setting sctipt in order to set enviroment variables.

```bash
. ~/ros2_ws/install/local_setup.bash
```

- Run agent node (it must be executed on the placed folder).

```bash
cd /root/ros2_ws/install/micrortps_agent/bin
./MicroRTPSAgent udp 8888 > /dev/null &
```

- Run background nodes.

```bash
/root/ros2_ws/install/actuator/lib/actuator/./actuator &
/root/ros2_ws/install/altitude_sensor/lib/altitude_sensor/./altitude_sensor &
```

- Run display node.

```bash
/root/ros2_ws/install/display/lib/display/./display
```


## Show working flow

- Show running nodes
    - Display all active nodes
    
    ```bash
    ros2 node list
    ```
- Information about topics
    
    - Display all topics

    ```bash
    ros2 topic list
    ```
    
    - show information about altitude topic

    ```bash
    ros2 topic info std_msgs_msg_Float64
    ```

    - Show all topic publicated messages

    ```bash
    ros2 topic echo /std_msgs_msg_Float64
    ```
