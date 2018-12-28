# Type support system

In Micro-ROS, as in regular ROS2, message types are defined using .msg files.
During Micro-ROS compilation the .msg files are used to generate a type support library with all the needed code to create, serialise and deserialise message types.

This process is automatic, and the only user interaction is for creating the message definition files and for the use of the generated code and utilities provided by rclc.

# Index

1. [Architecture](Architecture.md)
1. [API guide](API_guide.md)
1. [Code generation](Code_generation.md)
1. [Usage examples](Usage_examples.md)
1. [Roadmap](Roadmap.md)
