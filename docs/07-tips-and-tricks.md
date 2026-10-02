# 7. Useful Tips & Tricks

Working efficiently in Linux is all about knowing the right tools and shortcuts. Here are some pro-tips to speed up your ROS 2 workflow.

---

## 1. Use a Terminal Multiplexer (Terminator or Tmux)
In ROS 2, you will often need 4 or 5 terminals open at the same time (one for a node, one for a camera, one for RViz, etc.). The default Ubuntu terminal gets messy fast.

**Terminator** allows you to split a single window into multiple smaller terminals.
```bash
sudo apt install terminator
```
*Shortcuts:*
* `Ctrl` + `Shift` + `E` = Split vertically
* `Ctrl` + `Shift` + `O` = Split horizontally
* `Alt` + `Arrow Keys` = Move between splits

---

## 2. Bash Aliases (Shortcuts)
Typing `source ~/ros2_ws/install/setup.bash` or `cd ~/ros2_ws` repeatedly is annoying. You can create shortcuts (aliases) in your `~/.bashrc`.

Open the file:
```bash
nano ~/.bashrc
```
Scroll to the very bottom and add these lines:
```bash
# TRC ROS 2 Shortcuts
alias cw='cd ~/ros2_ws'
alias cs='cd ~/ros2_ws/src'
alias cb='cd ~/ros2_ws && colcon build --symlink-install'
alias cbs='cd ~/ros2_ws && colcon build --symlink-install --packages-select'
alias sb='source ~/.bashrc'
alias sros='source /opt/ros/jazzy/setup.bash'
alias sws='source ~/ros2_ws/install/setup.bash'
```
Save (`Ctrl+O`, `Enter`) and Exit (`Ctrl+X`). Then reload it:
```bash
source ~/.bashrc
```
Now, simply typing `cb` in any directory will automatically navigate to your workspace and build it with symlinks!

---

## 3. Visualizing Nodes with `rqt_graph`
Instead of guessing which node is publishing where, visualize your active computational graph:
```bash
sudo apt install ros-jazzy-rqt-graph -y
rqt_graph
```
This opens an interactive diagram showing all running nodes (ovals) and topics connecting them (rectangles). It is the fastest way to verify your system architecture.

---

## 4. Setting up VS Code for ROS 2
VS Code is the club's standard development editor.

1. **Install VS Code**:
   Download the `.deb` package from the [official VS Code website](https://code.visualstudio.com/) and install:
   ```bash
   sudo dpkg -i code_*.deb
   ```
2. **Recommended Extensions**:
   * **C/C++** (by Microsoft)
   * **Python** (by Microsoft)
   * **ROS** (by Microsoft or Robot Web Tools)
   * **CMake Tools**
3. **Avoid Red Squiggles (IntelliSense setup)**:
   * **Crucial Rule:** Always launch VS Code from a terminal where ROS 2 is sourced (`cd ~/ros2_ws && code .`).
   * For C++, ensure your `.vscode/c_cpp_properties.json` includes `"/opt/ros/jazzy/include/**"` and `"${workspaceFolder}/install/**/include/**"` in the `includePath`.

---

## 5. Quick Navigation with `colcon_cd`
Want to instantly jump into any package directory without typing long paths? Set up `colcon_cd`:
```bash
echo "source /usr/share/colcon_cd/function/colcon_cd.sh" >> ~/.bashrc
echo "export _colcon_cd_root=~/ros2_ws" >> ~/.bashrc
source ~/.bashrc
```
Now you can type `colcon_cd <package_name>` from anywhere to jump right to that package folder!

---

## 6. Install `htop`
When simulations (Gazebo/RViz) consume high memory or CPU, use `htop` to identify and terminate processes:
```bash
sudo apt install htop -y
htop
```

---

## 7. Tab Completion is Your Best Friend
Never manually type full node, topic, or package names. Start typing `ros2 topic echo /tur` and press **`Tab`**. ROS 2 will autocomplete the name. Pressing `Tab` twice will show all available matching options.
