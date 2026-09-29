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
# TRC ROS 2 Aliases
alias cw='cd ~/ros2_ws'
alias cs='cd ~/ros2_ws/src'
alias cb='cd ~/ros2_ws && colcon build'
alias cbs='cd ~/ros2_ws && colcon build --packages-select'
alias sb='source ~/.bashrc'
alias sros='source /opt/ros/humble/setup.bash'
alias sws='source ~/ros2_ws/install/setup.bash'
```
Save (`Ctrl+O`, `Enter`) and Exit (`Ctrl+X`). Then reload it:
```bash
source ~/.bashrc
```
Now, typing `cb` will automatically navigate to your workspace and build it!

---

## 3. Setting up VS Code for ROS 2
VS Code is the recommended IDE for TRC.

1. Download the `.deb` file from the [VS Code website](https://code.visualstudio.com/) and install it:
   `sudo dpkg -i code_*.deb`
2. **Install Extensions:**
   * C/C++ (by Microsoft)
   * Python (by Microsoft)
   * CMake Tools
   * **ROS** (by Microsoft)
3. **Launch from Terminal:** Always launch VS Code from a terminal where you have sourced ROS 2! 
   ```bash
   cd ~/ros2_ws
   code .
   ```
   If you don't do this, VS Code won't be able to find the `rclpy` or `rclcpp` libraries and will give you red squiggly error lines.

---

## 4. Install `htop`
When your computer freezes because Gazebo is using all your RAM, use `htop` to see what's going on and kill the process.
```bash
sudo apt install htop
htop
```

## 5. Tab Completion is Your Best Friend
Never type a full package name or topic name. Start typing `ros2 topic echo /cam` and hit the **`Tab`** key. ROS 2 will automatically complete the topic name for you. If it doesn't, press `Tab` twice to see all available options.
