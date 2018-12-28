# Overview

micro-ROS Agent is a ROS2 node wrapping the [Micro XRCE-DDS Agent](https://github.com/eProsima/Micro-XRCE-DDS-Agent).
This node acts as a server between DDS Network and micro-ROS nodes inside MCU.
It receives and sends messages from micro-ROS nodes.
It also keeps track of the Micro-ROS nodes exposing them to the ROS 2 network.
The node interacts with DDS Global Data Space on behalf of the Micro-ROS nodes.

# Index

1. [Micro-ROS agent features](MicroROS_agent_features.md)
1. [Roadmap](Roadmap.md)
