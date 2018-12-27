# Windows quick start

All steps are executed in a Windows cmd terminal.

1. **Build Linux docker image**

    ```cmd
    md %HOME%\Download\MicroROS_Docker
    cd %HOME%\Download\MicroROS_Docker
    curl --basic https://raw.githubusercontent.com/microROS/docker/master/windows/Dockerfile > Dockerfile
    docker build -t ros2 .
    ```
     >**Note:** If you dont have curl installed, you may [download](https://github.com/microROS/docker/tree/feature/micro-ROS/windows) the Dockerfile directly from the GitHub, place it in %HOME%\Download\MicroROS_Docker and execute the docker build.

1. **Run docker container for agent-side applications**

    ```cmd
    docker run -it --rm --name agent_docker ros2
    ```

1. **Download Agent-side repos**

    ```cmd
    md C:\A
    cd C:\A
    curl --basic https://raw.githubusercontent.com/microROS/micro-ROS-doc/master/repos/agent_minimum.repos > uros.repos
    vcs-import src < uros.repos
    ```

1. **Compile Agent-side**

    ```cmd
    colcon build
    ```

1. **Run Micro-ROS Agent**

    ```cmd
    cd C:\A\install\Lib\uros_agent\
    uros_agent.exe udp 8888
    ```

1. **Run docker container for client-side applications**

    ```cmd
    sudo docker run -it --rm --name client_docker ros2
    ```

1. **Download Client-side repos**

    ```cmd
    md C:\C\src
    cd C:\C
    curl --basic https://raw.githubusercontent.com/microROS/micro-ROS-doc/master/repos/client_minimum.repos > uros.repos
    vcs-import src < uros.repos
    ```

1. **Compile Client-side**

    ```cmd
    colcon build --cmake-args -DBUILD_SHARED_LIBS=ON
    ```

1. **Run publisher**

    ```cmd
    cd C:\C\install\Lib\string_publisher_c\
    string_publisher_c.exe
    ```

1. **Open new terminar in the running client-side docker container**

    ```cmd
    sudo docker  exec -it client_docker bash
    ```

1. **Run subscription**

    ```cmd
    cd C:\C\install\Lib\string_subscriber_c\
    string_subscriber_c.exe
    ```


After this steps you will see how publisher and subscriber share messages.
You may open as many publisher or/and subscriber as you want.
