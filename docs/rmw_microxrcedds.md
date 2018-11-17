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