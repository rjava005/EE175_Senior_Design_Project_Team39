# EE175_Senior_Design_Project_Team39
Main Repository for all working code for Team 39s EE175 Senior Design Project. The project's goal is to design and test a tele-op/autonomous surveillance/security vehicle for searching and tracking suspicious and criminal activity.

# HOW TO RUN THIS PROJECT STEP-BY-STEP TUTORIAL
[ ON RASPBERRY PI ]
1. Open Putty -> rjava005@raspberrypi.local

2. Start Camera Stream:
	- rpicam-vid -n -t 0 \
  --width 640 --height 480 --framerate 30 \
  --codec h264 --inline --profile baseline \
  --bitrate 800000 \
  --intra 30 \
  --libav-format h264 \
  -o - | \
gst-launch-1.0 -v fdsrc ! \
  h264parse config-interval=-1 ! \
  rtph264pay pt=96 config-interval=1 ! \
  udpsink host=Roman.local port=5600 sync=false async=false

3. Start Docker:
	- docker run -it --rm --net=host \
  --privileged \
  -v /dev:/dev \
  --device=/dev/i2c-1 \
  --device=/dev/ldlidar \
  --group-add dialout \
  -v ~/ros2_ws:/ros2_ws \
  -w /ros2_ws \
  ros2-jazzy-dev bash

4. Launch Full Bringup NSodes:
	- export ROS_DOMAIN_ID=7
	- source /ros2_ws/install/setup.bash
	- ros2 launch security_bot_bringup security_bot_pi.launch.py

[ ON WINDOWS ]

1. Start YOLO UDP Overlay/Forwarding 
	- cd security_bot
	- python yolo_udp_overlay_8s.py

2. Start ROS UDP Forwarding
	- cd security_bot
	- python udp_ros_forwarder.py

3. Start Controller Forwarding [ IN ADMINISTRATOR ]
	- usbipd list
	- usbipd attach --busid [# from list] --wsl

[ ON WSL ]

1. Launch Full Bringup Nodes:
	- cd ros2_ws
	- export ROS_DOMAIN_ID=7
	- source ~/ros2_ws/install/setup.bash
	- ros2 launch security_bot_bringup security_bot_wsl.launch.py
2. rviz2: rviz2
