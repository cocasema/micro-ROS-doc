# Jenkins jobs

## Micro-ROS Manual Windows

Runs test on a [windows docker](https://github.com/microROS/docker) container.
The below parameters can configure this job

* AGENT_SET_BRANCHES
* CLIENT_IGNORE_PACKAGE_RESULT
* AGENT_IGNORE_PACKAGE_RESULT
* AGENT_UROS_PATH
* AGENT_PACKAGES_TO_TEST
* CLIENT_SET_BRANCHES
* FEATURE_TO_TEST
* CLIENT_REPO_LIST_URL
* CLIENT_BUILD_SHARED_LIBS
* CLIENT_UROS_PATH
* LIENT_PACKAGES_TO_TEST
* AGENT_REPO_LIST_URL

All parameters are passed directly to the [python test script](Testing_mechanims.md#test-script-parameters).

## Micro-ROS Manual Linux

Runs test on a [Linux ubuntu 16.04 docker](https://github.com/microROS/docker) container.
The below parameters can configure this job

* AGENT_SET_BRANCHES
* CLIENT_IGNORE_PACKAGE_RESULT
* AGENT_IGNORE_PACKAGE_RESULT
* AGENT_UROS_PATH
* AGENT_PACKAGES_TO_TEST
* CLIENT_SET_BRANCHES
* FEATURE_TO_TEST
* CLIENT_REPO_LIST_URL
* CLIENT_BUILD_SHARED_LIBS
* CLIENT_UROS_PATH
* LIENT_PACKAGES_TO_TEST
* AGENT_REPO_LIST_URL

All parameters are passed directly to the [python test script](Testing_mechanims.md#test-script-parameters).

## Micro-ROS Manual

Runs `Micro-ROS Manual Linux` and `Micro-ROS Manual Windows` jobs.
The below parameters can configure this job

* AGENT_SET_BRANCHES
* CLIENT_IGNORE_PACKAGE_RESULT
* AGENT_IGNORE_PACKAGE_RESULT
* AGENT_UROS_PATH
* AGENT_PACKAGES_TO_TEST
* CLIENT_SET_BRANCHES
* FEATURE_TO_TEST
* CLIENT_REPO_LIST_URL
* CLIENT_BUILD_SHARED_LIBS
* CLIENT_UROS_PATH
* LIENT_PACKAGES_TO_TEST
* AGENT_REPO_LIST_URL

All parameters are passed directly to the nested Jenkins jobs.

## Micro-ROS Nightly Master

Runs `Micro-ROS Manual Linux` and `Micro-ROS Manual Windows` jobs switching all Micro-ROS repos into master branch.

## Micro-ROS Nightly Develop

Runs `Micro-ROS Manual Linux` and `Micro-ROS Manual Windows` jobs switching all Micro-ROS repos into develop branch.

## Micro-ROS Launcher

The automatic trigger of `Micro-ROS Nightly Master` and `Micro-ROS Nightly Develop` at 3:00 am every night.
