# mustar-gazebo
!!!!!IMPORTANT!!!!!
*suggestion: set the username for ubuntu as mustar (run ubuntu on WSL,Virtual Machine or Local) don't forget to setup the password
*install ROS NOETIC BEFORE setup
*delete CMakeList.txt in catkin_ws/src BEFORE setup
*optional: copy the .bashrc code to .bashrc in /home/mustar/ starting from line 119 to 187 AFTER setup
!!!!!IMPORTANT!!!!!
Download pc1.txt,README,og-mustar.zip or new-mustar.zip

METHOD 1:
1.extract og-mustar.zip and copy the files to /home/mustar/ in UBUNTU
NOTE: make sure to delete CMakeList.txt in catkin_ws/src

1. set catkin_ignore to these packages in catkin_ws/src. these packages will be skipped when running catkin_make
NOTE: PASTE THESE LINES INTO TERMINAL. delete CATKIN_IGNORE files if you don't want to skip the packages

touch ~/catkin_ws/src/basic_function_packages/rbx1_dynamixels/CATKIN_IGNORE
touch ~/catkin_ws/src/jupiterobot2/jupiterobot2_apps/jupiterobot2_move_grasp/CATKIN_IGNORE
touch ~/catkin_ws/src/jupiterobot2/jupiterobot2_head/jupiterobot2_head_bringup/CATKIN_IGNORE
touch ~/catkin_ws/src/jupiterobot2/jupiterobot2_arm/jupiterobot2_arm_bringup/CATKIN_IGNORE
touch ~/catkin_ws/src/jupiterobot2/jupiterobot2_voice/jupiterobot2_voice_xf/CATKIN_IGNORE

2. build workspace .PASTE THESE LINES INTO TERMINAL.
IF THERE IS ERROR, solve using chatgpt/ai and modify the files as little as possible

cd catkin_ws
catkin_make

3. install necessary ros-noetic packages. compare the packages from robot to yours
NOTE: PASTE THESE LINES INTO TERMINAL. open text pc2-detailed-installed-list.txt in /home/mustar if you want detail on list

#make list of installed packages in pc2 (your pc)
dpkg -l | grep ^ii | grep ros-noetic > pc2-detailed-installed-list.txt
dpkg -l | grep ^ii | grep ros-noetic | awk '{print $2}' > pc2.txt
#compare files from pc1 and pc2
comm -23 <(sort pc1.txt) <(sort pc2.txt) > missing-pkg.txt
grep -v '\-dbgsym$' missing-pkg.txt > missing-pruned.txt
#install packages from the list
sudo apt install $(cat missing-pruned.txt)

4. start gazebo
NOTE: DONT NEED TO BRINGUP. ONLY REAL ROBOT NEED BRINGUP
NOTE: Lidar only scan when obstacle is present
NOTE: rqt to check depth camera
NOTE: refer FCOEE for navigation etc.
NOTE: learn how to build obstacle/world in gazebo
NOTE: PASTE THESE LINES ON EACH DIFFERENT WINDOW

roslaunch jupiterobot2_gazebo jupiterobot2_world.launch
roslaunch jupiterobot2_navigation jupiterobot2_slam.launch
rqt
roslaunch jupiterobot2_teleop keyboard_teleop.launch

METHOD 2:
extract new-mustar.zip and paste catkin_ws to /home/mustar
cd catkin_ws
catkin_make
copy and paste .bashrc in /home/mustar/
test gazebo

IF THERE ARE STILL ERRORS DELETE catkin_ws folder and start over
