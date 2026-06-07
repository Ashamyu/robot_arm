# Hand Robotics Project - ROS 2 & MoveIt 2

## Overview

This project focuses on the kinematic modeling, configuration, and simulation of a custom multi-DoF robotic hand using **ROS 2**, **URDF/Xacro**, and **MoveIt 2**.

The project was developed as part of the **Master’s in AI & Robotics (M1)** coursework and demonstrates robotic hand modeling, transform management, visualization, and motion planning inside a complete ROS 2 environment.

🔗 **GitHub Repository:** [robot_arm](https://github.com/Ashamyu/robot_arm.git)

---

# Table of Contents

* [Robot Description](#robot-description)
* [Kinematic Structure](#kinematic-structure)
* [Why Xacro Instead of Raw URDF](#why-xacro-instead-of-raw-urdf)
* [Mass, Inertia, and Joint Assumptions](#mass-inertia-and-joint-assumptions)
* [Installation Guide](#installation-guide)
* [Running RViz 2](#running-rviz-2)
* [Running MoveIt 2](#running-moveit-2)
* [Screenshots and Media](#screenshots-and-media)
* [Challenges and Solutions](#challenges-and-solutions)
* [Author](#author)

---

# Robot Description

The `hand_robo` package contains the complete robotic hand model, including:

* URDF/Xacro robot description
* Joint and transform configuration
* Robot visualization in RViz 2
* Motion planning integration with MoveIt 2
* Interactive joint manipulation
* Kinematic chain management

The project was designed to support realistic robotic hand movement simulation while maintaining modular and reusable robot description files.

---

# Kinematic Structure

The robotic hand follows the transformation hierarchy below:

```text
World
 └── Basement
      └── Base_link
           └── base_plate
                └── Forward_drive_arm
                     └── horizontal_arm
                          └── claw_support
                               ├── gripper_right
                               └── gripper_left
```

---

# Why Xacro Instead of Raw URDF

This project uses **Xacro** instead of plain URDF files for several important reasons.

## 1. Better Modularity

Xacro reduces repeated XML code by allowing reusable parameters and macros.
Instead of duplicating identical link and joint definitions, shared properties can be defined once and reused throughout the robot model.

This makes the project:

* Easier to maintain
* Cleaner to read
* Faster to modify

---

## 2. Reusable Macros

Custom Xacro macros were created for inertia calculations and repeated structural components.

Rather than manually defining inertia values for every robot segment, the macros automatically generate them using configurable parameters such as:

* Mass
* Dimensions
* Radius
* Length

---

## 3. Easier Joint Coupling

Complex robotic hands often require synchronized finger movement.

Xacro simplifies the implementation of:

* Mimic joints
* Coupled motion behavior
* Shared constraints
* Repeated finger structures

This improves consistency and reduces configuration errors.

---

# Mass, Inertia, and Joint Assumptions

Because no physical manufacturing specifications were available, several engineering assumptions were made to ensure stable simulation performance.

## Material and Mass Assumptions

### Palm / Base Link

The base structure was assumed to be the heaviest component because it supports:

* Mechanical structures
* Electronics
* Actuator mounting
* Structural stability

### Finger Segments

The proximal, intermediate, and distal finger sections were assigned progressively lighter masses toward the fingertips.

Approximate inertias were generated using:

* Box primitives
* Cylindrical approximations
* Thin-shell assumptions

These approximations help maintain simulation stability inside RViz 2 and MoveIt 2.

---

## Joint Limits

### Rotational Joints

Joint rotation limits were constrained to:

* Prevent self-collision
* Maintain realistic movement ranges
* Simulate small servo motor behavior

### Mimic / Coupled Joints

Coupled joints were configured with controlled movement ranges to ensure:

* Stable grasping
* Consistent finger synchronization
* Reliable kinematic behavior

---

# Installation Guide

## System Requirements

This project was developed and tested on:

* **Ubuntu 24.04**
* **ROS 2 Jazzy Jalisco**

---

## Required Dependencies

Install all required ROS 2 packages before building the project:

```bash
sudo apt update

sudo apt install -y \
ros-jazzy-desktop \
ros-jazzy-moveit \
ros-jazzy-joint-state-publisher-gui \
ros-jazzy-xacro \
ros-jazzy-rviz2 \
python3-colcon-common-extensions
```

---

## Clone the Repository

Create a ROS 2 workspace and clone the repository:

```bash
mkdir -p ~/ros2_ws/src
cd ~/ros2_ws/src

git clone https://://github.com/Ashamyu/robot_arm.git
```

---

## Build the Workspace

Return to the workspace root directory and compile the package:

```bash
cd ~/ros2_ws

rm -rf build install log

colcon build --packages-select hand_robo
```

---

## Source the Workspace

After compilation, source the workspace setup file:

```bash
source ~/ros2_ws/install/setup.bash
```

---

# Running RViz 2

To launch the robotic hand model with interactive joint sliders:

```bash
ros2 launch hand_robo display.launch.py
```

This launches:

* RViz 2
* Robot State Publisher
* Joint State Publisher GUI

---

# Running MoveIt 2

To launch MoveIt 2 motion planning and inverse kinematics:

```bash
ros2 launch hand_robo_moveit_config demo.launch.py
```

This enables:

* Motion planning
* Inverse kinematics
* Trajectory execution
* Interactive planning inside RViz 2

---

# Challenges and Solutions

## Duplicate Package Error in Colcon

### Problem

The build process generated the following error:

```text
Duplicate package names not supported: hand_robo
```

This happened because the `build/`, `install/`, and `log/` directories were accidentally created inside the repository source folder, causing `colcon` to detect duplicate package paths.

---

### Solution

The issue was resolved by deleting the generated directories inside the repository:

```bash
rm -rf build install log
```

Then rebuilding the workspace from the root directory:

```bash
cd ~/ros2_ws
colcon build --packages-select hand_robo
```

This ensured that all generated files were placed correctly in the workspace-level folders.

---

# Author

**Sham Yu**
