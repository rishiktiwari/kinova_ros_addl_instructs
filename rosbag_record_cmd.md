# Record arm using rosbag

[[rosbag docs]](http://wiki.ros.org/rosbag/Commandline)

To record essential topics of Kinova Gen3 arm with vision and gripper module

```sh
rosbag record \
/camera/color/camera_info \
/camera/color/image_raw \
/camera/depth/camera_info \
/camera/depth/image \
/camera/depth_rectify_depth/parameter_descriptions \
/camera/depth_rectify_depth/parameter_updates \
/camera/depth/points \
/my_gen3/arm_state_topic \
/my_gen3/base_feedback \
/my_gen3/base_feedback/joint_state \
/my_gen3/control_mode_topic \
/my_gen3/execute_trajectory/cancel \
/my_gen3/execute_trajectory/feedback \
/my_gen3/execute_trajectory/goal \
/my_gen3/execute_trajectory/result \
/my_gen3/execute_trajectory/status \
/my_gen3/joint_states \
/my_gen3/in/cartesian_velocity \
/my_gen3/in/clear_faults \
/my_gen3/in/emergency_stop \
/my_gen3/in/joint_velocity \
/my_gen3/in/stop \
/my_gen3/kortex_error \
/my_gen3/mapping_info_topic \
/my_gen3/robotiq_2f_140_gripper_controller/gripper_cmd/cancel \
/my_gen3/robotiq_2f_140_gripper_controller/gripper_cmd/feedback \
/my_gen3/robotiq_2f_140_gripper_controller/gripper_cmd/goal \
/my_gen3/robotiq_2f_140_gripper_controller/gripper_cmd/result \
/my_gen3/robotiq_2f_140_gripper_controller/gripper_cmd/status \
/my_gen3/servoing_mode_topic \
/my_gen3/trajectory_execution_event \
/tf \
/tf_static \
--buffsize=4096 \
--chunksize=1024 \
--output-name=bag_file_name_here
```

> replace `bag_file_name_here` with the desired bag file name to save as.