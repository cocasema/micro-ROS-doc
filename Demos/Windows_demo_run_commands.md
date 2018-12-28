
# Linux demo run commands

## Simple upper bounded publish-subscribe

Run the Micro-ROS Agent.
For the Micro-ROS Agent to find the XML reference file, the execution must be done from the executable folder.

```bash
cd C:\A\install\Lib\uros_agent\
uros_agent.exe udp 8888
```

Run the publisher.

```bash
cd C:\C\install\Lib\int32_publisher_c\
int32_publisher_c.exe
```

Run the subscriber.

```bash
cd C:\C\install\Lib\int32_subscriber_c\
int32_subscriber_c.exe
```

## Simple unbounded publish/subscribe

Run the Micro-ROS Agent.
For the Micro-ROS Agent to find the XML reference file, the execution must be done from the executable folder.

```bash
cd C:\A\install\Lib\uros_agent\
uros_agent.exe udp 8888
```

Run the publisher.

```bash
cd C:\C\install\Lib\string_publisher_c\
string_publisher_c.exe
```

Run the subscriber.

```bash
cd C:\C\install\Lib\string_subscriber_c\
string_subscriber_c.exe
```
## Complex message demonstration

Run the Micro-ROS Agent.
For the Micro-ROS Agent to find the XML reference file, the execution must be done from the executable folder.

```bash
cd C:\A\install\Lib\uros_agent\
uros_agent.exe udp 8888
```

Run the publisher.

```bash
cd C:\C\install\Lib\complex_msg_publisher_c\
complex_msg_publisher_c.exe
```

Run the subscriber.

```bash
cd C:\C\install\Lib\complex_msg_subscriber_c\
complex_msg_subscriber_c.exe
```

## Real application demonstration

### Micro-ROS nodes

Run the Micro-ROS Agent.
For the Micro-ROS Agent to find the XML reference file, the execution must be done from the executable folder.

```bash
cd C:\A\install\Lib\uros_agent\
uros_agent.exe udp 8888
```

Run the altitude_sensor node.

```bash
cd C:\C\install\Lib\rad0_altitude_sensor_c
rad0_altitude_sensor_c.exe
```

Run the actuator node.

```bash
cd C:\C\install\Lib\rad0_actuator_c
rad0_actuator_c.exe
```

Run the display node.

```bash
cd C:\C\install\Lib\rad0_display_c\
rad0_display_c.exe
```

### ROS2 nodes

```bash
cd C:\A\install\Lib\rad0_control_cpp\
rad0_control_cpp.exe
```
