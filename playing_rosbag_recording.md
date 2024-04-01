# rosbag play

Terminal tab #1
```sh
roslaunch kortex_gazebo spawn_for_playback.launch arm:=gen3 dof:=6 gripper:=robotiq_2f_140 use_trajectory_controller:=false --master-logger-level=error
```

Terminal tab #2
```sh
rosbag play pickPlaceTask_ball_2.bag -d 2 --clock --loop --quiet
```

***NOTE: PointCloud Visualisation and 3D model pose in RViz may not work properly for unknown reasons, pressing reset fixes it temporarily.***

> Enabling "Experimental" and selecting "Approximate" Synchronization helps in fixing visualisation issues in RViz.

