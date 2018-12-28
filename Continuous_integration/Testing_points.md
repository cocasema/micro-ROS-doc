# Project testing points

In this section, all test points that are checked each time a CI execution runs will be enumerated.

## YAML file

* URLs for the repositories indicated the in the YAML file will be tested by downloading them.

* All released YAMLs files will be tested.

## Build 

* All packages included in the YAML files will be built with the colcon tool and all must be built successfully.

* All packages will be built successfully with the BUILD_SHARED_LIBS mode activated.

* Build compatibility with other packages will be tested during the build process.

## Integration and unit test

* Integration test will be executed and any issue will be logged.

* Unit test for each package will be executed any issue will be logged.

`Note:` Pre-known integration test failures are allowed as the project is in a development phase and not all features are already developed.

## Source code format and style

* All packages must have no code style divergences.

* All packages must comply with the ROS2 uncrustify rules.

* All packages must comply with the ROS2 lint_cpp rules.

* All packages must comply with the ROS2 lint_cmake rules.

* All code must have copyright file.
