Execute all comands below as root: `sudo -i`


# ROS + Gazebo + ros_kortex + Catkin Setup

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
[[Original Source]](https://gazebosim.org/docs/citadel/ros_integration)

```sh
apt install ros-melodic-ros-ign
```



### Kortex Setup
[[Original Source]](https://github.com/Kinovarobotics/ros_kortex/tree/melodic-devel)

***`exit` sudo if used `sudo -i` previously for the following commands:s***

```sh
cd ~

python3 -m pip install conan==1.59

rosdep update --include-eol-distros

conan config set general.revisions_enabled=1

conan profile new default --detect > /dev/null

conan profile update settings.compiler.libcxx=libstdc++11 default

mkdir -p catkin_workspace/src

cd catkin_workspace/src
```

**Use either one of the below repositories:**

>
> **[rishik_ros_kortex](https://github.com/rishiktiwari/rishik_ros_kortex) repo (recommended):**
>> ```sh
>> git clone https://github.com/rishiktiwari/rishik_ros_kortex.git ros_kortex
>> ```
>
> **Kinova repo:**
>> ```sh
>> git clone -b melodic-devel https://github.com/Kinovarobotics/ros_kortex.git
>> ```

```sh
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
**Desired Output:** /home/rishik/catkin_workspace/src:/opt/ros/melodic/share




### Test demo simulation in Gazebo & RViz

```sh
cd ~/catkin_workspace/src/ros_kortex/kortex_gazebo/launch

roslaunch kortex_gazebo spawn_kortex_robot.launch
```

**For gen3 with gripper use the following**

```sh
roslaunch kortex_gazebo spawn_kortex_robot.launch arm:=gen3 dof:=6 gripper:=robotiq_2f_140
```

> In RViz set the frame to **base_link** and click "add" then select RobotModel, not required if using rishik_ros_kortex repo.




## To use ROS with Arm

[[Official Guide]](https://github.com/Kinovarobotics/ros_kortex/blob/melodic-devel/kortex_examples/readme.md)

### Real arm test with ROS and RViz
#### For Kinova Gen3 Lite
```sh
cd ~/catkin_workspace/src/ros_kortex/kortex_driver/launch

roslaunch kortex_driver kortex_driver.launch arm:=gen3_lite
```

With reduced data publish rate use `cyclic_data_publish_rate`
```sh
roslaunch kortex_driver kortex_driver.launch arm:=gen3_lite cyclic_data_publish_rate:=2
```

#### For Gen3 6DoF with vision and gripper

* terminal tab 1 - launch the robot
  ```sh
  roslaunch kortex_driver kortex_driver.launch arm:=gen3 dof:=6 vision:=true gripper:=robotiq_2f_140
  ```

  > Command arguments are listed [here](https://github.com/rishiktiwari/rishik_ros_kortex/blob/master/kortex_driver/readme.md#usage).

* terminal tab 2 - launch the vision topics (for RGB & depth images)
  > Make sure vision setup mentioned below is completed before running the following command.

  ```sh
  roslaunch kinova_vision kinova_vision.launch
  ```

### Test commands

The following command sets speed of joint 0 (base) to -0.57 deg/s.

Note: `duration` value is not implemented by kortex so has no effect but is required to satisfy type validation.

```sh
rostopic pub -1 /my_gen3/in/joint_velocity kortex_driver/Base_JointSpeeds "joint_speeds:
- joint_identifier: 0
  value: -0.57
  duration: 0"
```

> Use topic parent name `/my_gen3_lite` for Kinova Gen3 Lite.

**To get realtime joint states (Real or Sim)**
```sh
rostopic echo /my_gen3/joint_states
```

**To stop**
```sh
rostopic pub -1 /my_gen3/in/stop std_msgs/Empty "{}"
```

### Sim arm test with CPP program

* Tab 1
  ```sh
  cd ~/catkin_workspace/src/ros_kortex/kortex_driver/launch
  roslaunch kortex_driver kortex_driver.launch arm:=gen3
  ```

* Tab 2
  ```sh
  cd ~/catkin_workspace/src/ros_kortex/kortex_examples/launch
  roslaunch kortex_examples full_arm_movement_cpp.launch robot_name:=my_gen3
  ```
# Gen3 Vision Setup

```sh
sudo apt install gstreamer1.0-tools gstreamer1.0-libav libgstreamer1.0-dev libgstreamer-plugins-base1.0-dev libgstreamer-plugins-good1.0-dev gstreamer1.0-plugins-good gstreamer1.0-plugins-base

sudo apt install ros-melodic-rgbd-launch
```

### Building & Installing Kinova vision package

```sh
cd ~/catkin_workspace/src

git clone https://github.com/Kinovarobotics/ros_kortex_vision.git

cd ../

catkin_make
```

> To selectively make the package use `catkin_make -DCATKIN_WHITELIST_PACKAGES="ros_kortex_vision"`

**IMPORTANT:**
Restart the terminal or enter `source ~/catkin_workspace/devel/setup.bash`

<details>
<summary><b>not recommended:</b> To install kortex_vision in new catkin workspace, expand me and ignore the above instructions.</summary>
<code>
mkdir -p ~/catkin_vision_workspace/src
<br><br>
cd ~/catkin_vision_workspace/src/
<br><br>
git clone https://github.com/Kinovarobotics/ros_kortex_vision.git
<br><br>
catkin_init_workspace
<br><br>
cd ../
<br><br>
catkin_make clean
<br><br>
catkin_make
<br><br>
echo "source ~/catkin_vision_workspace/devel/setup.bash" >> ~/.bashrc
<br><br>
source ~/.bashrc
</code>
</details>