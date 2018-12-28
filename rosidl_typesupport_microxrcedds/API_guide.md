# API guide 

## Message type support

The generated message type support has a struct that contains the API used to handle the message serialisation and deserialisation.

---
```C
const char * package_name_
```
Package name where the generated message code belongs.

---
```C
const char * message_name_;
```
Message name

---
```C
bool cdr_serialize(const void * untyped_ros_message, ucdrBuffer * cdr)
```
Serialize the message into a micro cdr buffer.

`untyped_ros_message`
Message pointer to serialize.

`cdr`
Micro buffer.

`return`
True if successful.

---
```C
bool cdr_deserialize(ucdrBuffer * cdr, void * untyped_ros_message, uint8_t * raw_mem_ptr, size_t raw_mem_size)
```
Deserialize the serialized message contained into the micro cdr buffer.

`cdr`
Micro buffer.

`untyped_ros_message`
Message struct pointer where the message will be deserialised.

`raw_mem_ptr` 
Byte memory pointer used to deserialise unbounded message data.
If NULL, unbounded data will be not deserialised.

`raw_mem_size`
Byte memory size.

`return` 
True if successful.

---
```C
uint32_t get_serialized_size(const void *);
```
Get the serialized message size.

`return`
Size.

---
```C
size_t max_serialized_size(bool full_bounded);
```
Get the maximum serialized message size.


## Service type support

The generated service type support has a struct that contains the API used to handle the service.
This API will provide a message type support for the client request message and message type support for the service request message.

---
```C
const char * package_name_;
```
Package name where the generated message code belongs.

---
```C
const char * service_name_;
```
Service name.

---
```C
const rosidl_message_type_support_t * request_members_();
```
Message type support for the request.

`return`
Type support pointer.

---
```C
const rosidl_message_type_support_t * response_members_();
```
Message type support API for the message response.

`return`
Type support pointer.
