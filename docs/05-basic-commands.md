# 5. Basic Commands & Packages

As a beginner in ROS 2, you will spend a lot of time in the terminal. Here is a cheat sheet of the most important commands you should memorize.

---

## 🏗️ Workspace & Build Commands

* **`colcon build --symlink-install`**: Compiles all packages in your workspace. The `--symlink-install` flag is a lifesaver for Python development—it creates symbolic links so edits to your existing Python scripts take effect without rebuilding! *(You still need to rebuild after adding a **new** file, node, launch file, or editing `setup.py`/`package.xml`. C++ code always needs a rebuild.)*
* **`colcon build --symlink-install --packages-select <package_name>`**: Builds only a single target package instead of your entire workspace (saves huge amounts of compilation time).
* **`source install/setup.bash`**: Updates the current terminal to recognize newly built packages. Always run this from `~/ros2_ws` after building a new package.
* **`rosdep install -i --from-path src --rosdistro jazzy -y`**: Installs all dependencies needed by the packages in `src/` (run from `~/ros2_ws` after cloning someone else's code).

---

## 🏃 Running Nodes & Launch Files

* **`ros2 run <package_name> <executable_name>`**: Runs an individual executable node.
  * *Example:* `ros2 run turtlesim turtlesim_node`
* **`ros2 launch <package_name> <launch_file>`**: Executes a launch file (Python, XML, or YAML), starting multiple nodes, configurations, and parameters in one go.
  * *Example:* `ros2 launch turtlesim multisim.launch.py`

---

## 🔎 Introspection & Debugging Commands

When debugging robot behavior or sensor pipelines, use these commands to inspect data in flight:

### Nodes
* **`ros2 node list`**: Lists all currently active nodes in the ROS graph.
* **`ros2 node info /<node_name>`**: Displays node details (publishers, subscribers, services, and actions).

### Topics & Messages
* **`ros2 topic list`**: Lists all currently active topics.
* **`ros2 topic echo /<topic_name>`**: Prints live data streaming over a topic directly in your terminal.
* **`ros2 topic hz /<topic_name>`**: Measures and displays the publishing rate (frequency in Hz).
* **`ros2 topic info /<topic_name>`**: Displays the message type and number of active publishers/subscribers.
* **`ros2 topic pub /<topic_name> <msg_type> '<data>'`**: Manually publishes a test message to a topic from the terminal.
* **`ros2 interface show <msg_type>`**: Shows the field structure of a message type (e.g., `ros2 interface show geometry_msgs/msg/Twist`).

### Services & Parameters
* **`ros2 service list`**: Lists active request/reply services.
* **`ros2 service call /<service_name> <srv_type> '<data>'`**: Calls a service from the terminal (e.g., `ros2 service call /clear std_srvs/srv/Empty` clears the turtlesim drawing).
* **`ros2 param list`**: Lists dynamic parameters available on active nodes.
* **`ros2 param get /<node_name> <param>`** / **`ros2 param set /<node_name> <param> <value>`**: Reads or changes a parameter while the node is running (e.g., `ros2 param set /turtlesim background_r 150`).

### Packages
* **`ros2 pkg list`**: Lists every installed package.
* **`ros2 pkg executables <package_name>`**: Lists the nodes you can `ros2 run` from a package.

### Recording Data (rosbag)
* **`ros2 bag record <topic1> <topic2>`**: Records topics to a bag folder (use `-a` to record everything). Stop with `Ctrl+C`.
* **`ros2 bag play <bag_folder>`**: Replays recorded data — great for testing code without the real robot.

### Health Check
* **`ros2 doctor`**: Checks your ROS 2 setup and network for common problems.

### Package Creation
Run these from inside `~/ros2_ws/src`:
* **Python Package**:
  ```bash
  ros2 pkg create --build-type ament_python --license Apache-2.0 <package_name> --dependencies rclpy
  ```
* **C++ Package**:
  ```bash
  ros2 pkg create --build-type ament_cmake --license Apache-2.0 <package_name> --dependencies rclcpp
  ```
*(Add `--node-name <node_name>` to also generate a ready-to-run "hello world" node.)*

---

## 🐢 The Classic Turtlesim Walkthrough

Turtlesim is the standard playground for learning ROS 2 topics and nodes.

1. **Terminal 1 (Simulator Node):**
   ```bash
   ros2 run turtlesim turtlesim_node
   ```
2. **Terminal 2 (Teleoperation Node):**
   ```bash
   ros2 run turtlesim turtle_teleop_key
   ```
   *(Keep this terminal focused and use your arrow keys to steer the turtle around)*.

3. **Terminal 3 (Echoing Velocity Commands):**
   ```bash
   ros2 topic echo /turtle1/cmd_vel
   ```
   *(You will see linear and angular velocities change live as you press keys)*.

4. **Terminal 4 (Publishing Manually):**
   ```bash
   ros2 topic pub --once /turtle1/cmd_vel geometry_msgs/msg/Twist "{linear: {x: 2.0, y: 0.0, z: 0.0}, angular: {x: 0.0, y: 0.0, z: 1.8}}"
   ```
   *(Sends a single command, so the turtle drives a short arc and stops — turtlesim stops moving if it gets no new command for about 1 second).*

   To make it drive in circles continuously, publish repeatedly at 1 Hz instead (stop with `Ctrl+C`):
   ```bash
   ros2 topic pub --rate 1 /turtle1/cmd_vel geometry_msgs/msg/Twist "{linear: {x: 2.0, y: 0.0, z: 0.0}, angular: {x: 0.0, y: 0.0, z: 1.8}}"
   ```

5. **Terminal 5 (See the big picture):**
   ```bash
   rqt_graph
   ```
   *(Shows a diagram of the nodes and the `/turtle1/cmd_vel` topic connecting them).*

> 📚 **Want to go further?** Work through the official [Beginner: CLI tools](https://docs.ros.org/en/jazzy/Tutorials/Beginner-CLI-Tools.html) and [Beginner: Client libraries](https://docs.ros.org/en/jazzy/Tutorials/Beginner-Client-Libraries.html) tutorials — they're the best next step after this guide.

[Next → Common errors & how to fix them](06-common-errors.md)
