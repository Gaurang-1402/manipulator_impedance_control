# Impedance Controller

Panda Robot Arm places blocks in the bowl


![unnamed](https://github.com/Gaurang-1402/manipulator_impedance_control/assets/71042887/b9a97d8b-6a98-4d00-920a-3d113f3f6b03)


# Task 

The goal of this project is to solve a simple manipulation task: picking up objects and moving them in a bowl.

We will be using a model of the Frank-Emika Panda robot, which features:
- 7 revolute joints

## Preliminary Analysis

### Understanding the Robot and Environment
- **Spatial Frame Location**: Base of the robot
- **Orientation Determination**: Trial and error method using the PD controller
- **Variable Definition**: Suitable variables defined to measure the length of different joints on the Panda robot
- **Simulation Interaction**: Manipulated blocks in the simulation to understand their physics

![image](https://github.com/Gaurang-1402/manipulator_impedance_control/assets/71042887/9ecc4b7b-8e91-42b5-a4fb-6b412cc675e1)

![image](https://github.com/Gaurang-1402/manipulator_impedance_control/assets/71042887/7a0406a7-5f2c-4f77-a9e6-902f6b291fed)


### Methodology
![image](https://github.com/Gaurang-1402/manipulator_impedance_control/assets/71042887/8713cab5-41c8-4c3e-836a-2e41ce575ebe)


#### Impedance Controller

An impedance controller was chosen to steer the end-effector trajectory, owing to the following advantages it offers:

1. **Adaptability**: It grants compliant behavior, permitting the robot to adjust to external forces and adapt to its surroundings. This trait is critical when the robot interacts with the bowls and blocks.
2. **Natural Movement**: The controller facilitates natural, human-like movements, thereby making the task of grasping a block more intuitive and human-friendly. This is achieved by mirroring a spring-damper system's operation, resulting in smoother actions.
3. **Energy Absorption and Safety**: During interactions with the environment, the robot can absorb and dissipate energy, a feature that is particularly beneficial when handling blocks and placing them in the bowl.

The choice of impedance controller allows defining an equilibrium position for the desired position, presenting a flexible solution compared to the resolved rate and PD controller.

To achieve this, I defined several variables such as the goal positions for the first red block, second red block, and the bowl. Utilizing the spatial frame pose, I calculated a temporary "base position" that served as an intermediary position before attempting to pick up the next block. This base position was positioned approximately in the middle of all the objects for ease of access.
In order to fine-tune the performance of my controller, I had to determine appropriate values for the spring constant and damping coefficients. Initially, I used arbitrary values and conducted tests to assess the controller's performance. Through a process of trial and error, I adjusted these parameters to improve the controller's ability to reach the desired targets.
To account for the effect of gravity and ensure free movement of the end effector, I utilized a gravity compensation computation function. This function provided me with the necessary values to add to the joint torque, in addition to the impedance controller.

To facilitate later plotting of the measured end effector trajectories, I utilized a given forward kinematics helper function. By extracting the end effector pose from this function, I saved the position at each time step in a separate array.

To simplify the task, I divided it into several segments. I incorporated an offset in the z-direction from the blocks and the bowl, which I referred to as the "docking position" for the purpose of explanation. These segments included:

1) Reach the block docking position from the base position. Time taken – 3 seconds.
2) Reach the block goal from the block docking position. Grip the block. Time taken – 2 seconds.
3) Return to the block docking position from the block goal. Time taken – 2 seconds.
4) Reach the bowl goal plus offset from the block docking position. Release the block. Time taken – 5 seconds.
5) Reach base position from the bowl goal plus offset. Time taken – 3 seconds.
   
This process is carried out for both the first red block and the second red block, numbered from left to right. The total time taken for the entire task is 30 seconds, with each block taking 15 seconds to drop the block and return to the equilibrium position.

The positioning of the gripper varies depending on the specific requirements of each instance. The offset from the block goal and the offset from the bowl goal are determined through trial and error. As a general guideline, the goal offset was set to be twice the second and first block offsets to ensure that the end effector gripping the block does not knock over the blocks or bump into the bowl edges while moving towards the bowl goal plus the offset position.

For the computation of the robot's trajectory across various positions, I utilized a compute trajectory function that I had previously defined in lab 4. This function ensured that the velocity of the end effector was set to zero upon reaching any of the goal positions.

The impedance controller operates based on the following concept: we obtain the reference position and reference trajectory using the aforementioned logic through the compute_trajectory function. We then
calculate the Jacobian in the spatial frame (O frame), resulting in a 6 by 7 matrix. To obtain the Jacobian corresponding to translation velocity and forces, referred to as J_linear in the code, we extract the first three rows of the Jacobian matrix, resulting in a 3 by 7 matrix. Utilizing this Jacobian, we compute the reference
velocity (or desired velocity). The joint torques are calculated using the following equation:

![image](https://github.com/Gaurang-1402/manipulator_impedance_control/assets/71042887/69f65296-b56f-470b-924d-3e6df20adf68)



### Analysis

Within the simulation loop, we maintain separate arrays to track the measured joint positions and measured joint velocities. This enables us to generate a chart that illustrates the changes in each of the 7 joint positions and velocities over time. By plotting this data, we can gain a visual understanding of how the joints evolve
throughout the duration of the simulation.

![image](https://github.com/Gaurang-1402/manipulator_impedance_control/assets/71042887/f4f167cb-328b-4710-80c9-085cd2ec88a9)

In Figure 3, it is evident from some of the joint positions vs. time graphs that there is a mirrored trajectory observed between 0 to 15 seconds and 15 to 30 seconds (particularly noticeable for θ₁, θ₄, θ₅, and θ₆). This phenomenon is likely attributed to the end effector following a similar trajectory when picking up the first block and the second block. The peaks on the graph during the 15 to 30-second trajectory are slightly lower in some cases because block two is closer to the base position compared to block one.

Moreover, on certain joint velocities vs. time graphs, it can be observed that the velocity of the end effector reaches zero approximately 10 to 14 times for each joint. This occurrence corresponds to the number of times the end effector needs to halt as it approaches the goal position, ensuring zero velocity before transitioning to
the subsequent phase of the robot's movement as dictated by the compute_trajectory function.

![image](https://github.com/Gaurang-1402/manipulator_impedance_control/assets/71042887/cbde32a8-4248-4f2a-b583-38082c9c8d1f)


In Figure 4, we observe the trajectory followed by the robot in three different planes: the xy plane, yz plane, and xz plane. Changes in the xy plane indicate the robot's smooth lateral movement from side to side, as seen from the perspective of the simulation's front view. Changes in the yz plane depict the robot's movement during block pickup or when navigating the offsets from the blocks and goals. Once again, a smooth trajectory is evident. Also, the end effector demonstrates prominent and smooth movements in the xz plane, as illustrated above.
The overlaps observed in the trajectory plots are likely a result of the robot returning to the same plane after completing its movement from the bowl goal position. This repetition in the plane contributes to the
overlapping segments observed in the trajectory visualization.
