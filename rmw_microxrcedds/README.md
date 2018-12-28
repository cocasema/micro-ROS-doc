# Overview

micro-ROS provides an RMW implementation using the latest code from eProsima's middleware for constrained devices, Micro XRCE-DDS.
In this version, we have a C implementation for part of the middleware primitives declared by the ROS `rmw` upper layer.

In micro-ROS, `rmw_microxrcedds`, as it is the only rmw_implementation found, will be used as the default one by `rmw_implementation` (`rmw_implementation` defaults the rmw implementation to use as `rmw_fastrps_cpp` in case it is found, otherwise it will use the first available alphabetically sorted). 
This unique implementation avoids the dependency with the third-party library [Poco](https://pocoproject.org/), used for the dynamic load of rmw_implementations.

The `rmw_microxrcedds` library is a ROS2 Middleware Abstraction Interface used by micro-ROS.
This library defines the interface used by upper layers in ROS2 stack and implemented using some middleware in the lower layers.

# Index

1. [Architecture](Architecture.md)
1. [Configuration](Configuration.md)
1. [API guide](API_guide.md)
1. [Roadmap](Roadmap.md)
