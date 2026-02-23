# 2026 CS 4379K / CS 5342 Introduction to Autonomous Robotics, Robotics and Autonomous Systems

## Programming Assignment: Milestone 2 (V1.0)

**Minhyuk Park and Tsz-Chiu Au**

### Introduction

Welcome to CS 4379K / CS 5342. This assignment will give you an idea of how to interact with ROBOTIS’ Turtlebot3 waffle Pi with a Manipulator Arm using the Robot Operating System 2.

The second milestone is about simulating and controlling a robot arm in Gazebo, and controlling the physical turtlebot by running teleoperation, SLAM, and Navigation nodes.

To do this, you will deploy our pre-configured Docker container that sets up all the software that is required for the assignment.
Please refer to the following video for an explanation of what a Docker container environment is. 

[https://www.youtube.com/watch?v=Gjnup-PuquQ](https://www.youtube.com/watch?v=Gjnup-PuquQ)

Robot Operating System version is associated with Ubuntu Long-Term Support Versions (e.g. Ubuntu 22.04 with Humble). We are using **ROS 2 Humble in a Docker environment** for Remote-PC. You might find the official tutorial on ROS 2 Humble useful in this course:

[https://docs.ros.org/en/humble/Tutorials.html](https://docs.ros.org/en/humble/Tutorials.html)

For all questions regarding milestone assignments and the robot, **you should contact the Doctoral Instructor Assistant via direct message on Slack**. Please do not contact the Instructor with questions regarding the milestone assignments. This is the URL for Slack for this course. 

<https://spring2026txstrobot.slack.com/>

### Assignment Requirement

A hardware video demonstration submission is required for Milestone Assignment 2. 

You need to demonstrate that you have a working setup and can operate the turtlebot in simulation by making videos. This will also demonstrate that you have a working setup for working with a physical turtlebot in the next milestone assignment. Refer to the demo requirement section at the end of the milestone assignment on what to include in the video.

**[SUBMISSION RULES]**

* **Individual Submission:** **Every team member must submit the video link(s) separately to Canvas.** If the video is duplicated within a team, that is acceptable; however, this ensures that only active participants who have access to the team’s recordings can receive credit. 

* **Standardized Hosting:** **To manage file sizes, do not upload raw video files (e.g., MP4) directly to Canvas.** Instead, **upload your videos to YouTube (set as "Unlisted")** and submit the links via a document.

### Video Demo Requirements

Your group will **record** one or more video clips. The estimated total length of the video clips is approximately two and a half minutes. **While you do not need to perform complex editing, please keep the total duration to a few minutes to ensure it remains concise.** One group member should narrate the video, explaining each step as it's performed. At the beginning of the first video clip, please show every group member's face and state the names of all group members.

Your recording setup should be organized to show all relevant windows at once: the terminal(s) used for launching nodes, the Gazebo simulation window, and the RViz visualization window.

You do not need to edit the videos, and uploading raw **footage** will suffice. You can split the demonstration into multiple videos **if necessary to show different parts of the requirement.** 

Rules for robot usage will apply for working with the physical Turtlebot3. Please refer to the inventory list given to you separately and the rules for the Robot room usage.

> **Change Log**
> v1.0: Tested on Humble environment

---

### **Part 1: Simulation of the Robotic Arm and Topic Inspection**

You will simulate the OpenManipulator-X in Gazebo, control it via keyboard teleoperation, and verify the message data structure using `ros2 topic`. Please execute all instructions with **[Remote PC]** on Docker shell. Note that you have to enable GUI and start the Docker container by following instruction from Milestone Assignment 1.

**[Remote PC]** Terminate any running Gazebo or RViz instances. Enter the command below on the **first Docker shell** to launch the Turtlebot3 Manipulation Gazebo environment.

```bash
ros2 launch turtlebot3_manipulation_gazebo gazebo.launch.py

```

**[Remote PC]** To control the TurtleBot3 in the Gazebo simulation, the servo server node of MoveIt must be launched first. Open a **second Docker shell** by opening another terminal window and entering the container on that terminal window. Type the following command in the new Docker shell.

    ros2 launch turtlebot3_manipulation_moveit_config servo.launch.py

**[Remote PC]** Open a **third docker shell**. We will use the Robotis teleoperation node to control the robot using your keyboard. Run the following command:

```bash
ros2 run turtlebot3_manipulation_teleop turtlebot3_manipulation_teleop

```

You should see the control menu appear. You can now use the keys (i,k,l,j for the base, and separate keys for joints/gripper) to move the robot in Gazebo.

```text
    Reading from keyboard
    ---------------------------
    Joint Control Keys:
      1/q: Joint1 +/-
      2/w: Joint2 +/-
      3/e: Joint3 +/-
      4/r: Joint4 +/-
    Use o|p to open/close the gripper.
    
    Command Control Keys:
      i: Move up
      k: Move down
      l: Move right
      j: Move left
      space bar: Move stop
    ---------------------------
    'ESC' to quit.
```

**[Remote PC]** Open a **fourth docker shell**. Now that the robot is running, we need to inspect the data being published.

1. **List Topics:** Run the following command to see all active topics in the simulation:
```bash
ros2 topic list

```

*Locate the topic named `/joint_states`.*

2. **Echo Topics:** We want to see the real-time data of the robot's arm joints. Run the following command:
```bash
ros2 topic echo /joint_states

```

**Activity:** Arrange your windows so you can see the **Teleop Terminal**, the **Gazebo Window**, and the **Topic Echo Terminal** simultaneously.
When you press keys in the Teleop terminal to move the arm, observe how the `position` values in the `/joint_states` topic change in the fourth docker shell. 

---

### Part 2: Operation of Physical Turtlebot 3

For part 2, we need you to demonstrate your ability to interface with the Physical Turtlebot by running Teleoperation, SLAM, and Navigation nodes. We will start by working with the robot plugged into the wall instead of using the battery. You need to make sure not to confuse which power adapter goes where.

**For more information on Turtlebot 3, please refer to lab 1 and lab 2 powerpoint materials on Canvas.We have also printed them out in the room for you.**

1. Power up the turtlebot by plugging in **12V power** to the turtlebot3’s front OpenCR board and **turning on the power switch** next to the power jack. The Turtlebot should power up. **Make sure you plug correct power supply to correct board**. We have **color coded the power jacks so that white 12V plugs to the front and yellow 19v supply plugs to the back**.
2. The green LED on the OpenCR board will light up, and the motors on the wheels and robot arm will start to feel stiff.
3. Plug in your group’s microSD card to the micro SD extender, which is connected to the Nvidia Jetson at the back of the turtlebot.
4. Plug in the **19v power jack** for the Nvidia Jetson.
5. Nvidia Jetson will start to emit green light, and the lidar sensor, which is connected to the Jetson’s USB port, should start spinning. If not, most likely it is not reading the SD card. You might need to reseat the microsd extender to jetson nx's back. Reseat the extender and sd card and go back to step 3.
6. You may then interface with Nvidia Jetson by plugging in hdmi and usb c cable between Jetson and Flipbook (Laptop-like portable monitor + keyboard battery unit).

Please note that we will not use a Docker environment on Jetson NX to minimize overhead. Execute all instructions tagged with **[Turetlebot Jetson]** on the Jetson NX terminal window directly.

Refer to the appendix for optional steps to reflash the SD card yourself if you think it is corrupt. You can also ask the DIA for assistance in reflashing the SD card.

**Self-help steps**

If you suspect Jetson is not booting up, most likely its bad connection with the micro SD card. Unplug the power, try reseating the SD card extender on the Nvidia Jetson, and put the power jack back in to reboot the Jetson. Otherwise, you can try to bypass the SD card extender and directly insert the SD card into the Jetson.

If you do not feel the motors on the turtlebot 3 to be stiff and if it does not respond to your commands later on the assignment, you might have reversed step 3 and 1 and have boot jetson first followed by turtlebot3's opencr. When that happens, openCR will power off the USB port instead of 12V, and will not be able to drive the motors. In that case, we recommend shutting both computers down and restarting from step 1.

**Jetson user and password for all groups' sd cards**

```text
User: nvidia
Password: nvidia

```

#### Teleoperation on Physical Turtlebot 

This manual is based on the following manual for Humble.

* **(Original URL)** [https://emanual.robotis.com/docs/en/platform/turtlebot3/manipulation/#simulation](https://emanual.robotis.com/docs/en/platform/turtlebot3/manipulation/#simulation)

This manual assumes you have completed Milestone 1 Part 1 on setting up your remote PC.

**[Turtlebot Jetson][Remote PC]**  Make sure Jetson is connected to the small router we provided (small_blue_wifi), along with the remote PC (NUC or PC that you configured for milestone assignment 1), so that they stay on the same local network. 

**[Turtlebot Jetson]** The following command will bring up the actual TurtleBot3 hardware with OpenMANIPULATOR-X on it. Open a terminal from the TurtleBot3 SBC.

Bring up the TurtleBot3 Manipulation using the following command.

```bash
ros2 launch turtlebot3_manipulation_bringup hardware.launch.py

```

**[Remote PC]** Enter the command below on Docker shell to launch MoveIt on RViz. You can play around in Rviz to better understand how robot arm planning works. Move blue ball around and press "plan" and "execute" buttons on the bottom left of the screen under "motion planning->planning" .

```bash
ros2 launch turtlebot3_manipulation_moveit_config moveit_core.launch.py

```

**[Remote PC]** To operate the robot with the keyboard teleoperation node, the RViz must be terminated. Then launch the servo server node and teleoperation nodes on a separate docker shell window.

```bash
ros2 launch turtlebot3_manipulation_moveit_config servo.launch.py
```

```bash
ros2 run turtlebot3_manipulation_teleop turtlebot3_manipulation_teleop

```

The following keys are used to control the TurtleBot3. Try moving the turtlebot in the physical space. 

```text
    Reading from keyboard
    ---------------------------
    Joint Control Keys:
      1/q: Joint1 +/-
      2/w: Joint2 +/-
      3/e: Joint3 +/-
      4/r: Joint4 +/-
    Use o|p to open/close the gripper.
    
    Command Control Keys:
      i: Move up
      k: Move down
      l: Move right
      j: Move left
      space bar: Move stop
    ---------------------------
    'ESC' to quit.
```

#### SLAM

This manual is based on the following manual for ROS2 Humble.

* **Original URL:** [https://emanual.robotis.com/docs/en/platform/turtlebot3/manipulation/#simulation](https://emanual.robotis.com/docs/en/platform/turtlebot3/manipulation/#simulation)

Note: This earlier section of the manual provides a better overview of SLAM. You might want to read up on the SLAM and Simulation section. Select Humble for specific instructions for Humble.

* **Original URL:** [https://emanual.robotis.com/docs/en/platform/turtlebot3/slam/](https://emanual.robotis.com/docs/en/platform/turtlebot3/slam/)

Close all terminals on the remote PC if you are coming from previous sections.

**[Turtlebot Jetson]** The following command will bring up the actual TurtleBot3 hardware with OpenMANIPULATOR-X on it. Open a terminal from the TurtleBot3 SBC.

Bring up the TurtleBot3 Manipulation using the following command. Skip this step if you are coming from the previous section and your bringup is already running.

```bash
ros2 launch turtlebot3_manipulation_bringup hardware.launch.py

```


**[Remote PC]** Launch the slam node using the following command on a docker shell on Remote PC.

```bash
ros2 launch turtlebot3_manipulation_cartographer cartographer.launch.py

```

**[Remote PC]** Open two more docker shells on Remote PC. Launch the servo server node. Launch the keyboard teleoperation node. Drive the TurtleBot3 platform to create a good “map” of the environment.

```bash
ros2 launch turtlebot3_manipulation_moveit_config servo.launch.py
```
```bash
ros2 run turtlebot3_manipulation_teleop turtlebot3_manipulation_teleop

```

**[Remote PC]** Open a new docker shell on Remote PC. Run the nav2_map_server to save the current map on RViz.

```bash
ros2 run nav2_map_server map_saver_cli -f ~/map

```

**[Optional][Remote PC]** Refer to the previous assignment’s tuning guide to tune your map, should you find it necessary while performing steps on navigation.

#### Navigation

This manual is based on the following manual for ROS2 Humble.

* **(Original URL)** [https://emanual.robotis.com/docs/en/platform/turtlebot3/manipulation/#simulation](https://emanual.robotis.com/docs/en/platform/turtlebot3/manipulation/#simulation)

Note: Simulation for vanilla turtlebot 3 and turtlebot 3 with manipulator arm that we use operate under different software packages. Small details such as file locations might be different. However, you might want to read up on the Navigation and Simulation section to get a better understanding of the entire process. Select Humble for specific instructions for Humble.

* **(Original URL)** [https://emanual.robotis.com/docs/en/platform/turtlebot3/navigation/](https://emanual.robotis.com/docs/en/platform/turtlebot3/navigation/)

Close all terminals on the remote PC if you are coming from previous sections.

**[Turtlebot Jetson]** The following command will bring up the actual TurtleBot3 hardware with OpenMANIPULATOR-X on it. Open a terminal from the TurtleBot3 SBC.

Bring up the TurtleBot3 Manipulation using the following command. Skip this step if you are coming from the previous section and your bringup is already running.

```bash
ros2 launch turtlebot3_manipulation_bringup hardware.launch.py

```

**[Remote PC]** Open a docker shell on Remote PC. Launch the navigation file using the following command. Note that you are referring to the map.yaml file created in the previous step for SLAM.

```bash
ros2 launch turtlebot3_manipulation_navigation2 navigation2.launch.py map_yaml_file:=$HOME/map.yaml

```

Click on the 2D Nav Goal button in the RViz menu GUI.

Click on the map to set the destination of the robot and drag the green arrow toward the direction where the robot will be facing.

This green arrow is a marker that can specify the destination of the robot.

The root of the arrow is the x, y coordinate of the destination, and the angle is determined by the orientation of the arrow.

As soon as x, y, and targets are set, TurtleBot3 will start moving to the destination immediately.

Recall from SLAM that you can tune SLAM parameters. You can tune Navigation parameters as well by modifying the appropriate YAML file.

**[Optional][Remote_PC]** We will leave you this URL again as a optional reference for the complete set of navigation parameters, just like we did for Assignment 1. **Please note that some parameter changes might break the software.**

[https://emanual.robotis.com/docs/en/platform/turtlebot3/navigation/#navigation](https://emanual.robotis.com/docs/en/platform/turtlebot3/navigation/#navigation)

---

### Video Demo Requirements (3-Minute Demonstration)

Refer to the introduction to the assignment submission requirement.

The demonstration must clearly show the successful completion of the following four parts in order. 

**Part A: Simulation with ROS2 CLI Tracking**
This first part demonstrates your ability to control the simulated robot and use ROS 2 CLI tools to track messages.

* **Launch Simulation:** Start the `turtlebot3_manipulation_gazebo` simulation.
* **Topic Inspection:** Open a terminal and run `ros2 topic echo /joint_states`. Ensure this terminal is clearly visible in the video.
* **Teleoperation:** In a separate terminal, use the keyboard teleoperation node to move the robotic arm.
* **Verification:** Clearly show that as you press keys to move the arm in Gazebo, the values in the `ros2 topic echo` terminal change in real-time. Demonstrate moving at least two different joints.

**Part B: Physical Robot SLAM**
This second part demonstrates mapping a real-world environment with the physical TurtleBot.

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
### Appendix

**[Optional] Nvidia Jeston NX SD Card Setup**

(This SD card preparation step for Jetson NX was already completed by the IA for you. You will receive a flashed, ready-to-go micro SD card for Turtlebot3 operation. However, if your SD card is corrupted, it would be faster to download and reflash the SD card image yourself using this guide.)

**Reflashing the micro SD card**

Download the premade image from the following link:

[https://drive.google.com/drive/folders/1ZuDb6T-RKJoFaYydzQ9XIYw8WizY3N7Y?usp=sharing](https://drive.google.com/drive/folders/1ZuDb6T-RKJoFaYydzQ9XIYw8WizY3N7Y?usp=sharing)

After downloading the image file, you should see the compressed image file.

Expand the zip file, and you should have "sd-blob.img".

There are several ways to burn the image to an SD card. You can use Raspberry Pi Imager or Ubuntu's Disks utility to write the image to an SD card. The Raspberry Pi Imager can be downloaded here:

[https://www.raspberrypi.org/software](https://www.raspberrypi.org/software)

If using Raspberry Pi Imager, please choose "Use custom" and select "sd-blob.img".

The following is the sudo user name and password for the sd card image provided.

```text
User: nvidia 
Password: nvidia

```

Please change the password after receiving your group’s micro SD card.

Please do not run `sudo apt-get upgrade` on the Nvidia Jetson. That process is not reversible, and the SD card needs to be reflashed.
