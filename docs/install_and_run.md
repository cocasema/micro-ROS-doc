# Micro-ROS Setup

## Overview

This guide provides you with a quick summary of how to install and configure the ROS2 environment for a micro-ROS workspace.


## Windows

### Step 1: Set-up ROS2 environment on Windows

The development environment for micro-ROS in Windows platforms is the same as the one for [ROS2](https://github.com/2/ros2/wiki), so the same set-up is needed.
You can follow the [ROS2 installation guide for windows.](https://index.ros.org/doc/ros2/Windows-Install-Binary/)


#### Using python virtual environments to isolate ROS2 environment

To avoid polluting your environment, you can set up a python virtualenv with all the tools needed for building micro-ROS.
For doing so you need to install [virtualenv](https://virtualenv.pypa.io/en/stable/) (assuming you have already installed [python](https://www.python.org/) and [pip](https://pip.pypa.io/en/stable/)).

```bash
pip install virtualenv
virtualenv VIRTUAL_ENV_LOCAL_DIRECTORY
VIRTUAL_ENV_LOCAL_DIRECTORY\Scripts\activate
pip install -U vcstool
pip install -U colcon-common-extensions
```

Once you execute the previous commands, you should have a python virtual env configured for micro-ROS compilation.
This python virtualenv resides in the given directory VIRTUAL_ENV_LOCAL_DIRECTORY.

Each time you want to work on micro-ROS, you merely need to run ```VIRTUAL_ENV_LOCAL_DIRECTORY\Scripts\activate``` and then the following commands you issue run using the virtualenv tools.

For shutting down an activate python virtual env.


#### Alternative: Using docker container

As an alternative to the installation of the ROS2 environment on windows, the user can download and compile a [docker](https://docs.docker.com/docker-for-windows/) image from [here](PENDING..).

```bash
docker build -t ros2 DOCKER_FILE_PATH
```

### Step 2: create work spaces

This step will create two workspaces. 
One for the client-side and other for the agent-side.
Client workspace will contain all Micro nodes and Agent workspace will contain all ROS2 nodes.
For now, we have to keep separate client-side packages from Agent-side packages.

For client-side workspace create an empty folder and inside it, you have to create an empty folder for the sources files named 'src'.

```bash
mkdir C:\C
mkdir C:\C\src
```

For agent-side workspace create an empty folder and inside it, you have to create an empty folder for the sources files named 'src'.

```bash
mkdir C:\A
mkdir C:\A\src
```

`Note:` To avoid the error of routes too long (due to the .NET library), it is advisable to create the working directory with the shortest route possible. This is, the name of the created folder has to be as short as possible and the path of it has to be as close as possible to the root directory.


### Step 3: import micro-ROS packages on Windows

For using micro-ROS, you need a predefined group of ROS2 packages.
A [YAML](http://yaml.org/) file for [vcstool](https://github.com/dirk-thomas/vcstool) is provided to sync all the needed sources easily.

Download all client-side repo package list and place it in the client workspace.

```bash
cd C:\C
curl --basic https://raw.githubusercontent.com/microROS/micro-ROS-doc/feature/repos/repos/uros_minimum.repos > micro_ros.repos
```

Then, clone all repos.

```bash
cd C:\C
vcs-import src < micro_ros.repos
```

Download all agent-side repo package list and place it in the client workspace.

```bash
cd C:\A
curl --basic https://raw.githubusercontent.com/microROS/micro-ROS-doc/feature/repos/repos/uros_minimum.repos > micro_ros.repos
```

Then, clone all repos.

```bash
cd C:\A
vcs-import src < micro_ros.repos
```


### Step 4: Build micro-ROS on Windows

In windows, you need to run [colcon](https://colcon.readthedocs.io/en/released/) build command inside your Visual Studio command prompt ("x64 Native Tools Command Prompt for VS 2017" in our case)

Build all packages for the client workspace.

```bash
cd C:\C
colcon build --cmake-args -DBUILD_SHARED_LIBS=ON --merge-install
```

Build all packages for the agent workspace.

```bash
cd C:\A
colcon build --cmake-args -DBUILD_SHARED_LIBS=ON --merge-install
```

`Note:` To avoid the error of routes too long (due to the .NET library), the '--merge-install' flag must be set. 


### Step 5: Configure execution environment on Windows

Once colcon has finished building micro-ROS, you need to set up the environment, so it finds all the required DLLs to run applications.
For doing so, colcon has copied for us the following batch file for each workspace.

```bash
C:\C\install\local_setup.bat
C:\A\install\local_setup.bat
```

The previous batch, sets up the environment for the current cmd session.
From now on we can run our [sample nodes](https://github.com/microROS/micro-ROS-demos/blob/master/README.md).

## Linux Debian

### Step 1: Set-up ROS2 environment on Windows

The development environment for micro-ROS in Windows platforms is the same as the one for [ROS2](https://github.com/ros2/ros2/wiki), so the same set-up is needed.
You can follow the [ROS2 installation guide for Linux Debian.](https://index.ros.org/doc/ros2/Linux-Install-Debians/)

#### Alternative: using docker container

As an alternative to the installation of the ROS2 environment on Linux Debian, the user can download and compile a [docker](https://docs.docker.com/install/linux/docker-ce/ubuntu/) image from [here](PENDING..).

```bash
docker build -t ros2 DOCKER_FILE_PATH
```

### Step 2: create work space

This step will create two workspaces. 
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

### Step 3: import micro-ROS packages on Linux Debian

For using micro-ROS, You need a predefined group of ROS2 packages.
A [YAML](http://yaml.org/) file for [vcstool](https://github.com/dirk-thomas/vcstool) is provided to sync all the needed sources easily.

For this step you have to download a basic group of packages from [here](https://raw.githubusercontent.com/microROS/micro-ROS-doc/feature/repos/repos/uros_minimum.repos). 
Rename de downloaded file as 'micro_ros.repo' and place it in the same folder where you want to clone all the repositories.


Download all client-side repo package list and place it in the client workspace.

```bash
cd ~/client_ws
wget https://raw.githubusercontent.com/microROS/micro-ROS-doc/feature/repos/repos/uros_minimum.repos -O micro_ros.repos
```

Then, clone all repos.

```bash
cd ~/client_ws
vcs-import src < micro_ros.repos
```

Download all agent-side repo package list and place it in the client workspace.

```bash
cd ~/agent_ws
wget https://raw.githubusercontent.com/microROS/micro-ROS-doc/feature/repos/repos/uros_minimum.repos -O micro_ros.repos
```

Then, clone all repos.

```bash
cd ~/agent_ws
vcs-import src < micro_ros.repos
```


### Step 4: Build micro-ROS on Linux Debian

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


### Step 5: Configure execution environment on Windows

Once colcon has finished building micro-ROS, you need to set up the environment, so it finds all the required .os to run applications.
For doing so, colcon has copied for us the following batch files.

```bash
. ~/client_ws/install/./local_setup.bash
. ~/agent_ws/install/./local_setup.bash
```

The previous batch, sets up the environment for the current shell session.
From now on we can run our [sample nodes](https://github.com/microROS/micro-ROS-demos/blob/master/README.md).
