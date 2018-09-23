# rclc micro-ROS package

## Overview

Micro-ROS rclc is the ROS client library for C language.
This client library provides you with the tools to interact with other ROS2 systems, from within a C program.

Currently it provides a subset of the ROS concepts compared to other user libraries as rclcpp and rclpy.

## Building blocks: Nodes, Subscriptions, Publishers and Type-support

User application can be built using rclc main building blocks: Nodes, Subscriptions, Publishers and the type-support mechanism.

Nodes are the main object which the user needs to create first.
From this object, subscriptions and publishers can be created.
Nodes handle subscriptions and trigger user callbacks.

Subscriptions and publishers interact using ROS messages.
These messages are interchanged using different named topics.
Those ROS messages types are supported thanks to the type-support method described [here](typesupport.md).

## Architecture

rclc is composed by four main C structures:

1. rclc_node_t:

    Represents a ROS node.
    This structure holds the rcl_node along with the list of subscriptions created by the user.

1. rclc_publisher_t.

    Represents a ROS publisher.
    It holds the rcl_publisher instance and keep tracks of the node which created it.

1. rclc_subscription_t.

    Represents a ROS subscription.
    Composed by the rcl_subscription along with the message type_support for the subscribed topic.
    It contains a pointer to the user callback that is used on every new message on the topic.

1. rclc_message_type_support_t.

    Used to get the type required functions like serialisation/deserialisation along with the type representation in C language.

    This type-support is generated from .msg files tying the functionality to the specific middleware used.
    To obtain the implementation you want, rclc provides with a utility macro: `RCLC_GET_MSG_TYPE_SUPPORT`.

    Message type support is composed by the size of the concrete type used and a the concrete middleware type-support for this type.
    To get this concrete type information we use a type-support system described in detail [here](typesupport.md).

You can find the diagram in [rclc.puml](rclc.puml).

<!-- ![PlantUML model](http://www.plantuml.com/plantuml/proxy?src=https://raw.github.com/master/src/main/rclc.puml) -->

## API

---

## rclc

```C
rclc_ret_t rclc_init(int argc, char const * const * argv);
```

Function that initialize all micro-ROS stack in a cascade way, initialise rcl and rcl initialise rmw.
Arguments are passed down to the rcl layer.

This function should be called before any other one.

```C
bool rclc_ok(void);
```

Checks the status of rcl initialization.

---

## Node

```C
rclc_node_t * rclc_create_node(const char * name, const char * namespace_);
```

Creates a ROS node.
It initialises lower layer nodes, rcl_node and rmw_node.

```C
rclc_ret_t rclc_destroy_node(rclc_node_t * node);
```

Removes a ROS node and all its related publishers and subscribers.

```C
void rclc_spin_node_once(rclc_node_t * node, int64_t time_out_ms);
```

This function listens to the ROS subscriptions from the ROS node for a given time.

It triggers user callback calls with ROS messages received on subscriptions.

```C
void rclc_spin_node(rclc_node_t * node);
```

Listens to the ROS subscriptions from the ROS node till the user close the application.

It triggers user callback calls with ROS messages received on subscriptions.

---

## Publisher

```C
rclc_publisher_t * rclc_create_publisher(rclc_node_t * node,
                                         const rclc_message_type_support_t type_support,
                                         const char * topic_name,
                                         size_t queue_size);
```

The rclc_create_publisher function creates a ROS publisher within the given node.

The created publisher publish on the ROS topic `topic_name` using `type_support` as the type for the ROS messages.

```C
rclc_ret_t rclc_destroy_publisher(rclc_publisher_t * publisher);
```

Removes the publisher.

```C
rclc_ret_t rclc_publish(const rclc_publisher_t * publisher, const void * ros_message);
```

Publish a ROS message using the ROS topic tied to the `publisher`.

---

## Subscription

```C
rclc_subscription_t * rclc_create_subscription(rclc_node_t * node,
                                const rclc_message_type_support_t type_support,
                                const char * topic_name,
                                rclc_callback_t callback,
                                size_t queue_size,
                                bool ignore_local_publications);
```

Creates a ROS subscription within the given node.

The created subscription attends on the ROS topic `topic_name` using `type_support` as the type for the ROS messages.

Publications on `topic_name` trigger call to the user `callback` provided.
`rclc_spin_node` functions trigger those calls.

```C
rclc_ret_t rclc_destroy_subscription(rclc_subscription_t * subscription);
```

Removes ROS subscription.

---

## Type-support

```C
RCLC_GET_MSG_TYPE_SUPPORT(pkg, dir, msg)
```

Creates a ROS message type-support for the type pkg::dir::msg.
This type-support has been created at compile time using user messages definitions (.msg) as sources.

---
