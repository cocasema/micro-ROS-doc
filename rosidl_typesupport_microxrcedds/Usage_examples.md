# Usage examples

In this section, an example of how to generate a simple message will be described.


## package.xml file
The [ROS2 manifest](https://wiki.ros.org/Manifest) must have the below dependencies:
* Must be `member_of_group` of `rosidl_interface_packages` group.
* Is `buildtool_depend` dependent of `rosidl_default_generators`

```xml
...
<buildtool_depend>rosidl_default_generators</buildtool_depend>
<member_of_group>rosidl_interface_packages</member_of_group>
...
```

## Message.msg file
For this example, the [ROS2 message](https://wiki.ros.org/msg) will be as described below.

```
int32 data
```

## CMakeList.txt file

Find the rosidl_default_generators package with REQUIRED rule.

```
find_package(rosidl_default_generators REQUIRED)
```

Call the `rosidl_generate_interfaces` macro giving the message relative path, the message dependencies and the name of the generation target.

```
rosidl_generate_interfaces(
    ${PROJECT_NAME}
    "Message.msg"
    DEPENDENCIES "builtin_interfaces"
    ADD_LINTER_TESTS
)
```
