# rt2_ass1
# First Assignment of the Research Track 2 course (Robotics Engineering)

# DESCRIPTION OF THE PACKAGE'S CONTENTS
I have two packages and four scripts in this repository. rt2_ass and rt2_ass1 are the names of the two packages.The software ROS noetic is used to implement the package rt2_assignment. The rt2_ass1 package, on the other hand, was created using the ROS2 foxy software. I need the rt2 assignment package in a ros noetic workspace and another in a ros2 foxy workspace.

It is necessary to have the ros1 bridge package installed in the ros2 foxy workspace in order to run this program. You may get this package from https://github.com/ros2/ros1 bidge.
ros.sh, ros2.sh, ros12.sh, and script.sh are the four scripts.

I included two nodes in the rt2_assignment package: go to point.py and user interface.py. The go_to_point node implements the server that lets the robot to achieve a goal position by first rotating to direct the robot towards the goal position, then moving in a straight line to the goal, and finally rotating to obtain the goal orientation..

The user interface provides a client that takes keyboard input and sends the command start or stop to the node state machine's server.

I have two components in the rt2 assignment1 package: the position service and the state machine, as well as the mapping_rule.yaml file. In ros noetic and ros2 foxy, the mapping rule is required to ensure communication between the two packages. When the state machine needs to generate another goal position, it calls the position service. It accepts the robot's possible x and y ranges as input and provides a random position and orientation. The component state machine, on the other hand, handles robot control; it waits for the start instruction and then calls the position service; after a response is received, it also calls the ros-implemented go to point server.

In both ros noetic and ros foxy, the script script.sh is used to run the complete program. It will launch three terminals: one in ros noetic, one that can run both ros noetic and ros foxy, and one that runs ros foxy. The script ros.sh is run in the first terminal, and the ros noetic workspace is sourced within that script. The launch file is then called in the first terminal to execute gazebo and the two nodes in the package rt2_assignment. Both workspaces will be sourced in the second terminal, and then the ros bridge will be executed to facilitate communication between the two packages.On the last terminal, the ros2 workspace is sourced, and the launchfile that starts the container and the two components is executed.

# HOW TO RUN
To run this program, you must have the 4 scripts in your root folder, as well as your ros noetic workspace called my ros and for your ros2 foxy workspace called my_ros2. If this is not the case, the scripts ros.sh, ros12.sh, and ros2.sh must be changed.
To begin the simulation, follow these steps:
- source script.sh

To compile the simulation: 
inside the ros noetic workspace run:
- catkin_make
Inside the ros2 foxy workspace run to build only the package:
- colcon build --symlink-install --packages-skip ros1_bridge
Inside the ros2 foxy workspace run to build only the bridge:
- colcon build --packages-select ros1_bridge --cmake-force-configure


# ROBOT BEHAVIOUR
With orientation 0 and velocity zero, the robot starts at point (0;0). It waits for a user command before beginning the move. When the robot first starts up, it moves to a random place and orientation. When the user issues the stop command, the robot comes to a complete halt and cancels all prior goals. If the start order is issued once more, the robot will begin traveling in the direction of a new target. If the stop command is not used before the robot reaches the goal position, it will automatically request a new goal position, which it will reach as well.

# SOFTWARE ARCHITECTURE AND ARCHITECTURAL CHOICES
This program is implemented in both ros noetic and ros2 foxy. In particular we have two nodes, written in python, in ros noetic and two components, written in cpp, in ros2 foxy. We have a central node, state_machine that communicates with all the other nodes, it is a server for user_interface and also it is a client for both the node go_to_point and the position_service. In order to implement the communication between these nodes it is needed the package ros1_bridge. 

# SYSTEM'S LIMITATIONS AND POSSIBLE IMPROVMENTS
One probable flaw with this code is that after serving a position request, the robot would serve another request even if the command stop was issued. To circumvent this, I utilized a global variable that is set to true when the robot reaches a goal position; when a new call of the timer callback is made, I check if that global variable is set to true, and if it is, I do nothing for one cycle. This can squander time and isn't practical in a real-world setting. Attempting to avoid this behavior could be a future implementation option.

