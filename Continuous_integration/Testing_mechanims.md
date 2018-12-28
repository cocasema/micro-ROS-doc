# Project testing mechanism

All Micro-ROS packages are tested in Windows and Linux environments.
For an easy and shared testing mechanisms, both environments are tested using a python script ([exe_repo_Test.py](https://github.com/qeyup/MicroROS_TestingTools)).
The script will:

1. Download all required repos for the test.
1. Configure all repos for the test. 
1. Execute colcon build.
1. Execute colcon test.
1. Organize test report for an easily readable in the std output.

This script must be executed in a [ROS2 build environment](https://index.ros.org/doc/ros2/Installation/). 
In order to get a clean test execution, all test are run in an isolated docker container.
You can download the Windows and Linux docker images from the github [docker repo](https://github.com/microROS/docker).


## Test script parameters

The test script has the below configurable parameters.

**Test_name**
Name of the test.
This parameter is optional.

*Usage*: --Test_name EXAMPLE

**work_dir**
Specify the workspace directory (src, build and install).
This parameter is optional.
By default is `HOME/ros2_ws`.
This parameter is optional.

*Usage*: --work_dir /home/Test

**file**
Specify the [YAML](http://yaml.org/) file that [vcstool](https://github.com/dirk-thomas/vcstool) will use to [import](https://github.com/dirk-thomas/vcstool#import-set-of-repositories) the testing repos.
This parameter is optional.

*Usage*: --file/home/Test/uros.repos

**url**
Specify the URL of the [YAML](http://yaml.org/) file that [vcstool](https://github.com/dirk-thomas/vcstool) will use to [import](https://github.com/dirk-thomas/vcstool#import-set-of-repositories) the testing repos.
This parameter is mandatory if not file parameter is not given.

*usage*: --url http://URL

**feature_to_test**
Specify the features to test.
All packages that have a branch with the feature name will be switched.
You can specify more than one features.
In case that one package has two or more specified features, just the last one will be used for that package. 
This parameter is optional.

*usage*: --feature_to_test feature/NEW_FEATURE_1 feature/NEW_FEATURE_2 ...

**branch**
Specify testing branches of the repos that you want to switch in the downloaded repo files before executed the test.
This action will be executed after feature_to_test switch.
This parameter is optional.

*Usage*: --branch target_package_1:branch_to_swith Target_package_2:branch_to_swith ...

**packages_to_test**
Specify the packages to be tested after build process success. 
If not defined, all packages will be tested.
This parameter is optional.

*Usage*: --packages_to_test PACKAGE_1 PACKAGE_2 ...

**Ignore_package_result**
Specify the packages to ignore that returned an error after test execution.
The test report will be displayed, but the test script will not return an error.
This parameter is optional.

*Usage*: --Ignore_package_result PACKAGE_1 PACKAGE_2 ...

**scope_folder**
Specify the folder (starting from where the repo source will be downloaded) where `--feature_to_test` and `--branch` will be applied.
By default `--feature_to_test` and `--branch` will be applied on all repos.
This parameter is optional.

*Usage*: --scope_folder ros2

**skip_build**
Add this flag to skip the building step.
Use it to reduce the script time in case you are testing again the execution and the build step was a success.
This parameter is optional.

*Usage*: --skip_build

**skip_test**
Add this flag to skip the colcon test step.
This parameter is optional.

*Usage*: --skip_test

**skip_download**
Add this flag to skip the download repo step.
Use it in case you already have the repo files downloaded.
This parameter is optional.

*Usage*: --skip_download

**Skip_cleaning_ws**
Add this to avoid to clean the workspace before executing the test.
This parameter is optional.

*Usage*: --Skip_cleaning_ws

**Build_extra_args**
Pass extra arguments to the colcon build process.
Because the python parser may conflict with the colcon build arguments, replace the "-" characters with "+" and internally will be changed to "-".
This parameter is optional.

*Usage* --Build_extra_args ++merge+install

**Test_extra_args**
Pass extra arguments to the colcon test process.
Because the python parser may conflict with the colcon build arguments, replace the "-" characters with "+" and internally will be changed to "-".
This parameter is optional.

*Usage* --Build_extra_args ++merge+install

**Exec_command_after_build**
Execute command after colcon build step success.
The command will not block as it will be executed in the background.
This parameter is optional.

*Usage*: --Exec_command_after_build EXECUTE_PROCESS


**Exec_command_after_test**
Execute command after colcon test step.
The command will not block as it will be executed in the background.
This parameter is optional.

Usage: --Exec_command_after_build STOP_EXECUTED_PROCESS

**Exec_wait_time**
Time (in seconds) that the script will wait to the command execution in order to know if the execution still running or not.
This parameter is optional.

Usage: --Exec_wait_time 30
