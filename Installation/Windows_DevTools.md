# Development tools installation

## Option A: Host system installation

The development environment for Micro-ROS in Windows platforms is the same as the one for [ROS2](https://github.com/2/ros2/wiki), so the same set-up is needed.
You can follow the [ROS2 installation guide for windows.](https://index.ros.org/doc/ros2/Windows-Install-Binary/)


### Using python virtual environments to isolate ROS2 development environment

To avoid polluting your environment, you can set up a python virtualenv with all the tools needed for building Micro-ROS.
For doing so you need to install [virtualenv](https://virtualenv.pypa.io/en/stable/) (assuming you have already installed [python](https://www.python.org/) and [pip](https://pip.pypa.io/en/stable/)).

```bash
pip install virtualenv
virtualenv VIRTUAL_ENV_LOCAL_DIRECTORY
VIRTUAL_ENV_LOCAL_DIRECTORY\Scripts\activate
pip install -U vcstool
pip install -U colcon-common-extensions
```

Once you execute the previous commands, you should have a python virtual env configured for Micro-ROS compilation.
This python virtualenv resides in the given directory VIRTUAL_ENV_LOCAL_DIRECTORY.

Each time you want to work on Micro-ROS, you merely need to run ```VIRTUAL_ENV_LOCAL_DIRECTORY\Scripts\activate``` and then the following commands you issue run using the virtualenv tools.

For shutting down an activate python virtual env.


## Option B: Using docker image with development tools installed.

As an alternative to the installation of the ROS2 development environment on Windows, the user can download and build a [docker](https://docs.docker.com/docker-for-windows/) image from [here](https://github.com/microROS/docker).
Docker container offers an isolated ROS2 development environment.

```bash
docker build -t ros2 DOCKER_FILE_PATH
```

> **Note:** The only requirement for use docker images is to have installed [docker CE](https://docs.docker.com/install/).

> **Note:** Docker images built over latest windows images are available.
