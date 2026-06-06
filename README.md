Robotic Hand Assembly ROS 2 Project

This repository contains the URDF/Xacro description for a robotic hand and arm mechanism, developed and validated for ROS 2.
Project Overview

This package defines the kinematic structure of an arm with 5 degrees of freedom, concluding with a parallel gripper mechanism. The model has been verified using ROS 2 native parsing tools.
Kinematic Structure

The robot follows the following transformation hierarchy:

    world -> base_link -> base_plate

    base_plate -> forward_drive_arm

    forward_drive_arm -> horizontal_arm

    horizontal_arm -> claw_support

    claw_support -> gripper_left & gripper_right (parallel mimic)

Verification

The URDF was validated using the check_urdf utility:

Robot Name: hand_robo_arm

Successfully Parsed XML 
Root Link: world has 1 child(ren)
child(1):  base_link
child(1):  base_plate
child(1):  forward_drive_arm
child(1):  horizontal_arm
child(1):  claw_support
child(1):  gripper_left
child(2):  gripper_right
How to Build
1. Clone the Repository

Clone this repository into the src folder of your ROS 2 workspace:

cd ~/robotics_ws/src
git clone https://github.com/Ashamyu/robot_arm.git hand_robo
2. Build the Package

Build the package using colcon:

cd ~/robotics_ws
colcon build --packages-select hand_robo
3. Source the Workspace

After the build completes successfully, source the workspace setup file:

source install/setup.bash
4. Verify the Installation (Optional)

Confirm that the package is available in your ROS 2 environment:

ros2 pkg list | grep hand_robo
