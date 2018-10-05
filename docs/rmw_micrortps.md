# rmw_micrortps

Micro-ROS provides an RMW implementation using the latest code from eProsima's middleware for constrained devices, Micro RTPS.
In this version we have a C implementation for part of the middleware primitives declared by the ROS `rmw` upper layer.

In Micro-ROS, `rmw_micrortps`, as it is the only rmw_implementation found, will be used as the default one by `rmw_implementation` (`rmw_implementation` defaults the rmw implementation to use as `rmw_fastrps_cpp` in case it is found, otherwise it will use the first available alphabetically sorted). This unique implementation avoids the dependency with the thirdparty library Poco, used for dynamic load of rmw_implementations.

## rmw

This layer is the ROS Middleware Abstraction Interface.
This library defines the interface used by upper layers in ROS stack and implemented using some middleware in the lower layers.

This API can be divided in five main components:

- Nodes
- Publisher
- Subscription
- Service Client
- Service Server

A part from that main components it provides with diverse set utilities as could be

- Initialization and showdown functions
- Wait functionality to listen to incoming topics in subscribers.
- Functions to query on ROS graph.
- Memory functions
- Name checkers.

In Micro-ROS we are using this `rmw` as it is in ROS2.

## rmw implementation with Micro RTPS

`rmw_micrortps` provides a definition to the API declared in `rmw`.
At the moment we have a partly definition of the whole API including:

-Nodes.
-Publishers.
-Subscription.
-Waits.

The middleware used to implement this components is Micro RTPS.

### Nodes

This component provides the upper layer, `rcl` with the capability to create Nodes.
In our case a Node initialises a Micro RTPS session and creates a participant in the Micro RTPS Agent.

In micro-ROS there is a limited amount of nodes to be created.
This limitation can be configured in the user configuration before building the application.

### Publishers

With a node, the `rcl` can create a publisher to a topic upon user request.
To create node the upper layers provide the type support for the type of the topic along with the topic name itself.
The creation of a rmw_publisher creates a publisher a datawriter and registers the topic in the Micro RTPS Agent.

In rmw_micrortps the creation of the entities could be configured to use XML or refs.
The main difference is that XML is generated within rmw_micrortps code at runtime, with the memory implications and refs needs to have the Micro RTPS Agent preconfigured with the references the user want to use.

In this first version there is a micro-ROS package which generates that Micro RTPS Agent for you automatically using the ROS message definitions built.

Once a publisher is created rmw_micrortps implements a publishing API.
This will take a non-serialized message for a topic, serialize it using the type support provided to the publisher creation function and then push the message to the Micro RTPS Agent.

### Subscriptions

Anologous to publishers, subsscriptions can be created using a already crated node.
On subscription creation a subscriber and datareader are created on Micro RTPS Agent and the topic is registered.

An important think to notice is that topic messages are not received till a wait is call upon rmw. For more information on the reasons on this behavior please read the oficial Micro RTPS documentation along with the XRCE-DDS protocol it implements.

## Waits

For message receptions rmw_wait must be performed.
On this call rmw_micrortps will ask all the publications to get new data from their topics. This uses the Micro RTPS request data function, which asks the Micro RTPS Agent for the latest messages on the topics.

After requesting data, rmw_micrortps waits on the Micro RTPS session till a timeout occurs or at least one new topic message arrived. The subscription is flagged as pending to read data from.

Once the wait gets triggered a take data is called per each subscription with new messages.
THis call deserialise the data using the type support provided to the subscription on its creation and pass the deserialised data to the upper layers which will call the user callback.

### Configuration