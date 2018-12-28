# micro-ROS Agent features

This section will explain the available features that Micro-ROS Agent has.

## XML automatic generation

During the build process, Micro-ROS agent package will look for all ROS2 messages to generate an initial list of XML profiles.
These profiles can are referenced in the Agent-Client communication to avoid sending the full XML content.
This reference mechanism can be switched on and off from the [Micro XRCE-DDS middleware](../rmw_microxrcedds/Configuration.md) layer.

## Agent-Client communication mechanism

Communication between the micro-ROS Agent and the micro-ROS nodes at the moment supports two types of transport:

- UDP.
- Serial-Port.

All available configurations are supported directly by the Micro XRCE-DDS agent.
The communication method and it's [configuration](https://micro-xrce-dds.readthedocs.io/en/latest/agent.html#micro-xrce-dds-agent) is handled directly by the Micro XRCE-DDS Agent.
