# fs_mod_ros

## Overview

This repository provides launch, configuration and mesh files for a navstack demo with [FS19_modROS](https://github.com/tud-cor/FS19_modROS).  

## Requirements

* ROS Melodic

`roscore` is run on a Linux host.

Python 3 is not a requirement on the Linux side.

ROS Noetic may work, but has not been tested by the authors.


## Building

The following instructions assume you'll clone the repository into your `$HOME` directory.
If that's not the case, adjust paths where necessary.

```sh
# create a Catkin workspace
mkdir -p $HOME/catkin_fs/src

# clone fs_mod_ros into Catkin src space
git clone https://github.com/tud-cor/fs_mod_ros $HOME/catkin_fs/src/fs_mod_ros

# install all dependencies
source /opt/ros/melodic/setup.bash
rosdep update --rosdistro=$ROS_DISTRO
rosdep install -i --from-paths $HOME/catkin_fs/src

# build the workspace
catkin config --install -w $HOME/catkin_fs
catkin build -w $HOME/catkin_fs

# activate the workspace
source $HOME/catkin_fs/install/setup.bash
```


## Running
#### Steps for running ROS navigation stack:

In one terminal:

```sh
source ~/catkin_fs/install/setup.bash

# launch the main entrypoint to running a simulated world within FarmSim
roslaunch fs_navigation farmsim_bringup.launch
```

With this, all required components have been set up, and we can proceed to run the ROS navigation stack.

In another terminal:

```sh
source ~/catkin_fs/install/setup.bash
roslaunch fs_navigation gmapping_demo.launch
```

In a third terminal:

```sh
source ~/catkin_fs/install/setup.bash

# visualize the messages in Rviz
roslaunch fs_viz rviz.launch
```

**Some important parameters might need to be tuned:**

`max_obstacle_height` in [costmap_common_stvl.yaml](fs_navigation/config/costmap_common_stvl.yaml): the default value (3 meters) can't be used because the z cooridante of the terrain is almost always larger than 3 meters. In town center area, the terrain is around 80m high. So here we set it 100m.


`minimumScore` in [gmapping.launch](fs_navigation/launch/gmapping.launch): is a score for considering the outcome of the scan matching good enough in gmapping. The default value is 0 which might cause jumping pose estimates in our application. Hence we set it to 100 to increase the trust to odometry information.


#### Visualising FarmSim data without the navigation stack:

In a terminal:

```sh
source ~/catkin_fs/install/setup.bash

# launch the main entrypoint to running a simulated world within FarmSim
roslaunch fs_navigation farmsim_no_nav_bringup.launch
```


In another terminal:

```sh
source ~/catkin_fs/install/setup.bash

# visualize the messages in Rviz
roslaunch fs_viz rviz_no_nav.launch
```

