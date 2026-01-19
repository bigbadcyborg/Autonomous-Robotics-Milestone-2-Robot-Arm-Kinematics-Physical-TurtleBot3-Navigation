# 2026 CS 4379K / CS 5342 Introduction to Autonomous Robotics, Robotics and Autonomous Systems

## Programming Assignment: Milestone 2 (V1.1)

**Minhyuk Park and Tsz-Chiu Au**

### Introduction

Welcome to CS 4379K / CS 5342. We have prepared a few milestones before the final project, in which you will use a physical robot to execute a mission given by us. The following milestones will give you an idea of how to interact with ROBOTIS’ Turtlebot3 waffle Pi with a Manipulator Arm using the Robot Operating System 2.

The second milestone is about simulating and controlling a robot arm in Gazebo, and controlling the physical turtlebot by running teleoperation, SLAM, and Navigation nodes.

To do this, you will deploy our pre-configured Docker container that sets up all the software that is required for the assignment.
Please refer to the following video for an explanation of what a Docker container environment is. 

[https://www.youtube.com/watch?v=Gjnup-PuquQ](https://www.youtube.com/watch?v=Gjnup-PuquQ)

You might find the official tutorial on ROS 2 Humble useful in this course:

[https://docs.ros.org/en/humble/Tutorials.html](https://docs.ros.org/en/humble/Tutorials.html)

### Assignment Requirement

A hardware video demonstration submission is required for Milestone Assignment 2. You need to demonstrate that you have a working setup and can operate a physical turtlebot by making a video. Refer to the demo requirement section at the end of the milestone assignment on what to include in the video. Once your group is done with the video demonstration that satisfies the demo requirement outlined at the end, please submit it to Canvas. Each group will submit one video. Rules for robot usage will apply for working with the physical Turtlebot3. Please refer to the inventory list given to you separately.

> **Change Log**
> v1.0 -> v1.1: Tested on Humble environment
> v0.9 -> v1.0: Clarified troubleshooting steps regarding Jetson’s boot sequence

---

### Part 1: Simulation of the Robotic Arm

For part 1, we need you to further demonstrate your simulated environment with the operation of a robotic manipulator in the Gazebo simulated environment. This assumes that you have a working setup from Milestone Assignment 1 Part 1.

**How to run an Open Manipulator X Arm simulation using RViz**

This tutorial is based on the following manual. Select Humble for the manual specific to ROS2 Humble.

* **(Original URL)** [https://emanual.robotis.com/docs/en/platform/turtlebot3/manipulation/](https://emanual.robotis.com/docs/en/platform/turtlebot3/manipulation/)

[Remote PC] To use MoveIt to operate the OpenMANIPULATOR-X in the Gazebo, terminate other Gazebo and RViz tools first.

[Remote PC] Enter the command below to launch RViz with MoveIt configuration.

```bash
ros2 launch turtlebot3_manipulation_moveit_config moveit_gazebo.launch.py

```

[Remote PC] The MoveIt Interface on RViz will be launched along with the Gazebo simulator. You can move the blue ball to move the location of the gripper in 3d space and click Plan and Execute under motionPlanning -> Planning -> Commands. Play around with parameters such as planning time.

---

### Part 2: Operation of Physical Turtlebot 3

For part 2, we need you to demonstrate your ability to interface with the Physical Turtlebot by running Teleoperation, SLAM, and Navigation nodes. We will start by working with the robot plugged into the wall instead of using the battery. You need to make sure not to confuse which power adapter goes where.

1. Power up the turtlebot by plugging in 12V power to the turtlebot3’s front OpenCR board and turning on the power switch next to the power jack. The Turtlebot should power up.
2. The green LED on the OpenCR board will light up, and the motors on the wheels and robot arm will start to feel stiff.
3. Plug in your group’s microSD card to the micro SD extender, which is connected to the Nvidia Jetson at the back of the turtlebot.
4. Plug in the power jack for the Nvidia Jetson.
5. Nvidia Jetson will start to emit green light, and the lidar sensor, which is connected to the Jetson’s USB port, should start spinning. If not, most likely it is not reading the SD card. Go to step 5.
6. You may then interface with Nvidia Jetson by plugging in hdmi and usb c cable between Jetson and Flipbook (Laptop-like portable monitor + keyboard battery unit).

If you suspect Jetson is not booting up, try reseating the SD card extender on the Nvidia Jetson and put the power jack back in to reboot the Jetson. Otherwise, you can try to bypass SD card extender and directly insert the SD card into the Jetson.

Refer to the appendix for steps to reflash the SD card if it is corrupt.

**Jetson sudo**

```text
User: nvidia
Password: nvidia

```

#### Teleoperation

This manual is based on the following manual for Humble.

* **(Original URL)** [https://emanual.robotis.com/docs/en/platform/turtlebot3/manipulation/#simulation](https://emanual.robotis.com/docs/en/platform/turtlebot3/manipulation/#simulation)

This manual assumes you have completed Milestone 1 Part 1 on setting up your remote PC.

[Turtlebot Jetson][Remote PC]  Make sure Jetson is connected to the small router we provided (small_blue_wifi), along with the remote PC (NUC or PC that you configured for milestone assignment 1), so that they stay on the same local network. You should also sync the time for both the remote PC and nvidia Jetson by going through Ubuntu settings.

[Turtlebot Jetson] The following command will bring up the actual TurtleBot3 hardware with OpenMANIPULATOR-X on it. Open a terminal from the TurtleBot3 SBC.

Bring up the TurtleBot3 Manipulation using the following command.

```bash
ros2 launch turtlebot3_manipulation_bringup hardware.launch.py

```

[Remote PC] Enter the command below to launch MoveIt on RViz.

```bash
ros2 launch turtlebot3_manipulation_moveit_config moveit_core.launch.py

```

[Remote PC] To operate the robot with the keyboard teleoperation node, the RViz must be terminated. Then launch the servo server node and teleoperation nodes on a separate terminal window.

```bash
ros2 launch turtlebot3_manipulation_moveit_config servo.launch.py
ros2 run turtlebot3_manipulation_teleop turtlebot3_manipulation_teleop

```

The following keys are used to control the TurtleBot3. Try moving the turtlebot in the physical space. Use O, K, L, ; keys to drive the TurtleBot3 platform.

```text
Use o|k|l|; keys to move turtlebot base and use 'space' key to stop the base
Use s|x|z|c|a|d|f|v keys to Cartesian jog
Use 1|2|3|4|q|w|e|r keys to joint jog.
'ESC' to quit.

```

#### SLAM

This manual is based on the following manual for ROS2 Humble.

* **Original URL:** [https://emanual.robotis.com/docs/en/platform/turtlebot3/manipulation/#simulation](https://emanual.robotis.com/docs/en/platform/turtlebot3/manipulation/#simulation)

Note: This earlier section of the manual provides a better overview of SLAM. You might want to read up on the SLAM and Simulation section. Select Humble for specific instructions for Humble.

* **Original URL:** [https://emanual.robotis.com/docs/en/platform/turtlebot3/slam/](https://emanual.robotis.com/docs/en/platform/turtlebot3/slam/)

Close all terminals on the remote PC if you are coming from previous sections.

[Turtlebot Jetson] The following command will bring up the actual TurtleBot3 hardware with OpenMANIPULATOR-X on it. Open a terminal from the TurtleBot3 SBC.

Bring up the TurtleBot3 Manipulation using the following command. Skip this step if you are coming from the previous section and your bringup is already running.

```bash
ros2 launch turtlebot3_manipulation_bringup hardware.launch.py

```

[Remote PC] Open a terminal on Remote PC. Launch the slam node using the following command.

```bash
ros2 launch turtlebot3_manipulation_bringup gazebo.launch.py

```

[Remote PC] Launch the slam node using the following command.

```bash
ros2 launch turtlebot3_manipulation_cartographer cartographer.launch.py

```

[Remote PC] Open two terminals on Remote PC. Launch the servo server node. Launch the keyboard teleoperation node. Use O, K, L, ; keys to drive the TurtleBot3 platform to create a good “map” of the environment.

```bash
ros2 launch turtlebot3_manipulation_moveit_config servo.launch.py
ros2 run turtlebot3_manipulation_teleop turtlebot3_manipulation_teleop

```

[Remote PC] Open a new terminal on Remote PC. Run the nav2_map_server to save the current map on RViz.

```bash
ros2 run nav2_map_server map_saver_cli -f ~/map

```

[Remote PC] Refer to the previous assignment’s tuning guide to tune your map, should you find it necessary while performing steps on navigation.

#### Navigation

This manual is based on the following manual for ROS2 Humble.

* **(Original URL)** [https://emanual.robotis.com/docs/en/platform/turtlebot3/manipulation/#simulation](https://emanual.robotis.com/docs/en/platform/turtlebot3/manipulation/#simulation)

Note: Simulation for vanilla turtlebot 3 and turtlebot 3 with manipulator arm that we use operate under different software packages. Small details such as file locations might be different. However, you might want to read up on the Navigation and Simulation section to get a better understanding of the entire process. Select Humble for specific instructions for Humble.

* **(Original URL)** [https://emanual.robotis.com/docs/en/platform/turtlebot3/navigation/](https://emanual.robotis.com/docs/en/platform/turtlebot3/navigation/)

Close all terminals on the remote PC if you are coming from previous sections.

[Turtlebot Jetson] The following command will bring up the actual TurtleBot3 hardware with OpenMANIPULATOR-X on it. Open a terminal from the TurtleBot3 SBC.

Bring up the TurtleBot3 Manipulation using the following command. Skip this step if you are coming from the previous section and your bringup is already running.

```bash
ros2 launch turtlebot3_manipulation_bringup hardware.launch.py

```

[Remote PC] Open a terminal on Remote PC. Launch the navigation file using the following command. Note that you are referring to the map.yaml file created in the previous step for SLAM.

```bash
ros2 launch turtlebot3_manipulation_navigation2 navigation2.launch.py map_yaml_file:=$HOME/map.yaml

```

Click on the 2D Nav Goal button in the RViz menu GUI.

Click on the map to set the destination of the robot and drag the green arrow toward the direction where the robot will be facing.

This green arrow is a marker that can specify the destination of the robot.

The root of the arrow is the x, y coordinate of the destination, and the angle is determined by the orientation of the arrow.

As soon as x, y, and targets are set, TurtleBot3 will start moving to the destination immediately.

Recall from SLAM that you can tune SLAM parameters. You can tune Navigation parameters as well by modifying the appropriate YAML file.

We will leave you this URL again as a reference for the complete set of navigation parameters, just like we did for Assignment 1:

[https://emanual.robotis.com/docs/en/platform/turtlebot3/navigation/#navigation](https://emanual.robotis.com/docs/en/platform/turtlebot3/navigation/#navigation)

---

### Video Demo Requirements (3-Minute Demonstration)

Your group will submit a single, continuous screen-recorded video (e.g., MP4 format) that is approximately three minutes long. One member should narrate the video, explaining each step. Please state the names of all group members at the beginning.

For the physical robot sections (Parts B and C), your video must show both your computer screen (RViz, terminals) and the physical robot's movement simultaneously. A picture-in-picture format, using a phone camera or webcam to show the robot, is highly recommended.

**Part A: Manipulator Control in RViz**
This part demonstrates your ability to control the simulated manipulator arm using MoveIt and RViz.

* **Launch Simulation:** Start the Gazebo simulation and launch the MoveIt RViz configuration.
* **Interactive Marker Control:** Use the interactive marker attached to the gripper in RViz to plan and execute several movements. You must demonstrate moving the end-effector up/down, forward/backward, and rotating it around at least one axis.
* **Gripper Goal State:** Use the "Goal State" dropdown in the MoveIt MotionPlanning panel to command the gripper. Show the successful execution of both the "open" and "close" states.
* **RViz UI Exploration:** Demonstrate one useful feature of the RViz interface that helps with motion planning. Briefly explain what you are showing.

**Part B: Physical Robot SLAM**
This section demonstrates mapping a real-world environment with the physical TurtleBot.

* **Launch Nodes:** Bring up the physical robot hardware and launch the appropriate SLAM and teleoperation nodes on your remote PC.
* **Map the Environment:** Use the keyboard teleoperation node to drive the TurtleBot around a physical space. The area you map should contain at least two distinct obstacles (e.g., a table and a trash can).
* **Show the Process:** Your video must clearly show the map being constructed in real-time in RViz as the physical robot moves through the environment.
* **Save the Map:** Once you have a complete and accurate map, use the command to save the map to a file.

**Part C: Physical Robot Navigation**
This final part demonstrates autonomous navigation using the map you created.

* **Launch Navigation:** Relaunch the robot nodes, this time starting the Navigation2 stack and loading the map you saved in Part B.
* **Localize the Robot:** In RViz, use the "2D Pose Estimate" tool to set the robot's initial position. Ensure the robot's digital pose on the map accurately reflects its actual physical location.
* **Navigate to Goals:** Use the "2D Nav Goal" tool to send the robot to two different physical locations:
* **Goal 1:** A simple, unobstructed point in the room.
* **Goal 2:** A point that requires the robot to navigate around one of the obstacles you mapped.


* **Show Success:** The video must clearly show the planned path in RViz and the physical robot safely following the path to arrive at both destinations.

---

