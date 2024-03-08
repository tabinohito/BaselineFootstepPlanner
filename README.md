# [BaselineFootstepPlanner](https://github.com/isri-aist/BaselineFootstepPlanner)
Humanoid footstep planner based on baseline methods with graph search

[![CI](https://github.com/isri-aist/BaselineFootstepPlanner/actions/workflows/ci-standalone.yaml/badge.svg)](https://github.com/isri-aist/BaselineFootstepPlanner/actions/workflows/ci-standalone.yaml)
[![CI](https://github.com/isri-aist/BaselineFootstepPlanner/actions/workflows/ci-catkin.yaml/badge.svg)](https://github.com/isri-aist/BaselineFootstepPlanner/actions/workflows/ci-catkin.yaml)
[![Documentation](https://img.shields.io/badge/doxygen-online-brightgreen?logo=read-the-docs&style=flat)](https://isri-aist.github.io/BaselineFootstepPlanner/)

## Install

### Requirements
- Compiler supporting C++17
- Tested on `Ubuntu 20.04 / ROS Noetic` and `Ubuntu 18.04 / ROS Melodic`

### Dependencies
This package depends on
- [SBPL](https://github.com/sbpl/sbpl) (automatically installed by `rosdep install`)

### Installation procedure
It is assumed that ROS is installed.

1. Setup catkin workspace.
```bash
$ mkdir -p ~/ros/ws_bfp/src
$ cd ~/ros/ws_bfp
$ wstool init src
$ wstool set -t src isri-aist/BaselineFootstepPlanner git@github.com:isri-aist/BaselineFootstepPlanner.git --git -y
$ wstool update -t src
```

2. Install dependent packages.
```bash
$ source /opt/ros/${ROS_DISTRO}/setup.bash
$ rosdep install -y -r --from-paths src --ignore-src
```

3. Build a package.
```bash
$ catkin build baseline_footstep_planner -DCMAKE_BUILD_TYPE=RelWithDebInfo --catkin-make-args all tests
```

## Examples
Make sure that it is built with `--catkin-make-args tests` option.

### Interactive planning
```
$ roslaunch baseline_footstep_planner footstep_planner.launch
```
https://user-images.githubusercontent.com/6636600/187672008-fb93fb0e-5ec0-4054-a31d-68ce6c884005.mp4

### Standalone planning
```
$ rosrun baseline_footstep_planner TestFootstepPlanner
```

## Integration into controller
The footstep planner in this library is available in the humanoid controller [BaselineWalkingController](https://github.com/isri-aist/BaselineWalkingController).

## SETTING


<details> <summary>Details of yaml Settings</summary><div>

### Example of [yaml file](/config/FootstepPlanner.yaml)　　

### Parameters
- theta_divide_num  
    Division number to discretize orientation
    ```
    theta_divide_num: 64
    ```
- xy_divide_step  
    Step to discretize the XY position
    ```
    xy_divide_step: 0.01 # [m]
    ```

- cost_scale  
    This is the scale by which cost is multiplied before rounding it to an int-type value.
    ```
    cost_scale: 1.0e3
    ````
- cost_theta_scale  
    Scale for converting orientation distance to position distance in cost calculation [m/rad]
    ```
    cost_theta_scale: 0 # [m/rad]
    ```
- step_cost  
    Cost of one step (unit correspond to [m])
    ```
    step_cost: 1.0
    ```

- heuristic_type  
    Heuristics type : DijkstraPath, EuclideanDistance
    ```
    heuristic_type: DijkstraPath
    ```

- dijkstra_path_heuristic_expand_scale  
    Expansion scale of grid map for Dijkstra path based heuristics
    ```
    dijkstra_path_heuristic_expand_scale: 5.0
    ```

- nominal_foot_separation  
    Nominal distance between left and right feet [m]
    ```
    nominal_foot_separation: 0.2
    ```

- r2l_action_cont_list  
    List of feasible footsteps of left foot relative to right foot   
    x[m], y[m], [rad]  
    ```
    r2l_action_cont_list:
    [
    # Able to step front 0.2m from right foot to left foot
    [0.2, 0, 0], 
    # Able to step back 0.1m from right foot to left foot
    [-0.1, 0, 0],
    # Able to step right side 0.1m from right foot to left foot
    [0, 0.1, 0],
    # Able to step left side 0.05m from right foot to left foot
    [0, -0.05, 0],
    # Able to step above the ground 0.349m from right foot to left foot
    [0, 0, 0.349],
    # Able to step above the ground -0.349m from right foot to left foot
    [0, 0, -0.349]
    ]
    ```

- SET Obstacle
    It's select 4 vertex.(Only support Cube shape)
    ```
    rect_obstacle_list: # [m]
    [ 
    [1.0, 0, 0.2, 1.0],
    [0, 1.0, 1.0, 0.2]
    ]

    ```
</details>
  