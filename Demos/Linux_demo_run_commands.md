# Linux demo run commands

## Simple upper bounded publish-subscribe

Run the Micro-ROS Agent.

`Note:` For the Micro-ROS Agent to find the XML reference file, the execution must be done from the executable folder.

```bash
cd ~/agent_ws/install/uros_agent/lib/uros_agent/
./uros_agent udp 8888
```

You may prefer to run the Agent in the background and discard all outputs to keep using the same terminal for the next step.

```bash
cd ~/agent_ws/install/uros_agent/lib/uros_agent/
./uros_agent udp 8888 > /dev/null &
```

Run the publisher.

```bash
~/client_ws/install/int32_publisher_c/lib/int32_publisher_c/./int32_publisher_c
```

You may prefer to run the publisher in the background and discard all outputs to keep using the terminal for the next step.

```bash
 ~/client_ws/install/int32_publisher_c/lib/int32_publisher_c/./int32_publisher_c > /dev/null &
```

Run the subscriber.

```bash
~/client_ws/install/int32_subscriber_c/lib/int32_subscriber_c/./int32_subscriber_c
```

## Simple unbounded publish/subscribe

Run the Micro-ROS Agent.
For the Micro-ROS Agent to find the XML reference file, the execution must be done from the executable folder.

```bash
cd ~/agent_ws/install/uros_agent/lib/uros_agent/
./uros_agent udp 8888
```

You may prefer to run the Agent in the background and discard all outputs to keep using the same terminal for the next step.

```bash
cd ~/agent_ws/install/uros_agent/lib/uros_agent/
./uros_agent udp 8888 > /dev/null &
```

Run the publisher.

```bash
 ~/client_ws/install/string_publisher_c/lib/string_publisher_c/./string_publisher_c
```

You may prefer to run the publisher in the background and discard all outputs in order to keep using the terminal for the next step.

```bash
 ~/client_ws/install/string_publisher_c/lib/string_publisher_c/./string_publisher_c > /dev/null &
```

Run the subscriber.

```bash
~/client_ws/install/string_subscriber_c/lib/string_subscriber_c/./string_subscriber_c
```

## Complex message demonstration

Run the Micro-ROS Agent

```bash
cd ~/agent_ws/install/uros_agent/lib/uros_agent/
./uros_agent udp 8888
```

You may prefer to run the Agent in the background and discard all outputs to keep using the same terminal for the next step.

```bash
cd ~/agent_ws/install/uros_agent/lib/uros_agent/
./uros_agent udp 8888 > /dev/null &
```

Run the publisher.

```bash
 ~/client_ws/install/complex_msg_publisher_c/lib/complex_msg_publisher_c/./complex_msg_publisher_c
```

You may prefer to run the Agent in the background and discard all outputs to keep using the same terminal for the next step.

```bash
 ~/client_ws/install/complex_msg_publisher_c/lib/complex_msg_publisher_c/./complex_msg_publisher_c > /dev/null &
```

Run the subscriber.

```bash
~/client_ws/install/complex_msg_subscriber_c/lib/complex_msg_subscriber_c/./complex_msg_subscriber_c
```

## Real application demonstration

### Micro-ROS nodes

Run the Micro-ROS Agent.
For the Micro-ROS Agent to find the XML reference file, the execution must be done from the executable folder.

```bash
cd ~/uros_WS/install/uros_agent/lib/uros_agent/
./uros_agent udp 8888
```

You may prefer to run the Agent in the background and discard all outputs to keep using the same terminal for the next step.

```bash
cd ~/uros_WS/install/uros_agent/lib/uros_agent/
./uros_agent udp 8888 > /dev/null &
```

Run the altitude_sensor node.

```bash
~/client_ws/install/rad0_altitude_sensor_c/lib/rad0_altitude_sensor_c/./rad0_altitude_sensor_c
```

You may prefer to run the publisher in the background and discard all outputs to keep using the terminal for the next steps.

```bash
~/client_ws/install/rad0_altitude_sensor_c/lib/rad0_altitude_sensor_c/./rad0_altitude_sensor_c > /dev/null &
```

Run the actuator node.

```bash
 ~/client_ws/install/rad0_actuator_c/lib/rad0_actuator_c/./rad0_actuator_c
```

You may prefer to run the publisher in the background and discard all outputs to keep using the terminal for the next steps.

```bash
 ~/client_ws/install/rad0_actuator_c/lib/rad0_actuator_c/./rad0_actuator_c > /dev/null &
```

Run the display node.

```bash
~/client_ws/install/rad0_display_c/lib/rad0_display_c/./rad0_display_c
```

##### ROS2 nodes

```bash
~/agent_ws/install/rad0_display_c/lib/rad0_display_c/./rad0_display_c
```
