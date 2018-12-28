# API guide

```C
rmw_ret_t rmw_init(void)
```
Initialize all required parameters and functionalities for using the rmw_microxrcedds.
It must be called before calling anything else.

`Return`
* RMW_RET_OK
* RMW_RET_ERROR
* RMW_RET_TIMEOUT

---
```C
rmw_node_t * rmw_create_node(const char * name, const char * namespace, size_t domain_id, const rmw_node_security_options_t * security_options)
```
Create a new node.
A new session with the Micro XRCE-DDS Agent will be opened.

`name`
Node name.

`namespace`
namespace.

`domain_id`
Domain id.

`security_options`
Ros2 security options. 
This parameter is not used.

`Return`
Pointer to the owner node.
If error, a NULL value is returned.

---
```C
rmw_ret_t rmw_destroy_node(rmw_node_t * node)
```
Destroy node. 
The associated session with Micro XRCE-DDS Agent will be closed.

`node`
Pointer to the owner node.

`Return`
* RMW_RET_OK
* RMW_RET_ERROR
* RMW_RET_TIMEOUT

---
```C
const rmw_guard_condition_t * rmw_node_get_graph_guard_condition(const rmw_node_t * node)
```
No implemented yet.

---
```C
rmw_publisher_t * rmw_create_publisher(const rmw_node_t * node, const rosidl_message_type_support_t * type_support, const char * topic_name, const rmw_qos_profile_t * qos_policies)
```
Create a new publisher. 
Required entities for the publisher in the Micro XRCE-DDS agent will be created.

`node`
Pointer to the owner node.

`type_support`
Used type support.

`topic_name`
Pointer to topic name string.

`qos_policies`
Pointer to quality of service pointer. 
The rmw_microxrccedds library does not use this parameter.

`Return`
Pointer to the created publisher.
If error, a NULL pointer is returned.

---
```C
rmw_ret_t rmw_destroy_publisher(rmw_node_t * node, rmw_publisher_t * publisher)
```
Destroy an existing publisher.
All created entities in the Micro XRCE-DDS Agent will be destroyed.

`node`
Pointer to the owner node.

`publisher`
Pointer to the publisher to destroy.

`Return`
* RMW_RET_OK
* RMW_RET_ERROR
* RMW_RET_TIMEOUT


---
```C
rmw_ret_t rmw_publish(const rmw_publisher_t * publisher, const void * ros_message)
```
Publish a message using an existing publisher.

`publisher`
Existing publisher pointer. 

`ros_message`
Pointer to message to publish.

`Return`
* RMW_RET_OK
* RMW_RET_ERROR
* RMW_RET_TIMEOUT

---
```C
rmw_ret_t rmw_publish_serialized_message(const rmw_publisher_t * publisher, const rmw_serialized_message_t * serialized_message)
```
No implemented yet.

```C
rmw_subscription_t * rmw_create_subscription(const rmw_node_t * node, const rosidl_message_type_support_t * type_support, const char * topic_name, const rmw_qos_profile_t * qos_policies, bool ignore_local_publications)
```
Create a new subscription. 
Required entities for the publisher in the Micro XRCE-DDS agent will be created.

`qos_policies`
Pointer to quality of service polices. 
The rmw_microxrccedds library does not use this parameter.

`ignore_local_publications`
Set True to ignore local publications. 
The rmw_microxrccedds library ignores this parameter.

`Return`
Pointer to the created subscription. 
In error, a NULL pointer is returned.

---
```C
rmw_ret_t rmw_destroy_subscription(rmw_node_t * node, rmw_subscription_t * subscription)
```
Destroy an existing subscription.
All created entities in the Micro XRCE-DDS Agent will be destroyed.

`node`
Pointer to the owner node.

`subscription`
Pointer to the subscription to destroy.

`Return`
* RMW_RET_OK
* RMW_RET_ERROR
* RMW_RET_TIMEOUT

---
```C
rmw_ret_t rmw_take(const rmw_subscription_t * subscription, void * ros_message, bool * taken)
```
Take a received subscription message.

`subscription`
Pointer to the subscription object.

`ros_message`
Pointer where the message will be stored.

`taken`
Indicates if the message has been taken.

`Return`
* RMW_RET_OK
* RMW_RET_ERROR
* RMW_RET_TIMEOUT

---
```C
rmw_ret_t rmw_take_with_info(const rmw_subscription_t * subscription, void * ros_message, bool * taken, rmw_message_info_t * message_info)
```
Take a received subscription message with info.

`subscription`
Pointer to the subscription object.

`ros_message`
Pointer where the message will be stored.

`taken`
Indicates if the message has been taken.

`message_info`
This parameter is not used.

`Return`
* RMW_RET_OK
* RMW_RET_ERROR
* RMW_RET_TIMEOUT

---
```C
rmw_ret_t rmw_take_serialized_message(const rmw_subscription_t * subscription, rmw_serialized_message_t * serialized_message, bool * taken)
```
No implemented yet.

---
```C
rmw_ret_t rmw_take_serialized_message_with_info(const rmw_subscription_t * subscription, rmw_serialized_message_t * serialized_message, bool * taken, rmw_message_info_t * message_info)
```
No implemented yet.

---
```C
rmw_client_t * rmw_create_client(const rmw_node_t * node, const rosidl_service_type_support_t * type_support, const char * service_name, const rmw_qos_profile_t * qos_policies)
```
No implemented yet.

---
```C
rmw_ret_t rmw_destroy_client(rmw_node_t * node, rmw_client_t * client)
```
No implemented yet.

---
```C
rmw_ret_t rmw_send_request(const rmw_client_t * client, const void * ros_request, int64_t * sequence_id)
```
No implemented yet.

---
```C
rmw_ret_t rmw_take_response(const rmw_client_t * client, rmw_request_id_t * request_header, void * ros_response, bool * taken)
```
No implemented yet.

---
```C
rmw_ret_t rmw_service_server_is_available(const rmw_node_t * node, const rmw_client_t * client, bool * is_available)
```
No implemented yet.

---
```C
rmw_service_t * rmw_create_service(const rmw_node_t * node, const rosidl_service_type_support_t * type_support, const char * service_name, const rmw_qos_profile_t * qos_policies)
```
No implemented yet.

---
```C
rmw_ret_t rmw_destroy_service(rmw_node_t * node, rmw_service_t * service)
```
No implemented yet.

---
```C
rmw_ret_t rmw_take_request(const rmw_service_t * service, rmw_request_id_t * request_header, void * ros_request, bool * taken)
```
No implemented yet.

---
```C
rmw_ret_t rmw_send_response(const rmw_service_t * service, rmw_request_id_t * request_header, void * ros_response)
```
No implemented yet.

---
```C
rmw_ret_t rmw_serialize(const void * ros_message, const rosidl_message_type_support_t * type_support, rmw_serialized_message_t * serialized_message)
```
No implemented yet.

---
```C
rmw_ret_t rmw_deserialize(const rmw_serialized_message_t * serialized_message, const rosidl_message_type_support_t * type_support, void * ros_message)
```
No implemented yet.

---
```C
rmw_guard_condition_t * rmw_create_guard_condition(void)
```
Create  a new guard condition.

`Return`
Created guard condition pointer.
If error, a NULL pointer is returned.

---
```C
rmw_ret_t rmw_destroy_guard_condition(rmw_guard_condition_t * guard_condition)
```
Destroy a guard condition.

`guard_condition`
Pointer to guard condition to destroy.

`Return`
* RMW_RET_OK
* RMW_RET_ERROR
* RMW_RET_TIMEOUT

---
```C
rmw_ret_t rmw_trigger_guard_condition(const rmw_guard_condition_t * guard_condition)
```
No implemented yet.

---
```C
rmw_wait_set_t * rmw_create_wait_set(size_t max_conditions)
```
Create a new wait set.

`Return`
Created wait set pointer.
If error, a NULL pointer is returned.

---
```C
rmw_ret_t rmw_destroy_wait_set(rmw_wait_set_t * wait_set)
```
Destroy a wait set.

`wait_set`
Pointer to wait set to destroy.

`Return`
* RMW_RET_OK
* RMW_RET_ERROR
* RMW_RET_TIMEOUT

---
```C
rmw_ret_t rmw_wait(rmw_subscriptions_t * subscriptions, rmw_guard_conditions_t * guard_conditions, rmw_services_t * services, rmw_clients_t * clients, rmw_wait_set_t * wait_set, const rmw_time_t * wait_timeout)
```
Waits until one of the given events occurs.

`subscriptions`
Pointer to subscriptions list to wait.
Set to NULL to ignore.

`guard_conditions`
Pointer to guard conditions list to wait.
Set to NULL to ignore.
This parameter not supported yet.

`services`
Pointer to services list to wait.
Set to NULL to ignore.
This parameter not supported yet.

`clients`
Pointer to clients list to wait.
Set to NULL to ignore.
This parameter not supported yet.

`wait_set`
Pointer to wait set list to wait.
Set to NULL to ignore.
This parameter not supported yet.

`wait_timeout`
Max time to wait until a timeout occurs.

`Return`
* RMW_RET_OK
* RMW_RET_ERROR
* RMW_RET_TIMEOUT

---
```C
rmw_ret_t rmw_get_node_names(const rmw_node_t * node, rcutils_string_array_t * node_names)
```
No implemented yet.

---
```C
rmw_ret_t rmw_count_publishers(const rmw_node_t * node, const char * topic_name, size_t * count)
```
No implemented yet.

---
```C
rmw_ret_t rmw_count_subscribers(const rmw_node_t * node, const char * topic_name, size_t * count)
```
No implemented yet.

---
```C
rmw_ret_t rmw_get_gid_for_publisher(const rmw_publisher_t * publisher, rmw_gid_t * gid)
```
No implemented yet.

---
```C
rmw_ret_t rmw_compare_gids_equal(const rmw_gid_t * gid1, const rmw_gid_t * gid2, bool * result)
```
No implemented yet.

---
```C
rmw_ret_t rmw_set_log_severity(rmw_log_severity_t severity)
```
No implemented yet.

---
```C
rmw_ret_t rmw_get_topic_names_and_types(const rmw_node_t * node, rcutils_allocator_t * allocator, bool no_demangle, rmw_names_and_types_t * topic_names_and_types)
```
No implemented yet.

---
```C
rmw_ret_t rmw_get_service_names_and_types(const rmw_node_t * node, rcutils_allocator_t * allocator, rmw_names_and_types_t * service_names_and_types)
```
No implemented yet.
