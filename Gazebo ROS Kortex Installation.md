Execute all comands below as root.
sudo -i


# ROS + Kortex + Catkin Setup

### Gazebo Citadel Install
[Source](https://gazebosim.org/docs/citadel/install_ubuntu)

```sh
apt-get install lsb-release wget gnupg

sudo sh -c 'echo "deb http://packages.osrfoundation.org/gazebo/ubuntu-stable `lsb_release -cs` main" > /etc/apt/sources.list.d/gazebo-stable.list'

wget http://packages.osrfoundation.org/gazebo.key -O - | sudo apt-key add -

apt update

apt install ignition-citadel
```

To test: `ign gazebo`


### ROS (Melodic) Install (incl. RViz, RQT, Sims)
[Source](http://wiki.ros.org/melodic/Installation/Ubuntu)

```sh
sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'

apt install curl

curl -s https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | sudo apt-key add -

apt update

apt install ros-melodic-desktop-full

echo "source /opt/ros/melodic/setup.bash" >> ~/.bashrc

source ~/.bashrc

apt install python-rosdep python-rosinstall python-rosinstall-generator python-wstool build-essential python-pip python3-pip

rosdep init

rosdep update --include-eol-distros
```



### Gazebo-ROS Integration
[Source](https://gazebosim.org/docs/citadel/ros_integration)

```sh
apt install ros-melodic-ros-ign
```



### Kortex Setup
[Source](https://github.com/Kinovarobotics/ros_kortex.git)

***Exit sudo***

```sh
cd ~

python3 -m pip install conan==1.59

rosdep update --include-eol-distros

conan config set general.revisions_enabled=1

conan profile new default --detect > /dev/null

conan profile update settings.compiler.libcxx=libstdc++11 default

mkdir -p catkin_workspace/src

cd catkin_workspace/src

git clone -b melodic-devel https://github.com/Kinovarobotics/ros_kortex.git

cd ../

rosdep install --from-paths src --ignore-src -y --rosdistro melodic

source /opt/ros/melodic/setup.bash

echo "source /opt/ros/melodic/setup.bash" >> ~/.bashrc

catkin_make

source devel/setup.bash

echo "source ~/catkin_workspace/devel/setup.bash" >> ~/.bashrc
```

#### Check bash ROS package path is correct
```sh
echo $ROS_PACKAGE_PATH
```
**Output:** /home/rishik/catkin_workspace/src:/opt/ros/melodic/share




### Test demo simulation in Gazebo & RViz

```sh
cd ~/catkin_workspace/src/ros_kortex/kortex_gazebo/launch

roslaunch kortex_gazebo spawn_kortex_robot.launch
```

**For gen3 with gripper use the following**

```sh
roslaunch kortex_gazebo spawn_kortex_robot.launch arm:=gen3 dof:=6 gripper:=robotiq_2f_140
```

> In RViz set the frame to **base_link** and click "add" then select RobotModel.




## To use ROS with Arm

[Official Guide](https://github.com/Kinovarobotics/ros_kortex/blob/melodic-devel/kortex_examples/readme.md)

### Real arm test with ROS and RViz
#### For Kinova Gen3 Lite
```sh
cd ~/catkin_workspace/src/ros_kortex/kortex_driver/launch

roslaunch kortex_driver kortex_driver.launch arm:=gen3_lite
```

**With reduced data publish rate use cyclic_data_publish_rate**
```sh
roslaunch kortex_driver kortex_driver.launch arm:=gen3_lite cyclic_data_publish_rate:=2
```

#### For Gen3 6DoF with vision and gripper

* tab 1 - launch the robot
  ```sh
  roslaunch kortex_driver kortex_driver.launch arm:=gen3 dof:=6 vision:=true gripper:=robotiq_2f_140 cyclic_data_publish_rate:=2
  ```

* tab 2 - launch the vision topics (camera & depth)
  ```sh
  roslaunch kinova_vision kinova_vision.launch
  ```

### Test commands

```sh
rostopic pub -1 /my_gen3_lite/in/joint_velocity kortex_driver/Base_JointSpeeds "joint_speeds:
- joint_identifier: 0
  value: -0.57
  duration: 0"
```

**To get realtime joint states (Real or Sim)**
```sh
rostopic echo /my_gen3_lite/joint_states
```

**To stop**
```sh
rostopic pub -1 /my_gen3_lite/in/stop std_msgs/Empty "{}"
```

### Sim arm test with CPP program

* Tab 1
  ```sh
  cd ~/catkin_workspace/src/ros_kortex/kortex_driver/launch
  roslaunch kortex_driver kortex_driver.launch arm:=gen3_lite
  ```

* Tab 2
  ```sh
  cd ~/catkin_workspace/src/ros_kortex/kortex_examples/launch
  roslaunch kortex_examples full_arm_movement_cpp.launch robot_name:=my_gen3_lite
  ```
# Gen3 Vision Setup

```sh
sudo apt install gstreamer1.0-tools gstreamer1.0-libav libgstreamer1.0-dev libgstreamer-plugins-base1.0-dev libgstreamer-plugins-good1.0-dev gstreamer1.0-plugins-good gstreamer1.0-plugins-base

sudo apt install ros-melodic-rgbd-launch
```

#### Building & Installing Kinova vision package

```sh
mkdir -p ~/catkin_vision_workspace/src

cd ~/catkin_vision_workspace/src/

git clone https://github.com/Kinovarobotics/ros_kortex_vision.git

catkin_init_workspace

cd ../

catkin_make clean

catkin_make

catkin_make install

echo "source ~/catkin_vision_workspace/devel/setup.bash" >> ~/.bashrc

source ~/.bashrc
```
