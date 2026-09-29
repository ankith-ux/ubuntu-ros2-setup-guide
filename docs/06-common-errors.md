# 6. Common Errors & Fixes

When starting out with Linux and ROS 2, you *will* encounter errors. Don't panic! Here are the most common ones and how to fix them.

---

## 1. `ros2: command not found`
**Why it happens:** Your terminal doesn't know where ROS 2 is installed.
**Fix:** You need to source the setup file.
```bash
source /opt/ros/humble/setup.bash
```
*Note: If you followed Chapter 4, this should happen automatically. Make sure you actually added it to your `.bashrc`!*

---

## 2. Package build fails with `colcon build`
**Why it happens:** You are usually missing a system dependency (another package that your package needs to compile).
**Fix:** Use `rosdep` to automatically install missing dependencies.
Go to the root of your workspace (`cd ~/ros2_ws`) and run:
```bash
rosdep install -i --from-path src --rosdistro humble -y
```
Then try building again.

---

## 3. Windows time is completely wrong after dual-booting
**Why it happens:** Windows expects the hardware clock to be in Local Time, while Ubuntu expects it to be in UTC. They fight over it, causing your Windows time to be offset.
**Fix:** Boot into Ubuntu, open a terminal, and tell Ubuntu to use Local Time just like Windows.
```bash
timedatectl set-local-rtc 1 --adjust-system-clock
```

---

## 4. `ModuleNotFoundError: No module named 'rclpy'`
**Why it happens:** Similar to error #1, but for Python. Your terminal doesn't know about the ROS 2 Python libraries.
**Fix:** 
1. Source ROS 2: `source /opt/ros/humble/setup.bash`
2. If you are trying to run a python script directly (e.g. `python3 script.py`), ensure you are not inside a virtual environment (`venv` or `conda`) unless you have specifically configured it to see system packages.

---

## 5. `Failed to connect to the bus: Could not parse server address` (or similar network/D-Bus errors in WSL/Docker)
**Why it happens:** If you aren't native dual-booting and are using WSL, GUI components or IPC mechanisms can fail.
**Fix:** Native dual-booting is recommended to avoid this. If using WSL, you need specialized setup for GUI forwarding (like VcXsrv) and network configurations.

---

## 6. Permissions denied when accessing Serial Ports (Arduino/Lidar)
**Why it happens:** By default, standard users do not have permission to read/write to hardware ports like `/dev/ttyUSB0` or `/dev/ttyACM0`.
**Fix:** Add your user to the `dialout` group.
```bash
sudo usermod -a -G dialout $USER
```
**CRITICAL:** You must log out and log back in (or restart your computer) for this to take effect!
