# Architecture

`rmw_microxrcedds` library can be divided into five main components:

- Node
- Publisher
- Subscription
- Service Client
- Service Server

Apart from that main components, it provides with diverse set utilities as could be

- Initialization and shut-down functions
- Wait functionality to listen to incoming topics in subscribers
- Functions to query on ROS graph
- Memory functions
- Name checkers

In micro-ROS we are using the upper `rmw` as it is in ROS2.

The `rmw-microxrcedds` library provides a definition to the API declared in `rmw`.
At the moment we have a partial definition of that API including:

- Nodes.
- Publishers.
- Subscription.
- Wait on subscriptions.

The middleware used to implement these components is Micro XRCE-DDS.

## Nodes

This component provides the upper layer, `rcl`, with the capability to create Nodes.
In our case, a Node initializes a Micro XRCE-DDS session and creates a participant in the Micro XRCE-DDS Agent.

In micro-ROS there is a limited amount of nodes to be created.
This limitation [can be configured](Configuration.md) in the user configuration before building the application.

## Publishers

With a node, the `rcl` can create a publisher to a topic upon user request.
To create the publisher, the upper layers provide the type support for the type of the topic along with the topic name itself.
The creation of a rmw_publisher creates a publisher a datawriter and registers the topic in the Micro XRCE-DDS Agent.

In `rmw_microxrcedds` the creation of the entities [could be configured](Configuration.md) to use XML or refs.
Some of their fundamental differences are:

* rmw generates XML at runtime and refs are statically configured, with all the memory requirements this implies. Memory wise; refs are less memory demanding than XML representations.
* XMLs are generated at runtime, and refs need to be pre-configured in the Micro XRCE-DDS Agent.
* Refs reduce the payload size, so it provides an improvement in bandwidth usage compared to the usage of XML.

In this first version, there is a Micro-ROS-agent package which generates that Micro XRCE-DDS Agent and its configuration for you automatically using the ROS message definitions built.

Once a publisher is created `rmw_microxrcedds` implements a publishing API.
This will take a non-serialized message for a topic, serialize it using the type support provided to the publisher creation function and then push the message to the Micro XRCE-DDS Agent.

### Subscriptions

Analogous to publishers, subscriptions can be created using an already created node.
On subscription creation, a subscriber and datareader are created on Micro XRCE-DDS Agent, and the topic is registered.

A vital thing to notice is that topic messages are not received until a wait is performed. 
For more information on the reasons for this, please read the [official Micro XRCE-DDS documentation](https://micro-xrce-dds.readthedocs.io/en/latest/) along with the XRCE-DDS protocol it implements.

On each call to wait, only one subscription is processed, so to process all the subscriptions, some looping mechanism should be provided (spin functions, for example).

## Waits

For message receptions, rmw_wait must be performed.
On this call, `rmw_microxrcedds` will ask all the publications to get new data from their topics. That request uses the Micro XRCE-DDS request data function, which asks the Micro XRCE-DDS Agent for the latest messages on the topics.

After requesting data, `rmw_microxrcedds` waits on the Micro XRCE-DDS session till a timeout occurs or at least one new topic message arrived. The subscription is flagged as pending to read data from.

Once the wait gets triggered, a take data is called for the subscription with new messages.
This call deserialises the data using the type support provided to the subscription on its creation and passes the deserialised data to the upper layers which will call the user callback.
