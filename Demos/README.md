# Overview

Micro-ROS demos organize all available functionalities in simple executable demonstrations for a new Micro-ROS user.
All demonstrations are available in the [Micro-ROS Demo](https://github.com/microROS/micro-ROS-demos) repo.

`Note:` To run the demonstrations, you need first to [install the ROS2 build tools](https://index.ros.org/doc/ros2/Installation/) and [build and install all the required packages](../MicroROS/Readme.md) for the required platform.


## Simple upper bounded publish-subscribe demonstration

The purpose of this demonstration is to publish and subscribe to a simple upper bounded ROS2 message.
The example will demonstrate how Micro-ROS layers (rcl, type support and rmw) will handle it.
Published ROS2 message contains just an int32 data.
Each time the publisher publish it, the message value will increment its value in one unit. 
Also, demonstrate how type support code is generated for a simple upper bounded message.

#### How to run the demo

1. [Linux](Linux_demo_run_commands.md#simple-upper-bounded-publish-subscribe)
1. [Windows](Windows_demo_run_commands.md#simple-upper-bounded-publish-subscribe)

## simple unbounded publish — subscribe demonstration

The purpose of this demonstration is to publish and subscribe to a simple unbounded ROS2 message.
The example will demonstrate how Micro-ROS layers (rcl, type support and rmw) will handle it.
Published ROS2 message contains just an unbounded string data.
Each time the publisher publish it, the message string value will increment its value in one unit. 
Also, demonstrate how type support code is generated for a simple unbounded message.

`Note:` Due the rmw uses static memory, unbounded messages are limited to an internal byte buffer.
The buffer size may be [configured](../RMW/Configuration.md) at compilation time.

#### How to run the demo

1. [Linux](Linux_demo_run_commands.md#simple-unbounded-publish-subscribe)
1. [Windows](Windows_demo_run_commands.md#simple-unbounded-publish-subscribe)

## Complex message demonstration

The purposes of this demonstration are to publish and subscribe to a complex ROS2 message.
This example will demonstrate how Micro-ROS layers (rcl, type support and rmw) will handle it.
The published ROS2 message contains the following types:
- All primitive data types.
- Nested message data.
- Unbounded strings data.
Each time the publisher publish it, all the parameters in the message value will increment its value in one unit. 
Also, demonstrate how type support code is generated for a complex message.

`Note:` Due the rmw uses static memory, unbounded messages are limited to an internal byte buffer.
The buffer size may be [configured](../RMW/Configuration.md) at compilation time.

#### How to run the demo

1. [Linux](Linux_demo_run_commands.md#complex-message-demonstration)
1. [Windows](Windows_demo_run_commands.md#complex-message-demonstration)

## Real application demonstration

This purpose of this demonstration is to show how Micro-ROS stack can be used in a real application scenario.
In this demonstration, an altitude control system is simulated.
The demo will show how can Micro-ROS nodes communicate with ROS2 nodes.

<p align="Center"><img  src="http://www.plantuml.com/plantuml/proxy?src=https://raw.githubusercontent.com/microROS/micro-ROS-doc/feature/MicroROSHomeDoc/Demos/Diagrams/ComunicationDiagram.plantuml"></p>
<p align="Center" style="font-size:200%;color:grey;">Communication diagram<p>

### Nodes descriptions

#### rad0_actuator

The mission of this node is to simulate a dummy engine power actuator.
It receives power increments and publishes the total power amount as a DDS topic.

The node is built using the Micro-ROS middleware packages (rmw_micro_xrcedds and rosidl_typesupport_microxrcedds).

It is meant to be running in a microcontroller processor, but for this demonstration, the node runs on the host PC.
The node is connected to the DDS world through a Micro XRCE-DDS Agent.

<p align="Center"><img  src="http://www.plantuml.com/plantuml/proxy?src=https://raw.githubusercontent.com/microROS/micro-ROS-doc/feature/MicroROSHomeDoc/Demos/Diagrams/ActuatorNodeDiagram.plantuml"></p>
<p align="Center" style="font-size:200%;color:grey;">Node workflow<p>

#### rad0_altitude_sensor

The mission of this node is to simulate a dummy altitude sensor.
It publishes the altitude variations as a DDS topic.

The node is built using the Micro-ROS middleware packages (rmw_micro_xrcedds and rosidl_typesupport_microxrcedds).

It is meant to be running in a microcontroller processor, but for this demonstration, the node runs on the host PC.
The node is connected to the DDS world through a Micro XRCE-DDS Agent.

<p align="Center"><img  src="http://www.plantuml.com/plantuml/proxy?src=https://raw.githubusercontent.com/microROS/micro-ROS-doc/feature/MicroROSHomeDoc/Demos/Diagrams/AltitudeSensorNodeDiagram.plantuml"></p>
<p align="Center" style="font-size:200%;color:grey;">Node workflow<p>
  

#### rad0_control

The mission of this node is to read altitude values and send to the actuator engine variations.
It also publishes the status (OK, WARNING or FAILURE) as a DDS topic.
The status depends on the altitude value.

The node is built using the ROS2 middleware packages (rmw_fastrtps and rosidl_typesupport_fastrtps).

It is meant to be running in on a regular PC, and it is directly connected to the DDS world.

<p align="Center"><img  src="http://www.plantuml.com/plantuml/proxy?src=https://raw.githubusercontent.com/microROS/micro-ROS-doc/feature/MicroROSHomeDoc/Demos/Diagrams/ControlCoreDiagram.plantuml"></p>
<p align="Center" style="font-size:200%;color:grey;">Node workflow<p>

#### rad0_display

The mission of this node is to simulate one LCD screen that prints the critical parameters.
It subscribes to the altitude, power and status messages available as a DDS topic.

The node is built using the Micro-ROS middleware packages (rmw_micro_xrcedds and rosidl_typesupport_microxrcedds).

It is meant to be running in a microcontroller processor, but for this demonstration, the node runs on the host PC.
The node is connected to the DDS world through a Micro XRCE-DDS Agent.

<p align="center"><img  src="http://www.plantuml.com/plantuml/proxy?src=https://raw.githubusercontent.com/microROS/micro-ROS-doc/feature/MicroROSHomeDoc/Demos/Diagrams/DisplayNodeDiagram.plantuml"></p>
<p align="Center" style="font-size:200%;color:grey;">Node workflow<p>


#### How to run the demo

1. [Linux](Linux_demo_run_commands.md#real-application-demonstration)
1. [Windows](Windows_demo_run_commands.md#real-application-demonstration)

<p align="center"><img  src="http://www.plantuml.com/plantuml/proxy?src=https://raw.githubusercontent.com/microROS/micro-ROS-doc/feature/MicroROSHomeDoc/Demos/Diagrams/Demo_SecuenceDiagram.plantuml"></p>
<p align="Center" style="font-size:200%;color:grey;">Communication sequence diagram<p>
