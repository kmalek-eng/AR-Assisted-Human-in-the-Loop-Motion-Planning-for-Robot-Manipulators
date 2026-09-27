# AR-Assisted Human-in-the-Loop Motion Planning for Robot Manipulators

This project implements an augmented reality (AR) interface for online robot programming, motion planning, and robot-state visualization. The interface allows the operator to define robot poses and obstacles using holographic objects, generate collision-free paths using RRT*, and review candidate paths before execution.

The system combines RRT* for position planning, SLERP for smooth end-effector orientation interpolation, and Sequential Quadratic Programming (SQP) for inverse kinematics. The planned motion is visualized in AR, allowing the operator to use workspace context to select or adjust the path before sending it to a seven-DOF Kinova Gen3 robot.

## Workflow

The operator uses Microsoft HoloLens to place holographic representations of the gripper's initial, intermediate, and final poses and to position virtual obstacles over physical obstacles in the workspace. These poses and obstacle locations provide the inputs for robot motion planning.

![Steps in AR-assisted robot programming](<Figure 4. Steps in robot programming using AR.svg>)

The headset sends the position and orientation of the AR objects to the planning code. The coordinates are transformed from the HoloLens world frame to the robot base frame. RRT* then generates a collision-free end-effector path, while SLERP calculates the corresponding orientation along the path.

SQP inverse kinematics calculates the robot joint configurations for the planned position and orientation. The resulting path and robot configurations are sent back to the AR interface so the operator can inspect the planned motion. The operator can reposition the holographic objects and repeat the planning process when needed. After the path is accepted, the calculated joint motion is sent to the Kinova control system through the Kortex API.

![AR robot programming platform](<Figure 5. Platform of immersive robot programming.svg>)

The robot model and motion planning were also simulated in MATLAB/Simulink before execution on the physical Kinova Gen3 manipulator.

## AR positioning uncertainty

An experiment was performed to measure positioning errors introduced when users align holographic gripper poses with physical reference objects. Users placed the holographic gripper over 3D-printed replicas of the robot end-effector at different positions and orientations and then used these poses to command the robot.

![AR object positioning experiment](<Figure 9. Experiments evaluating the uncertainties in AR objects.svg>)

The difference between the commanded robot position and the physical reference was measured in the robot tool-frame X and Y directions. The mean error was 1.0 mm in X with a standard deviation of 5.1 mm, and -1.2 mm in Y with a standard deviation of 4.5 mm.

## Methods

The main methods used in the platform are:

- RRT*: collision-free end-effector path planning
- SLERP: smooth interpolation of end-effector orientation
- SQP: inverse kinematics for calculating robot joint configurations
- Microsoft HoloLens: AR interaction and visualization
- Kinova Gen3: seven-DOF robot manipulator
- Kinova Kortex API: robot communication and control
- MATLAB/Simulink: robot kinematic simulation

## Demo

[View the demonstration video](<Untitled video (1).mp4>)
