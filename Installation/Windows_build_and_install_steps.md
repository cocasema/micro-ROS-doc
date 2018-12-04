# Windows build and install steps

## Step 1 (Option A): Set-up ROS2 development environment

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


## Step 1 (Option B): Using docker container with a ROS2 development environment.

As an alternative to the installation of the ROS2 development environment on Windows, the user can download and compile a [docker](https://docs.docker.com/docker-for-windows/) image from [here](https://github.com/microROS/docker).
Docker container offers an isolated ROS2 development environment.

```bash
docker build -t ros2 DOCKER_FILE_PATH
```

## Step 2: Create work spaces folder

This step will create two work-spaces.
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


## Step 3: Import Micro-ROS packages

For using Micro-ROS, you need a predefined group of ROS2 packages.
A [YAML](http://yaml.org/) file for [vcstool](https://github.com/dirk-thomas/vcstool) is provided to sync all the needed sources easily.

Download all client-side repo package list and place it in the client workspace.

```bash
cd C:\C
curl --basic https://raw.githubusercontent.com/microROS/micro-ROS-doc/master/repos/client_minimum.repos > micro_ros.repos
```

Then, clone all repos.

```bash
cd C:\C
vcs-import src < micro_ros.repos
```

Download all agent-side repo package list and place it in the client workspace.

```bash
cd C:\A
curl --basic https://raw.githubusercontent.com/microROS/micro-ROS-doc/master/repos/agent_minimum.repos > micro_ros.repos
```

Then, clone all repos.

```bash
cd C:\A
vcs-import src < micro_ros.repos
```

## Step 4: Build Micro-ROS

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


## Step 5: Configure execution environment

Once colcon has finished building Micro-ROS, you need to set up the environment, so it finds all the required DLLs to run applications.
For doing so, colcon has copied for us the following batch file for each workspace.

```bash
C:\C\install\local_setup.bat
C:\A\install\local_setup.bat
```

The previous batch, sets up the environment for the current cmd session.