# Configuration

The middleware implementation uses static memory assignations. 
Because of this, assignations of the memory are upper bounded so must be configured by the user before the build process. 
By default, the package sets the values for all memory bounded. 
The upper bound is configurable by a file that sets the values during the build process. 
The configuration file is placed in the rmw implementation: 'rmw_microxrcedds_c/rmw_microxrcedds.config' and has the following configurable parameters.

- CONFIG_MICRO_XRCEDDS_TRANSPORT (UDP/Serial): This parameter sets the type of communication that the Micro XRCE-DDS client uses.
- CONFIG_IP: In case you are using the UDP communication mode, this value indicates the IP of the Micro XRCE-Agent.
- CONFIG_PORT: In case you are using the UDP communication mode, this value indicates the port used by the Micro XRCE-Agent.
- CONFIG_DEVICE: In case you are using the serial communication mode, this value indicates the file descriptor of the seal port (Linux).
- CONFIG_MICRO_XRCEDDS_CREATION_MODE: chooses the preferred XRCE-DDS entities creation method. It could be XML or references.
Both create entities on the associated the Micro XRCE-DDS Agent; the difference is that the client dynamically creates XML, and references are pre-configured entities on the Micro XRCE-DDS Agent side.
- CONFIG_MAX_HISTORY: This value sets the number of MTUs buffers to. Micro XRCE-DDS client configuration provides their size.
- CONFIG_MAX_NODES: This value sets the maximum number of nodes.
- CONFIG_MAX_PUBLISHERS_X_NODE: This value sets the maximum number of publishers for a node.
- CONFIG_MAX_SUBSCRIPTIONS_X_NODE: This value sets the maximum number of subscriptions for a node.
- CONFIG_RMW_NODE_NAME_MAX_NAME_LENGTH: This value sets the maximum number of characters for a node name.
- CONFIG_RMW_TOPIC_NAME_MAX_NAME_LENGTH: This value sets the maximum number of characters for a topic name.
- CONFIG_RMW_TYPE_NAME_MAX_NAME_LENGTH: This value sets the maximum number of characters for a type name.
