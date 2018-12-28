# Local test execution

To verify newly developed features in local before uploading changes, [bash script](https://github.com/qeyup/MicroROS_TestingTools) (for Linux) and [bat script](https://github.com/qeyup/MicroROS_TestingTools) (for Windows) has been uploaded.
These script download, parameterize and execute the same [testing python script](Testing_mechanims.md) that Jenkins.
The local script must be executed in a [ROS2 build environment](https://index.ros.org/doc/ros2/Installation/). 
In order to get a clean test execution, is recommended to use the docker containers with the ROS2 environment installed.
You can download the Windows and Linux docker images from the github [docker repo](https://github.com/microROS/docker).


## Local execution in Linux

In a [ROS2 build environment](https://index.ros.org/doc/ros2/Installation/) execute the below steps:

**Note:** *In case you are going to use a docker container*
```shell
sudo docker run -it ROS2_IMAGE_TAG
```

**Step 1. Download de bash script**

```shell
wget https://raw.githubusercontent.com/qeyup/MicroROS_TestingTools/master/Jenkins_local.bash -O Jenkins_local.bash 
```

**Step 2. Open the script and edit the parameters according to the feature test**

For this step use your favorite text editor.

```
FEATURE_TO_TEST=""

AGENT_REPO_LIST_URL="https://raw.githubusercontent.com/microROS/micro-ROS-doc/master/repos/agent_minimum.repos"
AGENT_UROS_PATH="uros"
AGENT_PACKAGES_TO_TEST=""
AGENT_IGNORE_PACKAGE_RESULT=""
AGENT_SET_BRANCHES=""

CLIENT_REPO_LIST_URL="https://raw.githubusercontent.com/microROS/micro-ROS-doc/master/repos/client_minimum.repos"
CLIENT_UROS_PATH="uros"
CLIENT_PACKAGES_TO_TEST=""
CLIENT_IGNORE_PACKAGE_RESULT=""
CLIENT_SET_BRANCHES=""
CLIENT_BUILD_SHARED_LIBS="ON"
```


**Step 3. Execute test**
```shell
./Jenkins_local.bash 
```

## Local execution in Windows

In a [ROS2 build environment](https://index.ros.org/doc/ros2/Installation/) execute the below steps:

**Note:** *In case you are going to use a docker container*
```shell
sudo docker run -it ROS2_IMAGE_TAG
```

**Step 1. Download de bash script**

```shell
curl https://raw.githubusercontent.com/qeyup/MicroROS_TestingTools/master/Jenkins_local.bat -o Jenkins_local.bat 
```

**Step 2. Open the script and edit the parameters according to the feature test**

For this step use your favorite text editor.

```
set FEATURE_TO_TEST=""

set AGENT_REPO_LIST_URL="https://raw.githubusercontent.com/microROS/micro-ROS-doc/master/repos/agent_minimum.repos"
set AGENT_UROS_PATH="uros"
set AGENT_PACKAGES_TO_TEST=""
set AGENT_IGNORE_PACKAGE_RESULT=""
set AGENT_SET_BRANCHES=""

set CLIENT_REPO_LIST_URL="https://raw.githubusercontent.com/microROS/micro-ROS-doc/master/repos/client_minimum.repos"
set CLIENT_UROS_PATH="uros"
set CLIENT_PACKAGES_TO_TEST=""
set CLIENT_IGNORE_PACKAGE_RESULT=""
set CLIENT_SET_BRANCHES=""
set CLIENT_BUILD_SHARED_LIBS="ON"
```


**Step 3. Execute test**
```shell
Jenkins_local.bat 
```

