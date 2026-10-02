# 6. Common Errors & Fixes

When starting out with Linux and ROS 2, you *will* encounter errors. Don't panic! Here are the most common ones and how to fix them.

---

## 1. `ros2: command not found`
**Why it happens:** Your terminal doesn't know where ROS 2 is installed.
**Fix:** You need to source the setup file.
```bash
source /opt/ros/jazzy/setup.bash
```
*Note: If you followed Chapter 4, this should happen automatically. Make sure you actually added it to your `.bashrc`!*

---

## 2. Package build fails with `colcon build`
**Why it happens:** You are usually missing a system dependency (another ROS or Linux package that your package needs to compile).
**Fix:** Update rosdep and install all missing dependencies from the workspace root:
```bash
cd ~/ros2_ws
rosdep update
rosdep install -i --from-path src --rosdistro jazzy -y
```
Then run `colcon build --symlink-install` again.

---

## 3. Windows time is completely wrong after dual-booting
**Why it happens:** Ubuntu stores the motherboard's hardware clock (RTC) in UTC by default, while Windows stores it in your local timezone. When switching between OSes, Windows reads UTC as local time, shifting your clock by several hours.

**Fix (Recommended: Fix it in Windows):**
Force Windows to store and read the hardware clock in UTC:
1. **Open Command Prompt as Administrator:** Press the `Windows` key, type `cmd`, right-click **Command Prompt**, and select **Run as administrator**.
2. **Run the Registry Command:** Paste the following command and press `Enter`:
   ```cmd
   reg add "HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\TimeZoneInformation" /v RealTimeIsUniversal /d 1 /t REG_DWORD /f
   ```
   You should see: `"The operation completed successfully."`
3. **Restart and Sync:** Restart your PC back into Windows. Right-click the clock in your taskbar, select **Adjust date/time**, and toggle **Set time automatically** off and back on to force a fresh sync.

*(Note: Once this is done, both Windows and Ubuntu will interpret the hardware clock as UTC, preventing any clock shifts).*

---

## 4. `ModuleNotFoundError: No module named 'rclpy'`
**Why it happens:** Your Python interpreter cannot locate the ROS 2 Python bindings.
**Fix:** 
1. Source ROS 2 first: `source /opt/ros/jazzy/setup.bash`
2. **Virtual Environments Note:** If you are inside a Python virtual environment (`venv` or `conda`), it will not see system ROS 2 packages by default. Either run outside a venv, or create your venv with `--system-site-packages`:
   ```bash
   python3 -m venv --system-site-packages my_env
   ```

---

## 5. `error: externally-managed-environment` (pip install error on Ubuntu 24.04)
**Why it happens:** Ubuntu 24.04 enforces PEP 668 to prevent `pip` from breaking system-level apt packages.
**Fix:**
* **Preferred:** Install packages using apt: `sudo apt install python3-<package_name>`
* **If unavailable on apt:** Use `pipx` (`sudo apt install pipx && pipx install <tool>`) or create a virtual environment with `--system-site-packages`.
* **Avoid** using `pip install --break-system-packages` unless strictly necessary for disposable environments.

---

## 6. Permissions denied or USB disappearing when accessing Serial Ports (Arduino/LiDAR/IMU)
**Why it happens:**
1. Standard users lack read/write permissions to serial devices like `/dev/ttyUSB0` or `/dev/ttyACM0`.
2. Ubuntu comes with a braille display daemon (`brltty`) that mistakenly claims USB-serial converters (like CH340 or FTDI) and disconnects them seconds after plugging in.

**Fix:**
1. Add your user to the `dialout` group:
   ```bash
   sudo usermod -a -G dialout $USER
   ```
2. Remove the conflicting `brltty` package:
   ```bash
   sudo apt remove brltty -y
   ```
3. **CRITICAL:** Log out and log back in (or reboot) for group permissions to take effect.

---

## 7. Interference on Lab Wi-Fi (Cross-talk with other club members)
**Why it happens:** ROS 2 uses DDS multicast to automatically discover other nodes on the same local network. If multiple students in the lab are running ROS 2 on the same Wi-Fi, their nodes and topics will merge!
**Fix:** Assign yourself a unique **`ROS_DOMAIN_ID`** (a number between 0 and 101):
```bash
echo "export ROS_DOMAIN_ID=42" >> ~/.bashrc
source ~/.bashrc
```
*(Pick an ID agreed upon with your team so you only talk to your own robot and team members).*
