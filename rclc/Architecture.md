# Architecture

rclc is composed by four main C structures:

1. rclc_node_t:

    Represents a ROS node.
    This structure holds the rcl_node along with the list of subscriptions created by the user.

1. rclc_publisher_t.

    Represents a ROS publisher.
    It holds the rcl_publisher instance and keeps track of the node which created it.

1. rclc_subscription_t.

    Represents a ROS subscription.
    Composed by the rcl_subscription along with the message type_support for the subscribed topic.
    It contains a pointer to the user callback that is used on every new message on the topic.

1. rclc_message_type_support_t.

    Used to get the type required functions like serialisation/deserialisation along with the type representation in C language.

    This type-support is generated from .msg files tying the functionality to the specific middleware used.
    To obtain the implementation you want, rclc provides with a utility macro: `RCLC_GET_MSG_TYPE_SUPPORT`.

    Message type support is composed by the size of the concrete type used and the concrete middleware type-support for this type.
    To get this concrete type information we use a type-support system described in detail [here](rosidl_typesupport_microxrcedds/README.md).

## Building blocks: Nodes, Subscriptions, Publishers and Type-support

The user application can be built using rclc main building blocks: Nodes, Subscriptions, Publishers and the type-support mechanism.

Nodes are the main object which the user needs to create first.
From this object, subscriptions and publishers can be created.
Nodes handle subscriptions and trigger user callbacks.

Subscriptions and publishers interact using ROS messages.
These messages are interchanged using different named topics.
Those ROS messages types are supported thanks to the type-support method described [here](rosidl_typesupport_microxrcedds/README.md).
