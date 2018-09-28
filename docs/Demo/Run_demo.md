# Overview

This demo demostration focuses on how micro-Ros works for one real situation. 
For this porpose, the demo will simulate altitude control device.
this simulated device will be composed by tree micro-Ros nodes and two Ros2 nodes. 

`[COMUNICATION DIAGRAM]`

Each node will have its own work mision explained below.


## Actuator node

The actuator node is meant to be run in on a microcontroller processor but for this demo simulation, the node will run on the host PC.
The node will be built using the Micro-Ros framework with the next characteristics:

- Communication thru rmw_micrortps that uses the Micro-RTPS client API. Click [here](https://github.com/eProsima/micro-RTPS-client) for more info.
- All comunucations will be handled by a Micro-RTPS agent node node.
- The UDP internet protocol is the communication interface used for communications with the Micro-RTPS Agent.
- All node is written on C programing language.
- Uses the RCLC library API. Click [here](https://github.com/ros2/rclc) for more info.

The mission of this node is to simulate the altitude engine power control. 
Also will publicate the engine power value as a DDS topic. 

`[NODE WORK FLOW DIAGRAM]`


## Display node

The display node is meant to be run in on a microcontroller processor but for this demo simulation, the node will run on the host PC.
The node will be built using the Micro-Ros framework with the next characteristics:

- Communication thru rmw_micrortps that uses the Micro-RTPS client API. Click [here](https://github.com/eProsima/micro-RTPS-client) for more info.
- All comunucations will be handled by a Micro-RTPS agent node.
- The UDP internet protocol is the communication interface used for communications with the Micro-RTPS Agent.
- All node is written on C programing language.
- Uses the RCLC library API. Click [here](https://github.com/ros2/rclc) for more info.

The mission of this node is to hear all DDS messages and display them on the execution terminal. 

`[NODE WORK FLOW DIAGRAM]`


## Altitude sensor node

The altitude sensor node is meant to be run in on a microcontroller processor but for this demo simulation, the node will run on the host PC.
The node will be built using the Micro-Ros framework with the next characteristics:

- Communication thru rmw_micrortps that uses the Micro-RTPS client API. Click [here](https://github.com/eProsima/micro-RTPS-client) for more info.
- All comunucations will be handled by a Micro-RTPS agent node.
- The UDP internet protocol is the communication interface used for communications with the Micro-RTPS Agent.
- All node is written on C programing language.
- Uses the RCLC library API. Click [here](https://github.com/ros2/rclc) for more info.

The mission of this node is to simulate altitude changes and publicate them as a DDS topic. 
In order to simulate altitude changes, the variations will be random dimensioned values.

`[NODE WORK FLOW DIAGRAM]`


## MicroRTPS Agent node

The Micro-RTPS Agent node is meant to be run on a microprocesor. 
The node will be built using the Ros2 framework with the next characteristics

- All node is written on C++ programing language.
- Used the Micro-RTPS Agent API. Click [here](https://github.com/eProsima/micro-RTPS-agent)for more info.

The purpuse of the node is to interconnect the XRCE-DDS world with the DDS world, in other words, this node will be used as a gateway to connect Micro-Ros nodes with each others and with the Ros2 nodes.



## control core

The control core node os meant to be run on a microprocesor.
The node will be build using the Ros2 framework with the next characteristics.

- Communication thru mrw_fastrtps that uses the fast-RTPS API. Click [here](https://github.com/eProsima/Fast-RTPS) for more info.
- All node is written on C++ programing language.
- Uses the RCLCPP library API. Click [here](https://github.com/ros2/rclcpp) for more info.

The purpose of the node is to control the actuator engine power by publishing engine power increments.
The control will be done with the incoming values given by the altitude subscription.
The node also will publish a status report (OK / Warning / FAILURE). 



# Build demo nodes

The Ros2 and Micro-Ros nodes will be built on different workspaces.
For both of them you will need to have installed and configure the Micro-Ros / ROS2 build enviroment. In this [link]() you will found all information you need to install and use a Mico-Ros / Ros2 enviroment.   
We recommend you to use the docker option for each workspace.


## Micro-Ros workspace 

Once you have it installed and configured the build tools, you have to follow the below steps:

- Make workspace directory.

- Make 'Micro-ros-Demo.repo' file.

- Import all packages usig the vcs import tool.

- Build al packages.


## Ros2 workspace 

Once you have it installed and configured the build tools, you have to follow the below steps:

- Make workspace directory.

- Make 'Ros2-Demo.repo' file.

- Import all packages usig the vcs import tool.

- Build al packages.


# Run demo

For this step you must have built successfully the Micro-ROS and ROS2 nodes. 
You will need to run separately the Micro-Ros nodes from the Ros2 nodes.
It's important run the Micro-RTPS Agent node before the Micro-Ros nodes due to Micro-Ros nodes needs it for a successful start.
As it been said, the Micro-Ros nodes will be running on the host computer for the work demonstration.

Below is shonw an example of how is comunication flow should work.

``[COMUNICATION FLOW DIAGRAM]``


## Run Ros2 nodes.

To start the Ros2 Nodes you must follow this steps

- Set local seting

``` bash
comand
```

- Run all nodes in background and send the output to an log file.

``` bash
comand
```



## Run Micro-Ros nodes

To start the Micro-Ros Nodes you must follow this steps

- Set local seting

``` bash
comand
```

- Run all nodes in background and send the output to an log file.

``` bash
comand
```


## Run notes
- To watch the node output in real time you can execute this sentence.

``` bash
comand
```