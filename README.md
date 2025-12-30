# CS 7389K (2025): Advanced Robotics and Autonomous Systems

## Programming Assignment: Milestone 2 (V1.0)

**Minhyuk Park and Tsz-Chiu Au**

### Introduction

Welcome to CS 7389K. We have prepared a few milestones before the final project, in which you will use a physical robot to execute a mission given by us. The following milestones will give you an idea of how to interact with ROBOTIS’ Turtlebot3 waffle Pi with a Manipulator Arm using the Robot Operating System 2.

The second milestone is about simulating and controlling a robot arm in Gazebo, and controlling the physical turtlebot by running teleoperation, SLAM, and Navigation nodes.

You might find the official tutorial on ROS2 Foxy useful in this course. [https://docs.ros.org/en/foxy/Tutorials.html](https://docs.ros.org/en/foxy/Tutorials.html)

### Assignment Requirement

A hardware video demonstration submission is required for Milestone Assignment 2. You need to demonstrate that you have a working setup and can operate a physical turtlebot by making a video. Refer to the demo requirement section at the end of the milestone assignment on what to include in the video. Once your group is done with the video demonstration that satisfies the demo requirement outlined at the end, please submit it to Canvas. Each group will submit one video. Rules for robot usage will apply for working with the physical Turtlebot3. Please refer to the inventory list given to you separately.

> **Change Log**
> v0.9 -> v1.0: Clarified troubleshooting steps regarding Jetson’s boot sequence

---

### Part 1: Simulation of the Robotic Arm

For part 1, we need you to further demonstrate your simulated environment with the operation of a robotic manipulator in the Gazebo simulated environment. This assumes that you have a working setup from Milestone Assignment 1 Part 1.

**How to run an Open Manipulator X Arm simulation using RViz**

This tutorial is based on the following manual. Select Foxy for the manual specific to ROS2 Foxy.

* **(Wayback Machine)** [https://web.archive.org/web/20240309202855/https://emanual.robotis.com/docs/en/platform/turtlebot3/manipulation/#simulation](https://web.archive.org/web/20240309202855/https://emanual.robotis.com/docs/en/platform/turtlebot3/manipulation/#simulation)
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

This manual is based on the following manual for Foxy.

* **(Wayback Machine)** [https://web.archive.org/web/20240309203141/https://emanual.robotis.com/docs/en/platform/turtlebot3/manipulation/#simulation](https://web.archive.org/web/20240309203141/https://emanual.robotis.com/docs/en/platform/turtlebot3/manipulation/#simulation)
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

This manual is based on the following manual for ROS2 Foxy.

* **Wayback Machine:** [https://web.archive.org/web/20240309203141/https://emanual.robotis.com/docs/en/platform/turtlebot3/manipulation/#simulation](https://web.archive.org/web/20240309203141/https://emanual.robotis.com/docs/en/platform/turtlebot3/manipulation/#simulation)
* **Original URL:** [https://emanual.robotis.com/docs/en/platform/turtlebot3/manipulation/#simulation](https://emanual.robotis.com/docs/en/platform/turtlebot3/manipulation/#simulation)

Note: This earlier section of the manual provides a better overview of SLAM. You might want to read up on the SLAM and Simulation section. Select Foxy for specific instructions for Foxy.

* **Wayback machine:** [https://web.archive.org/web/20240309202817/https://emanual.robotis.com/docs/en/platform/turtlebot3/slam/#run-slam-node](https://web.archive.org/web/20240309202817/https://emanual.robotis.com/docs/en/platform/turtlebot3/slam/#run-slam-node)
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

This manual is based on the following manual for ROS2 Foxy.

* **(Wayback Machine)** [https://web.archive.org/web/20240309203141/https://emanual.robotis.com/docs/en/platform/turtlebot3/manipulation/#simulation](https://web.archive.org/web/20240309203141/https://emanual.robotis.com/docs/en/platform/turtlebot3/manipulation/#simulation)
* **(Original URL)** [https://emanual.robotis.com/docs/en/platform/turtlebot3/manipulation/#simulation](https://emanual.robotis.com/docs/en/platform/turtlebot3/manipulation/#simulation)

Note: Simulation for vanilla turtlebot 3 and turtlebot 3 with manipulator arm that we use operate under different software packages. Small details such as file locations might be different. However, you might want to read up on the Navigation and Simulation section to get a better understanding of the entire process. Select Foxy for specific instructions for Foxy.

* **(Wayback machine)** [https://web.archive.org/web/20240309202259/https://emanual.robotis.com/docs/en/platform/turtlebot3/navigation/#run-navigation-nodes](https://web.archive.org/web/20240309202259/https://emanual.robotis.com/docs/en/platform/turtlebot3/navigation/#run-navigation-nodes)
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

### Appendix

**[Optional] Nvidia Jeston NX SD Card Setup**

(This SD card preparation step for Jetson NX was already completed by the IA for you. You will receive a flashed, ready-to-go micro SD card for Turtlebot3 operation. However, if your SD card is corrupted, it would be faster to download and reflash the SD card image yourself using step 0.)

**0) Reflashing the micro SD card**

Download the premade image from the following link:

[https://drive.google.com/drive/folders/1ZuDb6T-RKJoFaYydzQ9XIYw8WizY3N7Y?usp=sharing](https://drive.google.com/drive/folders/1ZuDb6T-RKJoFaYydzQ9XIYw8WizY3N7Y?usp=sharing)

After downloading the image file, you should see the compressed image file.

Expand the zip file, and you should have "sd-blob.img".

There are several ways to burn the image to an SD card. You can use Raspberry Pi Imager or Ubuntu's Disks utility to write the image to an SD card. The Raspberry Pi Imager can be downloaded here:

[https://www.raspberrypi.org/software](https://www.raspberrypi.org/software)

In Raspberry Pi Imager, please choose "Use custom" and select "sd-blob.img".

The following is the sudo user name and password for the sd card image provided.

```text
User: nvidia 
Password: nvidia

```

Please change the password after receiving your group’s micro SD card.

Please do not run `sudo apt-get upgrade` on the Nvidia Jetson. That process is not reversible, and the SD card needs to be reflashed.

The following instructions are from our own and not from online sources, and describe setting up the SD card from scratch. Please ask IA for assistance should you face any problems with the Jetson SD card. This part of the instruction was prepared for internal use and was not tested to determine if it is replicable by students.

**1) Install Ubuntu 20.04 LTS (64-bit, Desktop) and necessary files (Jetpack version 5.15) for Nvidia Jetson NX (Jetson Xavier NX developer kit)** by going to the following link and choosing “SD Card Image Method, Jetson Xavier NX developer kit”. You may require an Nvidia Account to download the file.

[https://developer.nvidia.com/embedded/jetpack-sdk-515](https://developer.nvidia.com/embedded/jetpack-sdk-515)

Note: If you are coming from Jetpack version 4, you have to not only flash the SD card, but also update the bootloader on the Jetson module as well to install Jetpack version 5, which disables support from Jetpack version 4 but enables support for Jetpack version 5. This was already done by the IA. Here is a relevant tutorial for this procedure.

[https://jetsonhacks.com/2022/11/03/upgrade-jetson-xavier-nx-to-jetpack-5/](https://jetsonhacks.com/2022/11/03/upgrade-jetson-xavier-nx-to-jetpack-5/)

After downloading the image file, you should see the compressed image file, which should be "jetson-nx-jp515-sd-card-image.zip". After expanding the zip file, the file should be "sd-blob.img".

There are several ways to burn the image to an SD card. You can use Raspberry Pi Imager or Ubuntu's Disks utility to write the image to an SD card. The Raspberry Pi Imager can be downloaded here:

[https://www.raspberrypi.org/software](https://www.raspberrypi.org/software)

In Raspberry Pi Imager, please choose "Use custom" and select "sd-blob.img".

**2) Insert the SD card into Jetson NX.** Hook up the Jetson NX with the portable monitor and the keyboard/mouse. Then turn on the Turtlebot by plugging in the 19V power supply. After you boot up Jetson NX, Jetson NX will ask you to enter the system configuration.

One of the screens will ask you to "Select Nvpmodel Mode".
Please select "MODE_20W_6CORE" because we want Jetson NX to run with maximum performance.

After setting the system configuration, the Jetson NX will reboot itself.

**3) Install curl on Jetson NX:**

```bash
sudo apt -y install curl

```

Then update the Ubuntu repositories:

```bash
sudo apt-get update

```

Never run `sudo apt-get upgrade`, because it would cause incompatible kernel files to be installed on the Jetson NX.

At this point, you may want to remap the shortcut keys of Copy and Paste in your terminal to speed up the following steps.

Reboot Jetson.

**4) Set up Network:**

Set up a connection to TXST_Bobcats for internet connection and Small_Blue_Wifi router for local connection with turtlebot 3.
Run `ifconfig` to see the IP of Jetson NX while connected to Small_Blue_Wifi. Remember the IP as IP_OF_Jetson.

**5) Install ROS 2 Foxy (full version).**

Please refer to [https://docs.ros.org/en/foxy/Installation/Ubuntu-Install-Debians.html](https://docs.ros.org/en/foxy/Installation/Ubuntu-Install-Debians.html)

```bash
locale  # check for UTF-8
sudo apt update && sudo apt install locales
sudo locale-gen en_US en_US.UTF-8
sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
export LANG=en_US.UTF-8
locale  # verify settings

sudo apt install software-properties-common
sudo add-apt-repository universe

sudo apt update && sudo apt install curl -y
sudo curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg

echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(. /etc/os-release && echo $UBUNTU_CODENAME) main" | sudo tee /etc/apt/sources.list.d/ros2.list > /dev/null

sudo apt update

sudo apt upgrade

sudo apt install ros-foxy-desktop python3-argcomplete

sudo apt install ros-dev-tools

source /opt/ros/foxy/setup.bash
echo "source /opt/ros/foxy/setup.bash" >> ~/.bashrc

```

**6) Install SBC packages for the Jetson**

Please refer to [https://web.archive.org/web/20240309202810/https://emanual.robotis.com/docs/en/platform/turtlebot3/sbc_setup/#sbc-setup](https://web.archive.org/web/20240309202810/https://emanual.robotis.com/docs/en/platform/turtlebot3/sbc_setup/#sbc-setup)

```bash
sudo apt install python3-argcomplete python3-colcon-common-extensions libboost-system-dev build-essential
sudo apt install ros-foxy-hls-lfcd-lds-driver
sudo apt install ros-foxy-turtlebot3-msgs
sudo apt install ros-foxy-dynamixel-sdk
mkdir -p ~/turtlebot3_ws/src && cd ~/turtlebot3_ws/src

```

Note: there is no longer a foxy-devel branch, but the latest version works fine.
`##git clone -b foxy-devel https://github.com/ROBOTIS-GIT/turtlebot3.git`

```bash
git clone https://github.com/ROBOTIS-GIT/turtlebot3.git

cd ~/turtlebot3_ws/src/turtlebot3
rm -r turtlebot3_cartographer turtlebot3_navigation2
cd ~/turtlebot3_ws/
echo 'source /opt/ros/foxy/setup.bash' >> ~/.bashrc
source ~/.bashrc
colcon build --symlink-install --parallel-workers 1
echo 'source ~/turtlebot3_ws/install/setup.bash' >> ~/.bashrc
source ~/.bashrc

```

Change USB port setting for open CR

```bash
sudo cp `ros2 pkg prefix turtlebot3_bringup`/share/turtlebot3_bringup/script/99-turtlebot3-cdc.rules /etc/udev/rules.d/
sudo udevadm control --reload-rules
sudo udevadm trigger

```

ROS Domain ID Setting In ROS2

DDS communication, ROS_DOMAIN_ID must be matched between the Remote PC and TurtleBot3 for communication under the same network environment. The following commands show how to assign a ROS_DOMAIN_ID to the SBC in TurtleBot3. A default ID of TurtleBot3 is 30. Configuring the ROS_DOMAIN_ID of the Remote PC and SBC in TurtleBot3 to 30 is recommended.

```bash
echo 'export ROS_DOMAIN_ID=30 #TURTLEBOT3' >> ~/.bashrc
source ~/.bashrc

```

LDS Configuration The TurtleBot3 LDS has been updated to LDS-02 since the 2022 models (We are using LDS-02). Please follow the instructions below on the SBC of TurtleBot3.

```bash
sudo apt update
sudo apt install libudev-dev
cd ~/turtlebot3_ws/src
git clone -b ros2-devel https://github.com/ROBOTIS-GIT/ld08_driver.git
cd ~/turtlebot3_ws && colcon build --symlink-install
echo 'export LDS_MODEL=LDS-02' >> ~/.bashrc
source ~/.bashrc

```

**7) Configure Dynamixel for the Turtlebot Arm (Open Manipulator X)**

Note: IA has already done this step in advance. This is only required to be done once for fresh Dynamixels bought from Robotis.

You need to install the relevant firmware for OpenCR for Dynamixel Wizard to work.

Download Dynamixel Wizard and set it up on your desktop. (You can configure Dynamixel on a different computer than the remote pc and NVIDIA Jetson)

[https://emanual.robotis.com/docs/en/software/dynamixel/dynamixel_wizard2/](https://emanual.robotis.com/docs/en/software/dynamixel/dynamixel_wizard2/)

The following is the configuration needed for each of the Dynamixels. It is recommended to connect and update Dynamixels one at a time, since Dynamixels with the same ID or configuration can cause conflicts in the wizard.

Note: The gripper module(ID 15) requires Current-based Position Control Mode. Please make sure your DYNAMIXEL model supports the required Operating Mode. Refer to [https://emanual.robotis.com/docs/en/platform/openmanipulator_x/quick_start_guide_basic_operation/](https://emanual.robotis.com/docs/en/platform/openmanipulator_x/quick_start_guide_basic_operation/)

* ID 1 , 2, 11, 12, 13, 14, 15
* Baudrate 1mbps
* Drive mode: 0 or 1 (depending on if ID 15 or not)
* Operating mode 1
* Protocol 2
* Return delay time 0 (or this will cause timeout-related no txrx message)

**8) Configure OpenCR for the Turtlebot Arm (Open Manipulator X)**

Please do not install OpenCR's firmware yourself, as the IA has already installed it.
There are several versions of firmware
However, if you need to update the firmware for some reason, please use the following commands:
Please refer to Foxy
[https://web.archive.org/web/20240309202534/https://emanual.robotis.com/docs/en/platform/turtlebot3/manipulation/#manipulation](https://web.archive.org/web/20240309202534/https://emanual.robotis.com/docs/en/platform/turtlebot3/manipulation/#manipulation)
Download the OpenCR firmware file on Jetson NX (SBC) and upload the correct firmware with the following commands.

```bash
export OPENCR_PORT=/dev/ttyACM0
export OPENCR_MODEL=turtlebot3_manipulation
rm -rf ./opencr_update.tar.bz2
wget https://github.com/ROBOTIS-GIT/OpenCR-Binaries/raw/master/turtlebot3/ROS2/latest/opencr_update.tar.bz2
tar -xvf opencr_update.tar.bz2
cd ./opencr_update
./update.sh $OPENCR_PORT $OPENCR_MODEL.opencr

```

When the firmware is successfully uploaded to the OpenCR, jump_to_fw will appear on the terminal.

**9) Install SBC packages for the Turtlebot Arm (Open Manipulator X)**

Please refer to Foxy [https://web.archive.org/web/20240309202534/https://emanual.robotis.com/docs/en/platform/turtlebot3/manipulation/#manipulation](https://web.archive.org/web/20240309202534/https://emanual.robotis.com/docs/en/platform/turtlebot3/manipulation/#manipulation)

```bash
cd ~/turtlebot3_ws/src/
git clone -b foxy-devel https://github.com/ROBOTIS-GIT/turtlebot3_manipulation.git
cd ~/turtlebot3_ws && colcon build --symlink-install

```

**9) Increase the SWAP partition on Jetson NX to 6.7 for compiling heavy programs**

Refer to [https://jetsonhacks.com/2019/11/28/jetson-nano-even-more-swap/](https://jetsonhacks.com/2019/11/28/jetson-nano-even-more-swap/)

```bash
sudo gedit /etc/systemd/nvzramconfig.sh

```

Find and edit the line to the following to increase swap to 6.7GB

```bash
mem=$(((“${totalmem}” / 1  / “${NRDEVICES}”) * 1024))

```

Reboot

**10) Install OpenCV with CUDA support and Gstreamer support from source.**

We cannot use pip wheel or a preinstalled version because it is CPU-only.
Refer to this script to install OpenCV 4.6.0 with CUDA + Gstreamer support.
[https://forums.developer.nvidia.com/t/best-way-to-install-opencv-with-cuda-on-jetpack-5-xavier-nx-opencv-for-tegra/222777](https://forums.developer.nvidia.com/t/best-way-to-install-opencv-with-cuda-on-jetpack-5-xavier-nx-opencv-for-tegra/222777)

**11) Install Pytorch and torch vision**

We cannot use pip wheel because we need a specific version from NVIDIA for each of the Jetpack versions for Jetson SBCs.
Refer to [https://forums.developer.nvidia.com/t/pytorch-for-jetson/72048](https://forums.developer.nvidia.com/t/pytorch-for-jetson/72048) Jetpack 5, Pytorch v2.1.0

```bash
wget https://developer.download.nvidia.cn/compute/redist/jp/v512/pytorch/torch-2.1.0a0+41361538.nv23.06-cp38-cp38-linux_aarch64.whl
sudo apt-get install python3-pip libopenblas-base libopenmpi-dev libomp-dev
pip3 install 'Cython<3'
pip3 install numpy torch-2.1.0a0+41361538.nv23.06-cp38-cp38-linux_aarch64.whl

```

* Torch vision for PyTorch 2.1.0 => 0.16.1

```bash
sudo apt-get install libjpeg-dev zlib1g-dev libpython3-dev libopenblas-dev libavcodec-dev libavformat-dev libswscale-dev
git clone --branch v0.16.1 https://github.com/pytorch/vision torchvision   # see below for version of torchvision to download
cd torchvision
export BUILD_VERSION=0.16.1  # where 0.x.0 is the torchvision version
python3 setup.py install --user
cd ../  # attempting to load torchvision from build dir will result in import error
pip install 'pillow<7' # always needed for Python 2.7, not needed torchvision v0.5.0+ with Python 3.6

```

**12) Install TorchAudio**

We need to reinstall this from source because Nvidia tutorial installs the wrong version of TorchAudio for PyTorch.
Refer to [https://docs.pytorch.org/audio/2.5.0/build.jetson.html](https://docs.pytorch.org/audio/2.5.0/build.jetson.html) Build torchaudio

Install build tools

```bash
pip install cmake ninja

```

Install dependencies

```bash
sudo apt install ffmpeg libavformat-dev libavcodec-dev libavutil-dev libavdevice-dev libavfilter-dev

```

Build TorchAudio

```bash
pip3 uninstall -y torchaudio
git clone https://github.com/pytorch/audio
cd audio
git checkout v2.1.0
USE_CUDA=1 pip install -v -e . --no-use-pep517

```

Check the installation

```python
import torchaudio
print(torchaudio.__version__)
torchaudio.utils.ffmpeg_utils.get_build_config()

```

**13) Install Text To Speech**

```bash
pip install espeak

```

**14) Install ONNX Runtime (Used for running quantized neural network models)**

Refer to [https://elinux.org/Jetson_Zoo#ONNX_Runtime](https://elinux.org/Jetson_Zoo#ONNX_Runtime) Jetpack 5.1.2 onnx runtime 1.18.0, python 3.8

```bash
# Download pip wheel from location above for your version of JetPack
wget https://nvidia.box.com/shared/static/3fechcaiwbtblznlchl6dh8uuat3dp5r.whl
# Install pip wheel
pip3 install onnxruntime_gpu-1.16.0-cp38-cp38-linux_aarch64.whl

```

**15 ) Install Hailo 8 runtime (For running Hailo 8 AI chip) on Jetson**

Refer to this for installation [https://community.hailo.ai/t/how-can-i-connect-a-hailo-8-8l-to-my-pc-or-laptop/179/17](https://community.hailo.ai/t/how-can-i-connect-a-hailo-8-8l-to-my-pc-or-laptop/179/17)
Go to the Hailo developer zone. You need to make an account and sign in. Select AI software suit-> Hailo RT ->ARM64->Linux-> Python 3.8. Download 3 files: HailoRT-python package for Python 3.8, HailoRT- PCIe driver Ubuntu package, HailoRT- Ubuntu package(deb) for arm64
[https://hailo.ai/developer-zone/software-downloads/](https://hailo.ai/developer-zone/software-downloads/)

Install build-essential and dkms

```bash
sudo apt update
sudo apt install build-essential dkms -y

```

Install pcie driver for Hailo

```bash
sudo dpkg -i hailort-pcie-driver_*.deb

```

Reboot Jetson

```bash
sudo reboot

```

Install hailort, python wheel

```bash
# The exact filename may vary based on the version you downloaded
sudo dpkg -i hailo-runtime_*.deb
# Install the HailoRT Python wheel
# The exact filename will vary based on version and Python version
pip3 install hailort_*.whl

```

Verify installation by running some hailo command line tools

```bash
hailo status
hailortcli scan
hailo fw-control identify

```

**16 ) Install YOLO v 11**

Jetson TensorRT binding

```bash
sudo apt update
# This tells apt-get to ask dpkg to force-overwrite any conflicting files
sudo apt-get -o Dpkg::Options::="--force-overwrite" install \
    libpostproc-dev python3-libnvinfer python3-libnvinfer-dev -y

```

Yolo v11

```bash
sudo apt update
sudo apt install python3-pip -y
pip install -U pip
pip install ultralytics[export]

```

**17 ) Install LLM  (LLAMA) and whisper**

LLaMa, Whisper

```bash
git clone https://github.com/ggerganov/whisper.cpp && cd whisper.cpp && make
git clone https://github.com/ggerganov/llama.cpp
cd llama.cpp 
sudo apt-get update
sudo apt-get install -y build-essential cmake
mkdir -p build
cd build
cmake -DCMAKE_BUILD_TYPE=Release ..
cmake --build . --parallel
make
pip install TTS sounddevice

```

PortAudio

```bash
sudo apt-get update
sudo apt-get install libportaudio2 libportaudiocpp0 portaudio19-dev
pip install --upgrade --force-reinstall sounddevice

```

Download a 4-bit‐quantized GGML model

```bash
wget https://huggingface.co/meta-llama/Meta-Llama-2-3B-Instruct/resolve/main/ggml-model-q4_0.bin

```
