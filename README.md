# ROS 1 Goal Navigation

ROS 1 package that sends a mobile robot to a target position chosen by the user in a Gazebo simulation. It uses an action client with interactive goal management, a custom message for the robot state and a service that returns the last target.

> Second assignment (ROS 1 part) of the Research Track 1 course (1st year, 1st semester), MSc Robotics Engineering, University of Genoa. The ROS 2 part is in [ros2_gazebo_robot_controller](https://github.com/mrtelisa/ros2_gazebo_robot_controller).

The simulation, the robot and the `reaching_goal` action server (which moves the robot to the goal while avoiding obstacles) are provided by the course package [assignment_2_2024](https://github.com/CarmineD8/assignment_2_2024).

## Package structure

```
ass2_ros1/
├── scripts/
│   ├── action_client.py   # action client with user interface
│   └── service.py         # service returning the last target
├── msg/
│   └── RobotPosVel.msg    # robot position and velocity
├── srv/
│   └── Target.srv         # last target coordinates
├── launch/
│   └── ass2.launch        # simulation + both nodes
├── CMakeLists.txt
└── package.xml
```

## Nodes

### `action_client_node` (`action_client.py`)

1. Asks the user for the target coordinates (x, y), with input validation.
2. Sends the goal to the `reaching_goal` action server and stores its feedback.
3. While the robot is moving, the user can choose:
   - `q`: cancel the current goal;
   - `f`: print the latest feedback from the action server;
   - `e`: cancel the goal and exit.
4. When the goal is reached or aborted, a new target is requested.

In parallel, the node republishes the position and velocity of the robot from `/odom` on `/robot_state`.

| Type | Name | Message |
|---|---|---|
| Action client | `/reaching_goal` | `assignment_2_2024/Planning` |
| Subscriber | `/odom` | `nav_msgs/Odometry` |
| Publisher | `/robot_state` | `ass2_ros1/RobotPosVel` |

### `service_node` (`service.py`)

Provides the service `/target_service`, which returns the coordinates of the last target, read from the ROS parameters `/des_pos_x` and `/des_pos_y`.

| Type | Name | Message |
|---|---|---|
| Service server | `/target_service` | `ass2_ros1/Target` |

## Custom messages and services

```
# RobotPosVel.msg
float32 x        # position
float32 y
float32 vel_x    # linear velocity
float32 vel_z

# Target.srv
---
float32 target_x
float32 target_y
```

## Requirements

- ROS Noetic
- Gazebo
- The course package [assignment_2_2024](https://github.com/CarmineD8/assignment_2_2024)

## Installation

Clone both repositories in the `src/` folder of a catkin workspace and build it:

```bash
cd ~/catkin_ws/src
git clone https://github.com/mrtelisa/ros1_goal_navigation.git
git clone https://github.com/CarmineD8/assignment_2_2024
cd ~/catkin_ws
catkin_make
source devel/setup.bash
```

## Usage

Launch the simulation and both nodes:

```bash
roslaunch ass2_ros1 ass2.launch
```

Then enter the target coordinates in the terminal and interact with the robot while it moves (`q`, `f`, `e`).

To read the robot state or the last target:

```bash
rostopic echo /robot_state
rosservice call /target_service
```

## Branches

| Branch | Content |
|---|---|
| `main` | The assignment |
| `ass1_rt2` | First assignment of the Research Track 2 course (1st year, 2nd semester): code documentation with Sphinx (docstrings in both nodes, HTML pages in `docs/`) |
| `assignment2_rt2` | Second assignment of the Research Track 2 course: Jupyter notebook that replaces the action client, with a GUI, a live plot of the robot trajectory and goal statistics |
| `simulation` | Exam simulation: distance of the robot from a fixed point published on `/distance`, and a service `/linvel_service` to set the linear velocity of the robot |
| `exam` | Exam: robot velocity in km/h published on `/actual_velocity`, and a service `/goals_status` returning the number of goals reached and cancelled |
