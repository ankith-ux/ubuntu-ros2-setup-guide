# 5. Basic Commands & Packages

As a beginner in ROS 2, you will spend a lot of time in the terminal. Here is a cheat sheet of the most important commands you should memorize.

---

## 🏗️ Workspace & Build Commands

* **`colcon build`**: Compiles everything in your workspace. Must be run from the root of your workspace (`~/ros2_ws`).
* **`colcon build --packages-select <package_name>`**: Builds only a specific package instead of the whole workspace (saves a lot of time!).
* **`source install/setup.bash`**: Updates your terminal to know about the newly built packages. Must be run from the root of your workspace after building.

---

## 🏃 Running Nodes & Launch Files

* **`ros2 run <package_name> <executable_name>`**: Starts a single ROS 2 node.
  * *Example:* `ros2 run turtlesim turtlesim_node`
* **`ros2 launch <package_name> <launch_file_name>.launch.py`**: Starts a launch file, which can start multiple nodes and configure parameters at once.
  * *Example:* `ros2 launch nav2_bringup bringup_launch.py`

---

## 🔎 Introspection (Checking what's going on)

When your robot isn't doing what it's supposed to do, these are the commands you use to debug.

### Nodes
* **`ros2 node list`**: Lists all currently running nodes.
* **`ros2 node info /<node_name>`**: Shows details about a specific node (what topics it subscribes to/publishes).

### Topics
* **`ros2 topic list`**: Lists all active topics.
* **`ros2 topic echo /<topic_name>`**: Prints the data being published on a topic to your terminal in real-time.
* **`ros2 topic hz /<topic_name>`**: Shows the publishing frequency (Hertz) of a topic.
* **`ros2 topic info /<topic_name>`**: Shows the message type of the topic and the number of publishers/subscribers.

### Packages
* **`ros2 pkg list`**: Lists all installed ROS 2 packages.
* **`ros2 pkg create --build-type ament_python <package_name>`**: Creates a new Python ROS 2 package.

---

## 🐢 The Turtlesim Tutorial (Your First Try)

To practice these commands, open a few terminals and try the classic `turtlesim` tutorial.

1. **Terminal 1:** `ros2 run turtlesim turtlesim_node` (Starts the simulator).
2. **Terminal 2:** `ros2 run turtlesim turtle_teleop_key` (Allows you to drive the turtle with arrow keys).
3. **Terminal 3:** `ros2 topic echo /turtle1/cmd_vel` (Watch the velocity commands being sent as you press the keys).
