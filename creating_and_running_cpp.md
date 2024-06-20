# Creating & Running Custom C++ programs

Before proceeding, ensure Kortex API is installed. [Kortex API instructions](./Kortex%20API%20Setup.md)

> I recommend creating custom c++ programs that use Kortex API inside `kortex/api_cpp/examples` instead of `catkin_workspace`.
> 
> Do not follow below instructions if doing so.

### 1. Navigate to src in catkin_workspace, ideally with ros_kortex.
```sh
cd ~/catkin_workspace/src
```

### 2. Create the package and set its dependencies.
```sh
catkin_create_pkg rishik_kinova roscpp rospy std_msgs actionlib_msgs kortex_driver message_generation message_runtime moveit_commander
```
Here `rishik_kinova` is the package name, `roscpp rospy ...` are the dependencies.

### 3. Create your C++ files inside the package src.
```sh
cd ~/catkin_workspace/src/rishik_kinova/src

touch hello.cpp

nano hello.cpp
```

Paste the following C++ code in hello.cpp and save the file (ctrl+o enter).
```cpp
#include <iostream>

int main() {
    using namespace std;

    cout << "Namaste World" << endl;
    return 0;
}
```

### 4. Configure CMakeLists.
Edit the CMakeLists.txt of your package to enable executables.
```sh
nano ~/catkin_workspace/src/rishik_kinova/CMakeLists.txt
```

Uncomment and set the following lines under the BUILD section. Replace `${PROJECT_NAME}` with C++ file name (Ex. hello)
```txt
add_executable(hello src/hello.cpp)

add_dependencies(hello ${hello_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})

target_link_libraries(hello
  ${catkin_LIBRARIES}
)
```

Save the file.

### 5. Make the package.
Navigate to workspace directory and run `catkin_make`.
```sh
cd ~/catkin_workspace

catkin_make
```

Alternatively, to build specific package | [official doc](http://wiki.ros.org/catkin/commands/catkin_make):
```sh
catkin_make -DCATKIN_WHITELIST_PACKAGES="rishik_kinova"
```

> To build all the packages again use `catkin_make -DCATKIN_WHITELIST_PACKAGES=""`

### 6. Update source
Restart the terminal (best) or run:
```sh
source ~/catkin_workspace/devel/setup.bash
```

## Check
Run `rospack list` and see if the package exists or not.

Example:
```sh
rospack list | grep rishik_kinova
```

Also, the autocomplete for `rosrun` should be able to find the package.

## Catkin Workspace Tree
```txt
.
├── build
├── devel
└── src
```

## Catkin Src Tree
```txt
.
├── CMakeLists.txt -> /opt/ros/melodic/share/catkin/cmake/toplevel.cmake
├── rishik_kinova
│   ├── CMakeLists.txt
│   ├── include
│   ├── package.xml
│   └── src
├── ros_kortex
│   ├── CMakeLists.txt -> /opt/ros/melodic/share/catkin/cmake/toplevel.cmake
│   ├── kortex_api
│   ├── kortex_control
│   ├── kortex_description
│   ├── kortex_driver
│   ├── kortex_examples
│   ├── kortex_gazebo
│   ├── kortex_move_it_config
│   ├── LICENSE
│   ├── readme.md
│   └── third_party
└── ros_kortex_vision
    ├── CMakeLists.txt
    ├── doc
    ├── include
    ├── launch
    ├── LICENSE
    ├── package.xml
    ├── README.md
    ├── rviz
    └── src
```