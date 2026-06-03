  # OmniDrive-Robots — Hologlyph Bot (eYRC 2023-24)

  A ROS 2 + Gazebo simulation of a **3-wheeled omnidirectional (holonomic) robot** built for the [e-Yantra Robotics Competition 
  
  The robot navigates a Gazebo world by tracing geometric shapes (square, triangle, rectangle) using a client-server waypoint
  architecture.
  
  ---

  ## Demo

  [![OmniDrive-Robots Demo](https://img.youtube.com/vi/OwL2B2hbDH4/0.jpg)](https://youtu.be/OwL2B2hbDH4)
  
  > Click the thumbnail to watch the demo on YouTube.
  
  ---
  
  ## How It Works

  A **service node** randomly selects a geometric shape at startup and generates its waypoint coordinates. A **controller node**
  sequentially requests the next goal pose `(x, y, θ)` from the service and drives the robot to each waypoint until the full shape is
  traced.

  Controller Node  →  /next_goal (service)  →  Service Node
                   ←  (x_goal, y_goal, theta_goal)

  ---

  ## Tech Stack

  | Layer | Technology |
  |---|---|
  | Robotics middleware | ROS 2 (ament_python) |
  | Simulation | Gazebo |
  | Robot description | URDF / Xacro |
  | Language | Python 3 |
  | Numerics | NumPy |
  | ROS interfaces | `std_msgs`, `rclpy`, custom `my_robot_interfaces` |

  ---

  ## Robot Specs

  - **Drive type:** 3-wheel omnidirectional (holonomic)
  - **Wheels:** Right (blue), Left (orange), Back (green) — placed 120° apart
  - **Wheel radius:** 0.14 m | **Base height:** 0.28 m
  - **Gazebo plugin:** `libgazebo_ros_planar_move` — subscribes to `/cmd_vel`, publishes odometry to `/odom`

  ---

  ## Project Structure

  OmniDrive-Robots/
  ├── hb_task_1b/             # Python package
  ├── launch/
  │   ├── gazebo.launch.py        # Launches Gazebo simulation
  │   └── hb_task1b.launch.py     # Launches controller + service nodes
  ├── meshes/
  │   └── base.dae                # Robot base 3D mesh
  ├── scripts/
  │   ├── service_node.py         # Waypoint service node (shape selection + goal generation)
  │   └── controller.py           # Robot motion controller
  ├── urdf/
  │   ├── hb_bot.urdf.xacro       # Full robot URDF description
  │   └── materials.xacro         # Gazebo material colors
  ├── worlds/
  │   └── gazebo.world            # Gazebo simulation world
  ├── package.xml
  ├── setup.py
  └── setup.cfg

  ---

  ## Prerequisites

  - ROS 2 (Humble or later)
  - Gazebo (compatible with your ROS 2 version)
  - Python 3 with NumPy
  - Custom ROS interface package: `my_robot_interfaces`

  ---

  ## Setup & Running

  ```bash
  # 1. Clone the repo into your ROS 2 workspace
  cd ~/ros2_ws/src
  git clone https://github.com/shloksharma273/OmniDrive-Robots.git

  # 2. Build the workspace
  cd ~/ros2_ws
  colcon build --packages-select hb_task_1b
  source install/setup.bash

  # 3. Launch the Gazebo simulation
  ros2 launch hb_task_1b gazebo.launch.py

  # 4. In a new terminal, launch the nodes
  ros2 launch hb_task_1b hb_task1b.launch.py

  ---
  ROS Topics & Services

  ┌────────────┬────────────────────────────────────────┬────────────────────────────────┐
  │    Name    │                  Type                  │          Description           │
  ├────────────┼────────────────────────────────────────┼────────────────────────────────┤
  │ /cmd_vel   │ geometry_msgs/Twist                    │ Velocity commands to the robot │
  ├────────────┼────────────────────────────────────────┼────────────────────────────────┤
  │ /odom      │ nav_msgs/Odometry                      │ Robot odometry from Gazebo     │
  ├────────────┼────────────────────────────────────────┼────────────────────────────────┤
  │ /next_goal │ my_robot_interfaces/NextGoal (service) │ Request the next waypoint      │
  ├────────────┼────────────────────────────────────────┼────────────────────────────────┤
  │ /shape     │ std_msgs/String                        │ Name of the shape being traced │
  └────────────┴────────────────────────────────────────┴────────────────────────────────┘

  ---
  Competition Context

  This project was developed for the eYRC 2023-24 HB (Hologlyph Bot) theme by the e-Yantra Robotics Competition, IIT Bombay. The task
  involves autonomous shape-tracing using holonomic drive kinematics in a simulated environment.
