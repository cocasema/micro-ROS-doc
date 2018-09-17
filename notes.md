# Notes

## Question

What does the docker run command do?

```bash
docker run -it --privileged --mount type=bind,source="WORKSPACE_PATH",target="/root/ros2_ws/" --net=host microros
```

# Question

I was not able to find `vcs` command. May be it could be useful to tell that python's vcs tool is necessary.
```bash
sudo pip install vcstool
```

## Question

If I pulled boouga's docker then the image is called as the hub (`boouga/bouncy_ros2_build_sources`).

## Question

On the build step I have the following error:
```bash
Duplicate package names not supported:
- micrortps_client:
  - /root/ros2_ws/apps/eprosima/micrortpsclient/micro-RTPS-client
  - /root/ros2_ws/src/eProsima/micrortps-client
```

I had to remove `/root/ros2_ws/apps/eprosima`.

## Question

In which directory I should run the `colcon build` command.

## Question

What is the name of the project: `micro-ROS` or `micro-ROS2`?

## Question

_Micro RTPS Client_ was not able to find _Fast CDR_, I had to compile with `-DTHIRDPARTY=ON` flag.
