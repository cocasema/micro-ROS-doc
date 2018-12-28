# API guide

## rclc

```C
rclc_ret_t rclc_init(int argc, char const * const * argv)
```
A function that initialises all micro-ROS stack in a cascade way, it initialises rcl and rcl initialises rmw.
Arguments are passed down to the rcl layer.
This function should be called before any other one.

`argc`
Init args count.

`argv`
Init args pointer.

`return`
[rcl response](http://docs.ros2.org/beta1/api/rcl/types_8h.html)

---
```C
bool rclc_ok(void)
```
Checks the status of rcl initialization.

`return`
True if OK.

---

## Node

```C
rclc_node_t * rclc_create_node(const char * name, const char * namespace_)
```
Creates a ROS node.
It initializes lower layer nodes, rcl_node and rmw_node.

`name`
Pointer to the node name string.

`namespace_`
Pointer to the namespace string.

`return`

---
```C
rclc_ret_t rclc_destroy_node(rclc_node_t * node)
```
Removes a ROS node and all its related publishers and subscribers.

`node`
The pointer to the node to be destroyed.

`return`
[rcl response](http://docs.ros2.org/beta1/api/rcl/types_8h.html)
---
```C
void rclc_spin_node_once(rclc_node_t * node, int64_t time_out_ms)
```
This function listens to the ROS subscriptions from the ROS node for a given time.
It triggers user callback calls with ROS messages received on subscriptions.

`node`
Pointer to the node to spin.

`time_out_ms`
Spin timeout.

```C
void rclc_spin_node(rclc_node_t * node)
```
Listens to the ROS subscriptions from the ROS node until the user closes the application.
It triggers user callback calls with ROS messages received on subscriptions.

`node`
Pointer to the node to spin.

---

## Publisher

```C
rclc_publisher_t * rclc_create_publisher(rclc_node_t * node, const rclc_message_type_support_t type_support, const char * topic_name, size_t queue_size)
```
The rclc_create_publisher function creates a ROS publisher within the given node.
The created publisher publish on the ROS topic `topic_name` using `type_support` as the type for the ROS messages.

`node`
Pointer to the owner node.

`type_support`
Pointer to the message type support.

`topic_name`
Pointer to the topic name string.

`queue_size`
Queue size.

`return`
Pointer to the created publisher.

---
```C
rclc_ret_t rclc_destroy_publisher(rclc_publisher_t * publisher)
```
Removes the publisher.

`publisher`
Pointer to the publisher to be destroyed.

`return`
[rcl response](http://docs.ros2.org/beta1/api/rcl/types_8h.html)

---
```C
rclc_ret_t rclc_publish(const rclc_publisher_t * publisher, const void * ros_message)
```
Publish a ROS message using the ROS topic tied to the `publisher`.

`publisher`
Pointer to the publisher.

`ros_message`
Pointer to the ROS message.

`return`
[rcl response](http://docs.ros2.org/beta1/api/rcl/types_8h.html)

## Subscription

```C
rclc_subscription_t * rclc_create_subscription(rclc_node_t * node, const rclc_message_type_support_t type_support, const char * topic_name, rclc_callback_t callback, size_t queue_size,bool ignore_local_publications)
```
Creates a ROS subscription within the given node.
The created subscription listens on the ROS topic `topic_name` using `type_support` as the type for the ROS messages.
Publications on `topic_name` trigger call to the user `callback` provided.
`rclc_spin_node` functions trigger those calls.

`node`
Pointer to the owner node.

`type_support`
Pointer to the message type support.

`topic_name` 
Pointer to the topic name string.

`callback`
Pointer to the function that will be called when incoming new message.

`queue_size`
Queue size.

`ignore_local_publications`
Set to true to ignore local publications.

`return`
Pointer to the created subscriber.

---
```C
rclc_ret_t rclc_destroy_subscription(rclc_subscription_t * subscription)
```
Removes ROS subscription.

`subscription`
Pointer to the subscription to be destroyed.

`return`
[rcl response](http://docs.ros2.org/beta1/api/rcl/types_8h.html)

## Type-support

```C
RCLC_GET_MSG_TYPE_SUPPORT(pkg, dir, msg)
```
Creates a ROS message type-support for the type pkg::dir::msg.
This type-support has been created at compile time using user messages definitions (.msg) as sources.

`pkg`
Package name.

`dir`
Type support directory.

`msg`
Message name.

`return`
Pointer to the type support.

## Executor

```C
rclc_executor_t * rclc_create_executor()
```
Creates a rclc_executor_t, which processes callbacks in ROS Nodes.

`return`
Created executor pointer.

---
```C
rclc_ret_t rclc_destroy_executor(rclc_executor_t * executor)
```
Destroys a rclc_executor_t.

`executor`
Pointer to the executor to be destroyed.

`return`
[rcl response](http://docs.ros2.org/beta1/api/rcl/types_8h.html)

---
```C
rclc_ret_t rclc_executor_add_node(rclc_executor_t * executor, const rclc_node_t * node)
```
Adds a ROS Node to the internal list of nodes in the executor.

`executor`
Pointer to the owner executor.

`node`
Pointer to the node to be added.

`return`
[rcl response](http://docs.ros2.org/beta1/api/rcl/types_8h.html)

---
```C
rclc_ret_t rclc_executor_spin()
```
Causes the executor to process callbacks generated by ROS Nodes.

`return`
[rcl response](http://docs.ros2.org/beta1/api/rcl/types_8h.html)

