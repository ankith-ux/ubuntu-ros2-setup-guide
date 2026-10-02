# 4. Essential Post-Install Setup

Just installing ROS 2 isn't enough. We need to set up some essential tools to actually write, build, and manage your own robotic code.

---

### 1. Automatically Source ROS 2
Currently, you have to run `source /opt/ros/jazzy/setup.bash` every time you open a new terminal. Let's automate this by adding it to your `.bashrc` file (a script that runs every time a terminal opens).

```bash
echo "source /opt/ros/jazzy/setup.bash" >> ~/.bashrc
source ~/.bashrc
```

### 2. Install Build Tools & Dependencies (Colcon & rosdep)
`colcon` is the standard build tool used to compile ROS 2 packages, and `rosdep` automates installing package dependencies.
*(Note: If you installed `ros-dev-tools` in the previous chapter, these are already installed, but running this ensures nothing was missed).*

```bash
sudo apt install python3-colcon-common-extensions python3-rosdep -y
```

Initialize `rosdep` (this only needs to be done once per machine):
```bash
sudo rosdep init
rosdep update
```

### 3. Create Your First ROS 2 Workspace
A workspace is a dedicated folder where you build and develop your robotics code. The community convention is to use `ros2_ws` in your home directory, with a `src` directory inside:

```bash
mkdir -p ~/ros2_ws/src
cd ~/ros2_ws
colcon build --symlink-install
```
Running `colcon build` generates three new directories:
* `build/`: Where intermediate build files are stored.
* `install/`: Where built executables and environment setups are installed.
* `log/`: Contains logs from compilation.

### 4. Source Your Workspace Safely
Whenever you build packages in your workspace, your terminal needs to know about them. We can add this to your `.bashrc`, but we should use a conditional check so you don't get *"No such file or directory"* errors before you build:

```bash
echo 'if [ -f ~/ros2_ws/install/setup.bash ]; then source ~/ros2_ws/install/setup.bash; fi' >> ~/.bashrc
source ~/.bashrc
```

### 5. Verify Workspace Overlay
In ROS 2, packages are tracked via the **`AMENT_PREFIX_PATH`** environment variable (unlike `ROS_PACKAGE_PATH` in ROS 1). Verify that your local workspace is properly overlaying the underlying ROS installation:

```bash
echo $AMENT_PREFIX_PATH
```
You should see your local workspace path listed first, followed by the base ROS installation:
```text
/home/<your_username>/ros2_ws/install/<package_name>:/opt/ros/jazzy
```

You are now fully configured and ready to write and run ROS 2 packages!
