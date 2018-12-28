# Usage example

This section describes an example of how to make a simple publisher and subscriber.

## Publisher

**Step 1.** 
Init the rclc layer.

```C
size_t args_count = 0;
void * args_pointer = NULL;
rclc_ret_t init_response;
init_response = rclc_init(args_count, args_pointer);
if (init_response != RCL_RET_OK)
{
    // Error
}
```

**Step 2**
Create a new node.

```C
const char * node_name = "Example_publisher"
const char * node_namespace = ""
rclc_node_t* node     = rclc_create_node(node_name, node_namespace);
```

**Step 3**
Get the message type support for an int32 message.

```C
const rclc_message_type_support_t type_support;
type_support = RCLC_GET_MSG_TYPE_SUPPORT(std_msgs, msg, Int32)
```

**Step 4**
Create a new publisher in the previously-created node.

```C
const char* topic_name = "publish_example"
rclc_publisher_t* publisher;
publisher = rclc_create_publisher(node, type_support, topic_name, 1);
if (publisher == NULL)
{
    // Error
}
```

**Step 5**
Publish each second.

```C
std_msgs__msg__Int32 msg;
msg.data = 0;
rclc_ret_t publish_response;
 while (rclc_ok())
{   
    publish_response = rclc_publish(publisher, (const void*)&msg);
    if (publish_response != RCL_RET_OK)
    {
        // Error
    }
    rclc_spin_node_once(node, 1000);
}
```

## Subscriber

**Step 1.** 
Init the rclc layer.

```C
size_t args_count = 0;
void * args_pointer = NULL;
rclc_ret_t init_response;
init_response = rclc_init(args_count, args_pointer);
if (init_response != RCL_RET_OK)
{
    // Error
}
```

**Step 2**
Create a new node.

```C
const char * node_name = "Example_subscriber"
const char * node_namespace = ""
rclc_node_t* node     = rclc_create_node(node_name, node_namespace);
```

**Step 3**
Get the message type support for an int32 message.

```C
const rclc_message_type_support_t type_support;
type_support = RCLC_GET_MSG_TYPE_SUPPORT(std_msgs, msg, Int32)
```

**Step 4**
Create a new subscription in the created node.

```C
const char* topic_name = "subscriber_example"
rclc_subscription_t* subscription;
subscription = rclc_create_subscription(node, type_support, topic_name, on_message_callback, 1, false);
if (subscription == NULL)
{
    // Error
}
```

`Note:` 
The callback function must be defined following the underneath signature.
This function will be called every time a new message arrives.
```C
void on_message_callback(const void* msgin)
{
    const std_msgs__msg__Int32* msg = (const std_msgs__msg__Int32*)msgin;
    // message usage
}
```

**Step 5**
Spin the node.

```C
rclc_spin_node(node);
```
