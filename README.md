# Gazebo RealSense Plugin #

[![Coverity Scan Build Status](https://scan.coverity.com/projects/11940/badge.svg "Coverity Scan Build Status")](https://scan.coverity.com/projects/01org-gazebo-realsense)

A RealSense Camera Gazebo plugin.

This Gazebo plugin simulates a RealSense camera by publishing the 4 main
RealSense streams: Depth, Infrared, Infrared2 and Color. It is associated to a
RealSense model that is providade in ./models.

## Requirements ##

```text
Gazebo 8.0+
```

## Build ##

Create a build folder and make using CMAKE as follows:

```bash
mkdir build
cd build
cmake \
    -DGZRS_PLUGIN_INSTALL_PATH=/usr/local/gazebo/lib64/gazebo-11/plugins \
    -DGZRS_MODEL_INSTALL_PATH=/usr/local/gazebo/share/gazebo-11/models ..
make
```

## Install ##

The plugin binaries will be installed so that Gazebo finds them. Also the
needed models will be copied to the default gazebo models folder.

```bash
make install
```

## Run ##

```bash
gazebo --verbose
```

Insert the realsense model into the simulation.

- Click on the INSERT pane, select the realsense model and move it to the world.

## Interaction ##

The Plugin publishes the simulated streams to ImageStamped topics.
They can be inspected with the Gazebo Topic Viewer (Window->Topic Visualization) under ~/realsense/rs/stream/TOPIC_NAME.
