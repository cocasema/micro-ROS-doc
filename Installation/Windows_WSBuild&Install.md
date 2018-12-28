# ROS2/Micro-ROS Workspace build and install

1. **Create workspaces folder**

    This step will create two workspaces, one for the client-side and other for the agent-side.
    Client workspace will contain all micro-ROS nodes, and Agent workspace will include all ROS2 nodes.
    For now, we have to keep separate client-side packages from Agent-side packages.

    For client-side workspace create an empty folder, and inside it, you have to create an empty folder for the sources files named 'src'.

    ```bash
    mkdir C:\C
    mkdir C:\C\src
    ```

    For agent-side workspace create an empty folder, and inside it, you have to create an empty folder for the sources files named 'src'.

    ```bash
    mkdir C:\A
    mkdir C:\A\src
    ```

    > **Note:** To avoid the error of routes too long (due to the .NET library), it is advisable to create the working directory with the shortest route possible. This is, the name of the created folder has to be as short as possible, and the path of it has to be as close as possible to the root directory.

1. **Import Micro-ROS packages**

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

1. **Build Micro-ROS**

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

    > **Note:** To avoid the error of routes too long (due to the .NET library), the '--merge-install' flag must be set.


1. **Configure execution environment**

    Once colcon has finished building Micro-ROS, you need to set up the environment, so it finds all the required DLLs to run applications.
    For doing so, colcon has copied for us the following batch file for each workspace.

    ```bash
    C:\C\install\local_setup.bat
    C:\A\install\local_setup.bat
    ```

    The previous batch sets up the environment for the current cmd session.
    
