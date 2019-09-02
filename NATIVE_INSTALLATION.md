# Environment setup
## Install ROS
We will be using ROS Kinetic on Ubuntu 16.04 LTS (Xenial). **NO OTHER UBUNTU DISTRO/ROS VERSION IS SUPPORTED**

To install ROS Kinetic, follow the instructions here: [http://wiki.ros.org/kinetic/Installation/Ubuntu](http://wiki.ros.org/kinetic/Installation/Ubuntu)

Don't forget to edit your bashrc and install the development dependencies!

## Install Aikido dependencies
Add a few custom PPAs necessary for Aikido
```
$ sudo add-apt-repository ppa:dartsim/ppa
$ sudo add-apt-repository ppa:personalrobotics/ppa
$ sudo apt-get update
```

Install build dependencies for Dart
```
$ apt install git cmake build-essential libboost-filesystem-dev libmicrohttpd-dev libompl-dev libtinyxml2-dev libyaml-cpp-dev libccd-dev libfcl-dev liboctomap-dev libnlopt-dev liburdfdom-dev
```

Set your git identity. Substitute the email and name with the ones associated with your github account you use for the class.
```
$ git config --global user.email "tommy@usc.edu"
$ git config --global user.name "Tommy Trojan"
```

Build and install Dart from source
```
$ mkdir ~/ros_dependencies && cd ~/ros_dependencies
$ git clone https://github.com/dartsim/dart.git
$ cd dart
$ git checkout tags/v6.7.3
$ mkdir build && cd build
$ cmake ..
$ make -j4
$ make install
```

Extend your shared library path to include libdart
```
$ echo 'export LD_LIBRARY_PATH="$LD_LIBRARY_PATH:/usr/local/lib"' >> ~/.bashrc
$ source ~/.bashrc
```

Install some miscellaneous dependencies
```
$ apt install ros-kinetic-controller-interface ros-kinetic-transmission-interface ros-kinetic-realtime-tools ros-kinetic-controller-manager ros-kinetic-control-toolbox ros-kinetic-octomap-ros ros-kinetic-srdfdom python-catkin-tools python-pybind11
```

If you're setting up an installation before Lab 1, please stop here!
---

## Setting up your workspace
```
$ mkdir ~/rosws && cd ~/rosws
$ wstool init src
$ catkin init
$ catkin config --extend /opt/ros/kinetic
```

## Downloading the relevant repositories
```
$ git clone https://github.com/usc-csci-545-fall-2019/ros_installs.git ~/ros_dependencies/ros_installs
$ cd rosws/src
$ wstool init # exclude if already have .rosinstall
$ wstool merge ~/ros_dependencies/ros_installs/setup.rosinstall
$ wstool up
```

## Building the workspace
```
$ cd ~/rosws
$ catkin build
$ echo 'source $HOME/rosws/devel/setup.bash' >> ~/.bashrc
$ source ~/.bashrc # run this in all open terminals
```

## Running simple trajectories in simulation
Start 'roscore', 'rviz', subscribe to the topic '/dart_markers/simple_trajectories/update'
```
$ devel/bin/simple_trajectories -a
```

## Running simple trajectories in the real robot
Start 'roscore', 'rviz', subscribe to the topic '/dart_markers/simple_trajectories/update'
```
$ source devel/setup.bash
$ roslaunch ada_launch default.launch
```
On a separate tab:
```
$ source devel/setup.bash
$ devel/bin/simple_trajectories
```

## Running planning to TSR in simulation
Start 'roscore', 'rviz', subscribe to the topic '/dart_markers/soda_grasp/update'
```
$ source devel/setup.bash
$ devel/bin/soda_grasp_tsr
```

## Running planning to IK sampled from TSR in simulation
Start 'roscore', 'rviz', subscribe to the topic '/dart_markers/soda_grasp/update'
```
$ source devel/setup.bash
$ devel/bin/soda_grasp_ik
```
