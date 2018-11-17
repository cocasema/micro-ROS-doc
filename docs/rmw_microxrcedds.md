# rmw-microxrcedds

Micro-ROS provides an RMW implementation using the latest code from eProsima's middleware for constrained devices, Micro XRCE-DDS.
In this version, we have a C implementation for part of the middleware primitives declared by the ROS `rmw` upper layer.

In Micro-ROS, `rmw-microxrcedds`, as it is the only rmw_implementation found, will be used as the default one by `rmw_implementation` (`rmw_implementation` defaults the rmw implementation to use as `rmw_fastrps_cpp` in case it is found, otherwise it will use the first available alphabetically sorted). This unique implementation avoids the dependency with the third-party library Poco, used for the dynamic load of rmw_implementations.

## rmw

This layer is the ROS Middleware Abstraction Interface.
This library defines the interface used by upper layers in ROS stack and implemented using some middleware in the lower layers.

This API can be divided into five main components:

- Nodes
- Publisher
- Subscription
- Service Client
- Service Server

Apart from that main components, it provides with diverse set utilities as could be

- Initialisation and shut-down functions
- Wait functionality to listen to incoming topics in subscribers.
- Functions to query on ROS graph.
- Memory functions
- Name checkers.

In Micro-ROS we are using this `rmw` as it is in ROS2.

## rmw implementation with Micro XRCE-DDS

`rmw-microxrcedds` provides a definition to the API declared in `rmw`.
At the moment we have a partial definition of the whole API including:

-Nodes.
-Publishers.
-Subscription.
-Wait on subscriptions.

The middleware used to implement these components is Micro XRCE-DDS.

### Nodes

This component provides the upper layer, `rcl` with the capability to create Nodes.
In our case, a Node initialises a Micro XRCE-DDS session and creates a participant in the Micro XRCE-DDS Agent.

In micro-ROS there is a limited amount of nodes to be created.
This limitation can be configured in the user configuration before building the application.

### Publishers

With a node, the `rcl` can create a publisher to a topic upon user request.
To create node the upper layers provide the type support for the type of the topic along with the topic name itself.
The creation of a rmw_publisher creates a publisher a datawriter and registers the topic in the Micro XRCE-DDS Agent.

In rmw-microxrcedds the creation of the entities could be configured to use XML or refs.
Some of their fundamental differences are:

    * rmw generates XML at runtime and refs are statically configured, with all the memory requirements this implies. Memory wise; refs are less memory demanding than XML representations.
    * XMLs are generated at runtime, and refs need to be pre-configured in the Micro XRCE-DDS Agent.
    * Refs reduce the payload size, so it provides an improvement in bandwidth usage compared to the usage of XML.

In this first version, there is a micro-ROS-agent package which generates that Micro XRCE-DDS Agent for you automatically using the ROS message definitions built.

Once a publisher is created rmw-microxrcedds implements a publishing API.
This will take a non-serialized message for a topic, serialize it using the type support provided to the publisher creation function and then push the message to the Micro XRCE-DDS Agent.

### Subscriptions

Analogous to publishers, subscriptions can be created using an already created node.
On subscription creation, a subscriber and datareader are created on Micro XRCE-DDS Agent, and the topic is registered.

A vital thing to notice is that topic messages are not received till a wait is performed. For more information on the reasons for this , please read the official Micro XRCE-DDS documentation along with the XRCE-DDS protocol it implements.

On each call to wait, only one subscription is processed, so to process all the subscriptions, some kind of looping mechanism should be provided (spin functions, for example).

## Waits

For message receptions, rmw_wait must be performed.
On this call, rmw-microxrcedds will ask all the publications to get new data from their topics. This uses the Micro XRCE-DDS request data function, which asks the Micro XRCE-DDS Agent for the latest messages on the topics.

After requesting data, rmw-microxrcedds waits on the Micro XRCE-DDS session till a timeout occurs or at least one new topic message arrived. The subscription is flagged as pending to read data from.

Once the wait gets triggered a take data is called for the subscription with new messages.
This call deserialises the data using the type support provided to the subscription on its creation and passes the deserialised data to the upper layers which will call the user callback.

### Configuration

The middleware implementation uses static memory assignations. Because of this, assignations of the memory are upper bounded so must be configured by the user before the build process. By default, the package sets the values for all memory bounded. The upper bound is configurable by a file that sets the values during the build process. The configuration file is placed in the rmw implementation: 'rmw_microxrcedds_c/rmw_microxrcedds.config' and has the following configurable parameters.

*CONFIG_MICRO_XRCEDDS_TRANSPORT (udp/serial): This parameter sets the type of communication that the Micro XRCE-DDS client uses.
*CONFIG_IP: In case you are using the UDP communication mode, this value indicates the IP of the Micro XRCE-Agent.
*CONFIG_PORT: In case you are using the UDP communication mode, this value indicates the port used by the Micro XRCE-Agent.
*CONFIG_DEVICE: In case you are using the serial communication mode, this value indicates the file descriptor of the seal port (Linux).
*CONFIG_MICRO_XRCEDDS_CREATION_MODE: chooses the prefered XRCE-DDS entities creation method. It could be XML or references.
Both create entities on the associated the Micro XRCE-DDS Agent; the difference is that the client dynamically creates XML, and references are preconfigured entities on the Micro XRCE-DDS Agent side.
*CONFIG_MAX_HISTORY: This value sets the number of MTUs buffers to. Micro XRCE-DDS client configuration provides their size.
*CONFIG_MAX_NODES: This value sets the maximum number of nodes.
*CONFIG_MAX_PUBLISHERS_X_NODE: This value sets the maximum number of publishers for a node.
*CONFIG_MAX_SUBSCRIPTIONS_X_NODE: This value sets the maximum number of subscriptions for a node.
*CONFIG_RMW_NODE_NAME_MAX_NAME_LENGTH: This value sets the maximum number of characters for a node name.
*CONFIG_RMW_TOPIC_NAME_MAX_NAME_LENGTH: This value sets the maximum number of characters for a topic name.
*CONFIG_RMW_TYPE_NAME_MAX_NAME_LENGTH: This value sets the maximum number of characters for a type name.