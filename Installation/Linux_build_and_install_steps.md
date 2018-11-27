# Linux build and install steps

## Step 1: Set-up ROS2 development environment

The development environment for Micro-ROS in Windows platforms is the same as the one for [ROS2](https://github.com/ros2/ros2/wiki), so the same set-up is needed.
You can follow the [ROS2 installation guide for Linux.](https://index.ros.org/doc/ros2/Linux-Install-Debians/)

### Alternative: Using docker container to isolate ROS2 development environment

As an alternative to the installation of the ROS2 environment on Linux , the user can download and compile a [docker](https://docs.docker.com/install/linux/docker-ce/ubuntu/) image from [here](https://github.com/microROS/docker).

```bash
docker build -t ros2 DOCKER_FILE_PATH
```

## Step 2: Create work space

This step will create two work-spaces.
One for the client-side and other for the agent-side.
Client workspace will contain all Micro nodes and Agent workspace will contain all ROS2 nodes.
For now, we have to keep separate client-side packages from Agent-side packages.

For client-side workspace create an empty folder and inside it, you have to create an empty folder for the sources files named 'src'.

```bash
mkdir ~/client_ws
mkdir ~/client_ws/src
```

For agent-side workspace create an empty folder and inside it, you have to create an empty folder for the sources files named 'src'.

```bash
mkdir ~/agent_ws
mkdir ~/agent_ws/src
```

## Step 3: Import Micro-ROS packages

For using Micro-ROS, You need a predefined group of ROS2 packages.
A [YAML](http://yaml.org/) file for [vcstool](https://github.com/dirk-thomas/vcstool) is provided to sync all the needed sources easily.

For this step you have to download a basic group of packages from.

Download all client-side repo package list and place it in the client workspace.

```bash
cd ~/client_ws
wget https://raw.githubusercontent.com/microROS/micro-ROS-doc/master/Installation/repos/client_minimum.repos -O micro_ros.repos
```

Then, clone all repos.

```bash
cd ~/client_ws
vcs-import src < micro_ros.repos
```

Download all agent-side repo package list and place it in the client workspace.

```bash
cd ~/agent_ws
wget https://raw.githubusercontent.com/microROS/micro-ROS-doc/master/Installation/repos/agent_minimum.repos -O micro_ros.repos
```

Then, clone all repos.

```bash
cd ~/agent_ws
vcs-import src < micro_ros.repos
```


## Step 4: Build Micro-ROS

Build all packages for the client workspace.

```bash
cd ~/client_ws
colcon build --cmake-args -DBUILD_SHARED_LIBS=ON
```

Build all packages for the agent workspace.

```bash
cd ~/agent_ws
colcon build --cmake-args -DBUILD_SHARED_LIBS=ON
```


## Step 5: Configure execution environment

Once colcon has finished building Micro-ROS, you need to set up the environment, so it finds all the required .os to run applications.
For doing so, colcon has copied for us the following batch files.

```bash
. ~/client_ws/install/./local_setup.bash
. ~/agent_ws/install/./local_setup.bash
```

The previous batch, sets up the environment for the current shell session.
From now on we can run our [sample nodes](https://github.com/microROS/micro-ROS-demos/blob/master/README.md).
