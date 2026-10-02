# 4. Essential Post-Install Setup

Installing ROS 2 is just the beginning. This chapter sets up the tools and workspace structure you need to actually write and run your own robot code.

---

## 1. Automatically Source ROS 2

Right now, you would have to type `source /opt/ros/jazzy/setup.bash` every time you open a new terminal for `ros2` commands to work. Let's automate that by adding it to your `.bashrc` (a script that runs automatically each time a terminal opens).

```bash
echo "source /opt/ros/jazzy/setup.bash" >> ~/.bashrc
source ~/.bashrc
```

To confirm it worked, try:
```bash
ros2 --version
```
You should see the ROS 2 version printed without any errors.

---

## 2. Initialize rosdep
`rosdep` is a tool that automatically installs system-level dependencies for any ROS package you download. It was already installed in the previous step as part of `ros-dev-tools`. We just need to initialize it (once per machine):

```bash
sudo rosdep init
rosdep update
```

> **Note:** If `sudo rosdep init` shows `ERROR: default sources list file already exists`, that's fine — it just means it was already initialized. Skip to `rosdep update`.

---

## 3. Create Your First ROS 2 Workspace

A **workspace** is a dedicated folder where you write, build, and organize all your robot code. The standard name used by the community is `ros2_ws`.

```bash
mkdir -p ~/ros2_ws/src
cd ~/ros2_ws
colcon build --symlink-install
```

> **Note:** You'll see a warning like `No packages were found` — that's expected! The `src/` folder is empty right now. The build still creates the necessary directory structure.

Running `colcon build` generates three directories:
- `build/` — Intermediate files created during compilation.
- `install/` — The final output: executables, libraries, and environment setup files.
- `log/` — Build logs for debugging errors.

---

## 4. Source Your Workspace

Just like you source the base ROS 2 installation, you also need to source your workspace so your terminal knows about the packages you've built. Add a **safe conditional** to your `.bashrc` so it only runs when the workspace exists:

```bash
echo 'if [ -f ~/ros2_ws/install/setup.bash ]; then source ~/ros2_ws/install/setup.bash; fi' >> ~/.bashrc
source ~/.bashrc
```

*(The `if` check prevents a "No such file or directory" error when opening a fresh terminal before you've built anything).*

---

## 5. Verify Everything Is Set Up

Check that your environment is correctly layered (workspace on top of base ROS 2):

```bash
echo $AMENT_PREFIX_PATH
```

- **After a fresh empty build**, you'll see only: `/opt/ros/jazzy` — this is normal since no packages have been built yet.
- **Once you build your first package**, your workspace path will appear first: `/home/<your_username>/ros2_ws/install/<pkg>:/opt/ros/jazzy`

You are now fully set up and ready to create your first ROS 2 package! ✅

