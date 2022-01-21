<h1 align="center">
  <br>
 <img src="https://github.com/Vamshi2198/Map-My-World/blob/main/src/images/Project-Title.png">
  <br>
</h1>
  
<h2 align="center">Deploy RTAB-Map on ROSbot to create 2D and 3D maps of aws simulated small house world!</h2>
  
<p align="center">
  <a href="https://www.udacity.com/robotics">
     <img src="https://s3-us-west-1.amazonaws.com/udacity-robotics/Extra+Images/RoboND_flag.png">
  </a>
  <a href="https://husarion.com/manuals/rosbot/">
     <img src="https://github.com/Vamshi2198/Go-Chase-it-/blob/main/src/images/husarion.jpg" width = "100" height = "50" >
  </a>
  <a href="https://aws.amazon.com/robomaker/">
     <img src="https://github.com/Vamshi2198/Go-Chase-it-/blob/main/src/images/aws.png" width = "100" height = "50">
  </a>
</p>

<p align="center">
  <a href="#overview">Overview</a> •
  <a href="#prerequisites">Prerequisites</a> •
  <a href="#directory-structure">Directory Structure</a> •
  <a href="#how-to-launch">How To Launch</a> •
  <a href="#testing">Testing</a>
</p>

## Overview  
This project is a part of Udacity's Robotics Software Engineer Nanodegree Program. In this project, I used [ROSbot](https://github.com/husarion/rosbot_description) as a mobile robot and [aws-robomaker-bookstore-world](https://github.com/aws-robotics/aws-robomaker-small-house-world) as a gazebo world to replicate realistic simulation. RTAB-Map (Real-Time Appearance-Based Mapping) is a popular solution for SLAM to develop robots that can map environments in 3D. RTAB-Map has good speed and memory management, and it provides custom developed tools for information analysis. Most importantly, the quality of the documentation on ROS Wiki (http://wiki.ros.org/rtabmap_ros) is very high. This project uses the rtabmap_ros package, which is a ROS wrapper (API) for interacting with RTAB-Map.

In this project:
* ROSbot is used  to interface with rtabmap_ros package because it consits of all the essential sensors namely RGB-D camera and 2D laser scanner.
* using teleop node, ROSbot is moved around the room to generate a proper map of the environment.

## Prerequisites
* Gazebo >= 7.0  
* ROS >= Kinetic
* ROS rtabmap-ros package 
```
sudo apt-get install ros-${ROS_DISTRO}-rtabmap-ros
```
* make >= 4.1(mac, linux), 3.81(Windows)
  * Linux: make is installed by default on most Linux distros
  * Mac: [install Xcode command line tools to get make](https://developer.apple.com/xcode/features/)
  * Windows: [Click here for installation instructions](http://gnuwin32.sourceforge.net/packages/make.htm)
* gcc/g++ >= 5.4
  * Linux: gcc / g++ is installed by default on most Linux distros
  * Mac: same deal as make - [install Xcode command line tools](https://developer.apple.com/xcode/features/)
  * Windows: recommend using [MinGW](http://www.mingw.org/)


## Directory Structure  
```
.Map-My-World                                  # Map-My-World Project
├── catkin_ws                                  # Catkin workspace
│   ├── src
│   │   ├── aws-robomaker-small-house-world    # folder that contains small-house world
│   │   ├── images 
│   │   ├── my_robot                           # my_robot package        
│   │   │   ├── launch                         # launch folder for launch files  
│   │   │   │   ├── mapping.launch
│   │   │   │   ├── robot_description.launch
│   │   │   │   ├── rtabmap.db                 #database
│   │   │   │   ├── world.launch               # Launches bookstore world
│   │   │   │   ├── teleop.launch              # To drive the rosbot
│   │   │   ├── meshes                         # meshes folder for sensors
│   │   │   │   ├── astra.stl
│   │   │   │   ├── box.stl
│   │   │   │   ├── rplidar.stl
│   │   │   │   ├── upper.stl
│   │   │   │   ├── wheel.stl
│   │   │   ├── realsense2_camera              # folder that contains launch files for realsense camera
│   │   │   ├── realsense2_description         # folder that contains description files for realsense camera
│   │   │   ├── urdf                           # urdf folder for xarco files
│   │   │   │   ├── materials.xacro            #contains material properties used in rosbot
│   │   │   │   ├── my_robot.xacro             
│   │   │   │   ├── rosbot.gazebo              #contains plugins to interact with rosbot
│   │   │   ├── worlds                         # world folder for world files
│   │   │   │   ├── empty.world
│   │   │   ├── CMakeLists.txt                 # compiler instructions
│   │   │   ├── package.xml                    # package info
```
## How To Launch

#### Clone the project in catkin_ws/src/ and source the environment
```sh
$ cd /home/workspace/catkin_ws/src/
$ git clone https://github.com/Vamshi2198/Map-My-World
$ source /opt/ros/${ROS_DISTRO}/setup.bash
```
#### Note : The world file proivided is empy because it only contains the url of remote repository, for this purpose you need to clone the aws-bookstore-world and place it inside your src folder. Also, delete the folder named aws-robomaker-bookstore-world manually before cloning.
```sh
$ cd /home/workspace/catkin_ws/src/Where-am-I/src/
$ git clone https://github.com/aws-robotics/aws-robomaker-small-house-world
```
#### Also, repeat the same with teleop_twist_keyboard packages. i.e, remove the empty file folder and clone the packages
```sh
$ cd /home/workspace/catkin_ws/src/Where-am-I/src/
$ git clone https://github.com/ros-teleop/teleop_twist_keyboard
```
#### Build the `Where-am-I` project
```sh
$ cd /home/workspace/catkin_ws/ 
$ catkin_make
```
#### After building the package, source your workspace
```sh
$ cd /home/workspace/catkin_ws/
$ source devel/setup.bash
```
#### Launch my_robot in Gazebo
```sh
$ roslaunch my_robot world.launch
```
#### Launch teleop node in new terminal
```sh
$ cd /home/workspace/catkin_ws/
$ source devel/setup.bash
$ rosrun teleop_twist_keyboard teleop_twist_keyboard.py
```
#### Launch rtabmap node in new terminal
```sh
$ cd /home/workspace/catkin_ws/
$ source devel/setup.bash
$ roslaunch my_robot mapping.launch
```

## Testing
* Send move command via teleop package to control your robot and observe real-time visualization in the environment with `rtabmapviz`.
* Once you satisfied, view rtabmap-databaseViewer with:
```sh
$ rtabmap-databaseViewer ~/.ros/rtabmap.db
```
* Remember to rename your ~/.ros/rtabmap.db before your next attempt since it will be deleted due to the launch file setting in mapping.launch

The code was tested on the following specifications:
- **Processor:** `Intel Core i7-10875H`
- **Graphics:** `Nvidia GeForce GTX 1650 Ti 4GB GDDR6`
- **OS:** ` Ubuntu 20.04.3 LTS`
- **Kernal:** `5.10.60.1-microsoft-standard-WSL2`
- **ROS:** `noetic`

