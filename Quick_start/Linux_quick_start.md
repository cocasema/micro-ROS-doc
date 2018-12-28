# Linux quick start

All steps are executed in a Linux terminal.

1. **Build Linux docker image**

    ```shell
    sudo docker pull microros/linux
    ```
     >**Note:** For this step superuser privileges are required.

1. **Run docker container for agent-side applications**

    ```shell
    sudo docker run -it --rm --name agent_docker ros2
    ```
     >**Note:** For this step superuser privileges are required.*

1. **Download Agent-side repos**

    ```shell
    mkdir -p $HOME/uros_ws/src
    cd $HOME/uros_ws
    wget https://raw.githubusercontent.com/microROS/micro-ROS-doc/master/repos/agent_minimum.repos -O uros.repos
    vcs-import src < uros.repos
    ```

1. **Compile Agent-side**

    ```shell
    colcon build
    ```

1. **Run Micro-ROS Agent**

    ```shell
    cd $HOME/uros_ws/install/uros_agent/lib/uros_agent/
    uros_agent udp 8888
    ```

1. **Run docker container for client-side applications**

    ```shell
    sudo docker run -it --rm --name client_docker ros2
    ```
     >**Note:** For this step superuser privileges are required.

1. **Download Client-side repos**

    ```shell
    mkdir -p $HOME/uros_ws/src
    cd $HOME/uros_ws
    wget https://raw.githubusercontent.com/microROS/micro-ROS-doc/master/repos/client_minimum.repos -O uros.repos
    vcs-import src < uros.repos
    ```

1. **Compile Client-side**

    ```shell
    colcon build --cmake-args -DBUILD_SHARED_LIBS=ON
    ```

1. **Run publisher**

    ```shell
    cd $HOME/uros_ws/install/string_publisher_c/lib/string_publisher_c
    ./string_publisher_c
    ```

1. **Open a new terminal in the running client-side docker container**

    ```shell
    sudo docker  exec -it client_docker bash
    ```
     >**Note:** For this step superuser privileges are required.*

1. **Run subscription**

    ```shell
    cd $HOME/uros_ws/install/string_subscriber_c/lib/string_subscriber_c
    ./string_subscriber_c
    ```

After this steps you will see how publisher and subscriber share messages.
You may open as many publisher or/and subscriber as you want.
