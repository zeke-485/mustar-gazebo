# HOW TO SETUP GAZEBO SIMULATION OF JUPITERROBOT IN ANOTHER PC





## NOTE: THIS IS EXPERIMENTAL
* my setup is on WSL UBUNTU 20.04.06 LTS. there might difference on the outcomes. you may try running this setup on local or virtual-machine
This entire setup might need the entire workspace in order to make sure that the system is similar to your robot
* this will update occasionally

### Suggestion
 * set the username as "mustar" ,dont forget to set up the password
 * copy the edited .bashrc file uploaded to your .bashrc in /home/mustar/
 * download the pc1.txt and paste to /home/mustar/ or recreate it in your robot. There might be differences of the packages from the uploaded pc1.txt

## IMPORTANT!!

 * install ROS NOETIC by following the instruction [here](https://wiki.ros.org/noetic/Installation/Ubuntu)
 * delete the CMakeList.txt in catkin_ws/src before setting up the workspace

## CLONE THE FILES AND WORKSPACES FROM YOUR ROBOT

 * Copy the entire workspace in /home/mustar/ (ex. catkin_ws) from robot (pc1)

 * Copy the .bashrc of the robot in /home/mustar/

 * Create the list of installed ros-noetic packages in your robot

```sh
dpkg -l | grep ^ii | grep ros-noetic | awk '{print $2}' > pc1.txt
```
 * Copy pc1.txt as reference


## SETTING UP GAZEBO IN YOUR PC


### 1. paste the copied files to your /home/user/ (ex. /home/mustar/)


### 2. ignore the packages that is related to the hardware of the robot by creating CATKIN_IGNORE files.

 * you may delete the CATKIN_IGNORE file if the packages is needed
 * you may add the touch CATKIN_IGNORE command to any of the package you don't need

```sh
touch ~/catkin_ws/src/basic_function_packages/rbx1_dynamixels/CATKIN_IGNORE
touch ~/catkin_ws/src/jupiterobot2/jupiterobot2_apps/jupiterobot2_move_grasp/CATKIN_IGNORE
touch ~/catkin_ws/src/jupiterobot2/jupiterobot2_head/jupiterobot2_head_bringup/CATKIN_IGNORE
touch ~/catkin_ws/src/jupiterobot2/jupiterobot2_arm/jupiterobot2_arm_bringup/CATKIN_IGNORE
touch ~/catkin_ws/src/jupiterobot2/jupiterobot2_voice/jupiterobot2_voice_xf/CATKIN_IGNORE
```

### 3. build the workspace

```sh
cd catkin_ws
catkin_make
```

### 4. install necessary ros-noetic packages. compare the packages from robot (pc1) and your pc (pc2)

 * Make sure to create or paste pc1.txt first
 * open text pc2-detailed-installed-list.txt in /home/user/ if you want details of your packages

```sh
#make list of installed packages in pc2 (your pc)
dpkg -l | grep ^ii | grep ros-noetic > pc2-detailed-installed-list.txt
dpkg -l | grep ^ii | grep ros-noetic | awk '{print $2}' > pc2.txt
#compare files from pc1 and pc2
comm -23 <(sort pc1.txt) <(sort pc2.txt) > missing-pkg.txt
grep -v '\-dbgsym$' missing-pkg.txt > missing-pruned.txt
#install packages from the list
sudo apt install $(cat missing-pruned.txt)
```
### 5. setup .bashrc

 * you may refer to the uploaded .bashrc or the .bashrc from your robot
 
### 6. launch gazebo


```sh
roslaunch jupiterobot2_gazebo jupiterobot2_world.launch
```

* bringup commmand is only needed on the physical robot. gazebo will launch all the necessary hardware based on urdf
* LIDAR only scan when obstacles is presented
* refer to FCOEE or jupiterrobot pdf for more commands
* learn how to create room and obstacles in gazebo like [this](https://classic.gazebosim.org/tutorials?tut=build_world)

* Examples for commands of jupiterrobot:
  * SLAM MAP BUILDING
```sh
roslaunch jupiterobot2_navigation jupiterobot2_slam.launch
```
  * checking the depth camera simulation
```sh
roslaunch astra_camera astra.launch
```
```sh
rqt
```
  * launch keyboard control
```sh
roslaunch jupiterobot2_teleop keyboard_teleop.launch
```
  * launch moveit
```sh
roslaunch jupiterobot2_moveit_config demo.launch
```


* the setup of your RVIZ might be different from your robot, you may try to setup your rviz again
* IF THERE ARE STILL ERRORS DELETE catkin_ws folder and start over. else please search another method online
* teleop joystick might not work properly as it need physical connection



## TEST THE ARM MOVEMENT
Try rostopic pub

 * Move the arm
```sh
rostopic pub /arm_group_controller/command trajectory_msgs/JointTrajectory "header:
  stamp: {secs: 0, nsecs: 0}
  frame_id: ''
joint_names: ['arm1_joint', 'arm2_joint', 'arm3_joint', 'arm4_joint']
points:
- positions: [0.5, -0.5, 0.3, -0.3]
  velocities: [0.0, 0.0, 0.0, 0.0]
  accelerations: []
  effort: []
  time_from_start: {secs: 2, nsecs: 0}"

```

 * Move the gripper
```sh
rostopic pub /gripper_group_controller/command trajectory_msgs/JointTrajectory "header:
  stamp: {secs: 0, nsecs: 0}
  frame_id: ''
joint_names: ['gripper_joint']
points:
- positions: [0.3]
  velocities: [0.0]
  accelerations: []
  effort: []
  time_from_start: {secs: 1, nsecs: 0}"

```

 * Move the head
```sh
rostopic pub /head_group_controller/command trajectory_msgs/JointTrajectory "header:
  stamp: {secs: 0, nsecs: 0}
  frame_id: ''
joint_names: ['head_joint']
points:
- positions: [0.2]
  velocities: [0.0]
  accelerations: []
  effort: []
  time_from_start: {secs: 1, nsecs: 0}"
```
## GAZEBO SIMULATION:

<img width="1920" height="1032" alt="Image" src="https://github.com/user-attachments/assets/eddad945-74c8-420f-93dc-e6478058c325" />



## MOVE-IT SIMULATION

<img width="1367" height="945" alt="Image" src="https://github.com/user-attachments/assets/b6e50038-162e-4bda-9193-f426186eea1a" />



## NAVIGATION SLAM SIMULATION

<img width="1207" height="1003" alt="Image" src="https://github.com/user-attachments/assets/fafb797e-b335-4231-a830-6cf696116a94" />
