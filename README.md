# AR-Assisted Human-in-the-Loop Motion Planning for Robot Manipulators

This project implements an AR-based robot programming platform using Microsoft HoloLens and a seven-DOF Kinova Gen3 manipulator. The operator defines gripper poses and obstacles with holographic objects, reviews the planned robot motion in AR, and can adjust the inputs before the motion is executed.

## Workflow

The operator places holographic representations of the gripper's initial, intermediate, and final poses and aligns virtual obstacles with physical obstacles in the workspace. After the poses are confirmed, the platform calculates the robot motion and displays the planned path and robot configurations in AR. The operator can adjust the holographic objects and repeat the planning process before sending the accepted motion to the robot.

![Steps in AR-assisted robot programming](<Figure 4. Steps in robot programming using AR.png>)

## Methods

The HoloLens provides the positions and orientations of the gripper poses and obstacles in its world coordinate frame. The platform transforms these values to the robot base frame after aligning the HoloLens and robot coordinate systems.

RRT* generates a collision-free end-effector path, SLERP calculates the end-effector orientation along the path, and inverse kinematics calculates the corresponding robot joint configurations. The planned path and joint configurations are sent back to the HoloLens for visualization. After the operator accepts the motion, the joint commands are sent to the Kinova Gen3 through the Kortex API. MATLAB/Simulink is also used to simulate the robot kinematics and planned motion.

![AR robot programming platform](<Figure 5. Platform of immersive robot programming.png>)

## MATLAB implementation

The main MATLAB script is [`Gen3_25_both_app_automate.m`](<MATLAB code/Gen3_25_both_app_automate.m>). It receives pose and obstacle data from the HoloLens and coordinates the main planning and control workflow, including coordinate transformations, path planning, inverse kinematics, and high-level robot commands.

The following scripts provide high-level commands for moving the Kinova Gen3 arm to predefined configurations and controlling the gripper:

- `go_initial1.m`
- `go_initial2.m`
- `go_initial3.m`
- `go_zero.m`
- `come_home.m`
- `open_gripper.m`
- `close_gripper.m`

`RrtPlanner.m` contains the RRT-based path-planning code and was modified for integration with the main MATLAB workflow. `constr.mat` and `constr.txt` contain supporting constraint data used by the planner.

The remaining MATLAB files are primarily components from the Kinova MATLAB/Simulink packages used for robot communication and control.

## AR positioning uncertainty

An experiment was performed to measure errors caused by manually aligning holographic gripper poses with physical objects. Users aligned the holographic gripper with 3D-printed replicas of the robot end-effector placed at different positions and orientations, and these poses were then used to guide the robot.

![AR object positioning experiment](<Figure 9. Experiments evaluating the uncertainties in AR objects.png>)

The mean positioning error was 1.0 mm in the tool-frame X direction with a standard deviation of 5.1 mm, and -1.2 mm in the Y direction with a standard deviation of 4.5 mm.

## Demo

https://github.com/user-attachments/assets/1643842c-61c7-4311-a2d7-3893bcd6c568
