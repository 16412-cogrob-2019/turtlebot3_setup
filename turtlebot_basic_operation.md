# Interact with Turtlebots
#cognitiverobotics/2019

*NOTE: These instructions assume you have a Linux machine running natively or are running on the lab computer. (I haven’t been able to get a VM connect to the Turtlebots).

## Install Turtlebot Packages
(After you have install ROS Kinetic)
1. Install dependencies: `sudo apt-get install ros-kinetic-joy ros-kinetic-teleop-twist-joy ros-kinetic-teleop-twist-keyboard ros-kinetic-laser-proc ros-kinetic-rgbd-launch ros-kinetic-depthimage-to-laserscan ros-kinetic-rosserial-arduino ros-kinetic-rosserial-python ros-kinetic-rosserial-server ros-kinetic-rosserial-client ros-kinetic-rosserial-msgs ros-kinetic-amcl ros-kinetic-map-server ros-kinetic-move-base ros-kinetic-urdf ros-kinetic-xacro ros-kinetic-compressed-image-transport ros-kinetic-rqt-image-view ros-kinetic-gmapping ros-kinetic-navigation ros-kinetic-interactive-markers`
2. In your home folder, 
```
$ mkdir catkin_ws && cd catkin_ws
$ mkdir src && cd src
$ git clone https://github.com/ROBOTIS-GIT/turtlebot3_msgs.git
$ git clone https://github.com/ROBOTIS-GIT/turtlebot3.git
$ cd ~/catkin_ws && catkin_make
```

3. Add the following line to the `~/.bashrc` file
```
source ~/catkin_ws/devel/setup.bash
```
5. Our Turtlebot is unique because we often run multiple at the same time. You will need to go to our GitHub organization and add the launch files from the “[turtlebot3_setup](https://github.com/16412-cogrob-2019/turtlebot3_setup)” repo to the correct launch folders in the Turtlebot package.

## Connect to the bot
1. Install Telegram (on your phone or computer). This will allow access to the bot’s IP address. 
	* Use @MERSTurtlebot for Turtlebot 1, @MERSTurtle2bot for Turtlebot2, etc.
2. Turn on TurtleBot and query for IP address by sending `/ip` to the bot.
3. Open a terminal and type `ssh pi@IP_ADDRESS`
	* Password:  `turtlebot`

## Configure connection
You need to make sure that the turtlebot knows your computer is its master!
	* On your computer (i.e. [Remote PC]):
		1. Run `ifconfig` to get your IP address
		2. Add the following lines to your .bashrc file (`nano ~/.bashrc`):
```
		ROS_MASTER_UI=http://YOUR_IP_ADDRESS:11311
		ROS_HOSTNAME=YOUR_IP_ADDRESS
```
		3. Restart your terminal or run `source ~/.bashrc`
	* On the Turtlebot:
		1. Change the following line of the .bashrc file (`nano ~/.bashrc`):
```
		ROS_MASTER_UI=http://YOUR_IP_ADDRESS:11311
```

## Bring Up TurtleBot
1. [Remote PC] `roscore`
2. [Turtlebot] `roslaunch turtlebot3_bringup turtlebot3_robot_multi.launch ns:=tb3_#`
	* where “#” is the number of the Turtlebot you are trying to run. We have "zero indexed" the turtlebots. The turtlebot label "1" has namespace "tb3_0" and so on.
3. To see if you are connected, run `rostopic list` on you remote pc in a new terminal
	* You should see some Turtlebot messages!

## Teleoperation
You can drive the robot around with your keyboard!
0. Be sure you have done step 3 from “Install Turtlebot Packages”. 
1. Bring up Turtlebot as in steps above.
2. [Remote PC] (In new terminal): `roslaunch turtlebot3_teleop turtlebot3_teleop_key_multi.launch ns=:tb3_#`

## Camera (only Turtlebot 1)
1. [Turtlebot]  `roslaunch turtlebot3_bringup turtlebot3_robot_multi_camera.launch ns:=tb3_0`
2. [Remote PC] (in new terminal)  `rqt_image_view`
	* This will bring up the raw stream of video from the camera in a UI. Use the dropdown menu to select the feed and use the save button to save a frame.
3. [Remote PC] (in new terminal)  `rosrun rqt_reconfigure rqt_reconfigure` to reconfigure the camera
4. See documentation here: [GitHub - UbiquityRobotics/raspicam_node: ROS node for camera module of Raspberry Pi](https://github.com/UbiquityRobotics/raspicam_node)
	* Your ROS node can listen to the image topics to get the images in real time 
