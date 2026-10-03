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
* `Ctrl` + `Shift` + `W` = Close the current split

**Tmux** does the same thing *inside* any terminal, and keeps running even if you disconnect — very useful when you SSH into a robot's onboard computer (e.g. a Raspberry Pi):
```bash
sudo apt install tmux
```
*Shortcuts (press `Ctrl` + `B`, release, then the key):* `%` = split vertically, `"` = split horizontally, `Arrow Keys` = move between splits, `d` = detach (reattach later with `tmux attach`).

---

## 2. Bash Aliases (Shortcuts)
Typing `source ~/ros2_ws/install/setup.bash` or `cd ~/ros2_ws` repeatedly is annoying. You can create shortcuts (aliases) in your `~/.bashrc`.

Open the file:
```bash
nano ~/.bashrc
```
Scroll to the very bottom and add these lines:
```bash
# ROS 2 Shortcuts
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
Instead of guessing which node is publishing where, visualize your active computational graph (it's already included with `ros-jazzy-desktop`):
```bash
rqt_graph
```
This opens an interactive diagram showing all running nodes (ovals) and topics connecting them (rectangles). It is the fastest way to verify your system architecture. *(Click the 🔄 refresh button at the top-left if you start new nodes after opening it.)*

---

## 4. Setting up VS Code for ROS 2
VS Code is a popular, beginner-friendly development editor for ROS 2.

1. **Install VS Code**:
   Download the `.deb` package from the [official VS Code website](https://code.visualstudio.com/) and install it from your `Downloads` folder (using `apt` instead of `dpkg` automatically installs any missing dependencies):
   ```bash
   cd ~/Downloads
   sudo apt install ./code_*.deb
   ```
   *(If it asks whether to add the Microsoft apt repository, say **Yes** — that keeps VS Code updated with `sudo apt upgrade`.)*
2. **Recommended Extensions** (search for them in the Extensions tab, `Ctrl` + `Shift` + `X`):
   * **C/C++** (by Microsoft)
   * **Python** (by Microsoft)
   * **Robot Developer Extensions for ROS 2** (by Ranch Hand Robotics) — the maintained successor of Microsoft's old **ROS** extension (`ms-iot.vscode-ros`), which is deprecated. Uninstall the old one if you have it.
   * **CMake Tools** (by Microsoft)
   * **XML** (by Red Hat) — helpful for `package.xml`, URDF and launch files
3. **Avoid Red Squiggles (IntelliSense setup)**:
   * **Crucial Rule:** Always launch VS Code from a terminal where ROS 2 is sourced (`cd ~/ros2_ws && code .`).
   * For C++, ensure your `.vscode/c_cpp_properties.json` includes `"/opt/ros/jazzy/include/**"` and `"${workspaceFolder}/install/**/include/**"` in the `includePath`.
   * For Python, if imports like `rclpy` are underlined, add `"/opt/ros/jazzy/lib/python3.12/site-packages"` to `python.analysis.extraPaths` in your VS Code settings.

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

---

## 8. Keep Your System Updated
ROS 2 Jazzy gets regular bug-fix releases through `apt`. Once every week or two, run:
```bash
sudo apt update && sudo apt upgrade -y
```

---

🎓 **That's the end of the guide!** You now have a working Ubuntu 24.04 + ROS 2 Jazzy setup. Head back to the [main README](../README.md), or continue with the official [ROS 2 Jazzy tutorials](https://docs.ros.org/en/jazzy/Tutorials.html).
