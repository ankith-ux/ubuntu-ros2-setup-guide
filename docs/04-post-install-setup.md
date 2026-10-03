# 4. Essential Post-Install Setup

Installing ROS 2 is just the beginning. This chapter sets up the tools and workspace structure you need to actually write and run your own robot code.

---

## 1. Automatically Source ROS 2

Right now, you would have to type `source /opt/ros/jazzy/setup.bash` every time you open a new terminal for `ros2` commands to work. Let's automate that by adding it to your `.bashrc` (a script that runs automatically each time a terminal opens).

```bash
echo "source /opt/ros/jazzy/setup.bash" >> ~/.bashrc
source ~/.bashrc
```

> ⚠️ Run each `echo ... >> ~/.bashrc` command in this guide **only once**. Running it again adds a duplicate line. You can check what's in the file anytime with `cat ~/.bashrc` (scroll to the bottom).

To confirm it worked, **open a brand-new terminal** and run:
```bash
printenv ROS_DISTRO
```
You should see `jazzy` printed. You can also run `ros2 -h` — it should print the list of `ros2` commands without any errors.

---

## 2. Enable Tab Completion for `colcon`

`ros2` commands already support `Tab` completion once ROS is sourced. Let's enable it for `colcon` too:

```bash
echo "source /usr/share/colcon_argcomplete/hook/colcon-argcomplete.bash" >> ~/.bashrc
source ~/.bashrc
```

---

## 3. Initialize rosdep
`rosdep` is a tool that automatically installs system-level dependencies for any ROS package you download. It was already installed in the previous chapter as part of `ros-dev-tools`. We just need to initialize it (once per machine):

```bash
sudo rosdep init
rosdep update
```

> **Note:** If `sudo rosdep init` shows `ERROR: default sources list file already exists`, that's fine — it just means it was already initialized. Skip to `rosdep update`.
>
> ⚠️ **Never** run `rosdep update` with `sudo`. Only `rosdep init` needs `sudo`.

---

## 4. Create Your First ROS 2 Workspace

A **workspace** is a dedicated folder where you write, build, and organize all your robot code. The standard name used by the community is `ros2_ws`.

```bash
mkdir -p ~/ros2_ws/src
cd ~/ros2_ws
colcon build --symlink-install
```

> **Note:** You'll see `Summary: 0 packages finished` — that's expected! The `src/` folder is empty right now. The build still creates the necessary directory structure.

Running `colcon build` generates three directories next to `src/`:
- `build/` — Intermediate files created during compilation.
- `install/` — The final output: executables, libraries, and environment setup files.
- `log/` — Build logs for debugging errors.

> 🚨 **Always run `colcon build` from the workspace root (`~/ros2_ws`)**, never from inside `src/` or a package folder. Otherwise you'll end up with extra `build/install/log` folders in the wrong place.

---

## 5. Source Your Workspace

Just like you source the base ROS 2 installation, you also need to source your workspace so your terminal knows about the packages you've built. Add a **safe conditional** to your `.bashrc` so it only runs when the workspace exists:

```bash
echo 'if [ -f ~/ros2_ws/install/setup.bash ]; then source ~/ros2_ws/install/setup.bash; fi' >> ~/.bashrc
source ~/.bashrc
```

*(The `if` check prevents a "No such file or directory" error when opening a fresh terminal before you've built anything).*

> 📝 **Order matters:** the base ROS line (`/opt/ros/jazzy/setup.bash`) must come **before** the workspace line in `~/.bashrc`. If you followed this chapter in order, it already is.

---

## 6. Build and Run Your First Package

Let's prove the whole setup works by creating, building, and running a tiny Python package:

```bash
cd ~/ros2_ws/src
ros2 pkg create --build-type ament_python --license Apache-2.0 --node-name my_node my_package
cd ~/ros2_ws
colcon build --symlink-install
source install/setup.bash
ros2 run my_package my_node
```

You should see:
```
Hi from my_package.
```

> 💡 **Remember:** After building a **new** package, you must run `source install/setup.bash` (or open a new terminal) before `ros2 run` can find it.

---

## 7. Verify Everything Is Set Up

Check that your environment is correctly layered (workspace on top of base ROS 2):

```bash
echo $AMENT_PREFIX_PATH
```

- **With an empty workspace**, you'll see only: `/opt/ros/jazzy` — this is normal since no packages have been built yet.
- **Once you build a package** (like `my_package` above), your workspace path will appear first: `/home/<your_username>/ros2_ws/install/my_package:/opt/ros/jazzy`

---

## 8. (Optional) Install Gazebo Simulation

Most robotics projects simulate robots before running them on real hardware. ROS 2 Jazzy pairs with **Gazebo Harmonic**, and you install both the simulator and the ROS bridge with one command:

```bash
sudo apt install ros-jazzy-ros-gz
```

Test it:
```bash
gz sim shapes.sdf
```
A window with a few 3D shapes should open. Press the ▶️ play button at the bottom-left to start the simulation.

> 📝 Gazebo **Classic** (`gazebo11`, `gazebo_ros`) is discontinued and does **not** work with Jazzy. If a tutorial uses `ros-jazzy-gazebo-ros-pkgs` or the `gazebo` command, it's outdated — look for a `ros_gz` / `gz sim` version instead.

You are now fully set up and ready to start building robots! ✅

[Next → Learn the basic commands you'll use every day](05-basic-commands.md)
