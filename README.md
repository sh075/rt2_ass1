# rt2_ass1
# First Assignment of the Research Track 2 course (Robotics Engineering)

Shahrzad Eskandari Majdar (5060737)

This README.md file is related to the second branch **ros2**.   

The cpp nodes ('Robot FSM' and 'position server') should be written in ROS2, as components. Because of that, by using the ros1_bridge, they can be interfaces with the ROS nodes and with the simulation in Gazebo. The 'go_to_point' node can still be implemented as a service.

## Architecture of the system:
The package contains the nodes and the simulation environment for controlling a mobile robot in the Gazebo simulation environment.

The robot starts at the orign (0,0) without any orientation or velocity. After being requested by the user or pressing 1, it starts moving towards the first random generated position. The user can command the robot to stop at any time by pressing 0. However, it will stop after reaching the target. When the next 'start' command is called, the robot will move to the next random generated position.

This program is implemented in both ros noetic and ros2 foxy. In particular, we have two nodes, written in python, in ros noetic and two components, written in cpp, in ros2 foxy. We have a central node, state_machine that communicates with all the other nodes, it is a server for user_interface', and it is also a client for both the node 'go_to_point' and the 'position_service'. In order to implement the communication between these nodes, it is needed the package ros1_bridge. 


## How to run the node:
- a launch file to start the container manager and the components should be created
- a script to launch all required nodes and the simulation should be implemented

To brige ROS and ROS2, we need the scripts named 'ros.sh', 'ros12.sh' and 'ros2.sh'. 

To compile the simulation: 
inside the ros noetic workspace run:
 ```
 catkin_make
 ```
Inside the ros2 foxy workspace, build only the package and run:
```
 colcon build --symlink-install --packages-skip ros1_bridge
 ```
Inside the ros2 foxy workspace, build only the bridge and run:
```
 colcon build --packages-select ros1_bridge --cmake-force-configure
```
To start the simulation, run:
```
 source bridge.sh
```


