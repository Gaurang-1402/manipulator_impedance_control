# Robotic-Manipulation-and-Locomotion
ROB-UY 2004 Robotic Manipulation and Locomotion


![image](https://github.com/Gaurang-1402/manipulator_impedance_control/assets/71042887/8713cab5-41c8-4c3e-836a-2e41ce575ebe)


# Task 

The goal of this project is to solve a simple manipulation task: picking up objects and moving them in a bowl.

## Report Requirements
- **Length**: 2-3 pages
- **Format**: PDF only
- **Content**:
  - The methodology you followed to answer the questions
  - Answers to questions requiring typeset answers
- **Additional Notes**:
  - You may add the plots in the report (does not count towards the page limit) or in the Jupyter notebook.

## Robot Model

We will be using a model of the Frank-Emika Panda robot, which features:
- 7 revolute joints

## Constraints

To successfully complete this task, your solution must adhere to the following constraints:
- **Libraries**: Only numpy and scipy can be used
- **Controller**: At least one controller in the end-effector space must be used
- **Motion**: Motions generated must be smooth
- **Gravity Compensation**: The solution must compensate for the robot's gravity

## Report Details

In your report:
- Describe and justify the choice of controller
- Analyze the system behavior
  - Include plots of end-effector trajectories, velocities, joint trajectories, etc., as deemed necessary

## Preliminary Analysis

### Phase 1: Understanding the Robot and Environment
- **Spatial Frame Location**: Base of the robot
- **Orientation Determination**: Trial and error method using the PD controller
- **Variable Definition**: Suitable variables defined to measure the length of different joints on the Panda robot
- **Simulation Interaction**: Manipulated blocks in the simulation to understand their physics

### Phase 2: Illustrations and Diagrams
- **Figure 1**: Illustrates the orientation of the spatial frame
- **Figure 2**: Showcases the different lengths on the robot based on a provided diagram

*Remember to include Figures 1 and 2 with appropriate captions to aid in understanding.*

