# 3. ROS 2 Installation Steps

This guide will walk you through installing **ROS 2 Jazzy Jalisco** on **Ubuntu 24.04 LTS**.

Open a terminal (`Ctrl` + `Alt` + `T`) and run each command one at a time.

> 💡 **Terminal tips for first-timers:**
> * To paste into the terminal, use `Ctrl` + `Shift` + `V` (plain `Ctrl` + `V` doesn't work).
> * When typing your password after `sudo`, **nothing appears on screen** — that's normal. Just type it and press `Enter`.
> * If asked `Do you want to continue? [Y/n]`, type `Y` and press `Enter`.

*(Reference: [Official ROS 2 Jazzy Docs](https://docs.ros.org/en/jazzy/Installation/Ubuntu-Install-Debs.html))*

---

### Step 1: Set Locale
Make sure your system supports UTF-8. A fresh Ubuntu 24.04 install should already have this, but run it to be safe.
```bash
locale  # check for UTF-8

sudo apt update && sudo apt install locales
sudo locale-gen en_US en_US.UTF-8
sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
export LANG=en_US.UTF-8

locale  # verify settings
```

---

### Step 2: Enable Universe Repository
This enables the Ubuntu Universe package source, which some ROS dependencies need.
```bash
sudo apt install software-properties-common
sudo add-apt-repository universe
sudo apt update
```

---

### Step 3: Add the ROS 2 Repository
ROS 2 uses the `ros2-apt-source` package to add its package source and signing key (and keep them updated automatically). Run these commands in order, **in the same terminal**:
```bash
sudo apt update && sudo apt install curl -y
export ROS_APT_SOURCE_VERSION=$(curl -s https://api.github.com/repos/ros-infrastructure/ros-apt-source/releases/latest | grep -F "tag_name" | awk -F'"' '{print $4}')
echo $ROS_APT_SOURCE_VERSION
```
The `echo` should print a version number (like `1.3.0`).

> ⚠️ **Printed nothing?** GitHub limits how many requests can come from one network. On shared college or lab Wi-Fi this limit is often hit when many people install at once. Switch to **mobile hotspot** and run the `export` line again, or open [the releases page](https://github.com/ros-infrastructure/ros-apt-source/releases/latest) in your browser and set it manually, e.g. `export ROS_APT_SOURCE_VERSION=1.3.0` (use the number shown on that page).

Now download and install the package:
```bash
curl -L -o /tmp/ros2-apt-source.deb "https://github.com/ros-infrastructure/ros-apt-source/releases/download/${ROS_APT_SOURCE_VERSION}/ros2-apt-source_${ROS_APT_SOURCE_VERSION}.$(. /etc/os-release && echo ${UBUNTU_CODENAME:-${VERSION_CODENAME}})_all.deb"
sudo dpkg -i /tmp/ros2-apt-source.deb
```
Then reload your package index so `apt` can see the new ROS packages:
```bash
sudo apt update
```

---

### Step 4: Upgrade Your System
Before installing ROS 2, make sure your system is fully up-to-date:
```bash
sudo apt upgrade
```
*(Review what will be upgraded and press `Y` to confirm).*

---

### Step 5: Install ROS 2 Development Tools
These tools include `colcon` (build tool) and `rosdep` (dependency manager), which you will use constantly.
```bash
sudo apt install ros-dev-tools
```

> **⚠️ If you get a dependency conflict error:** Your Ubuntu apt sources may be missing updates/backports. Check by running:
> ```bash
> grep Suites /etc/apt/sources.list.d/ubuntu.sources
> ```
> If `noble-updates` or `noble-backports` is missing from the `Suites:` line, edit that file and add them:
> `Suites: noble noble-updates noble-backports`
> (You'll need `sudo nano /etc/apt/sources.list.d/ubuntu.sources` to edit it — save with `Ctrl+O`, `Enter`, exit with `Ctrl+X`.)
> Then run:
> ```bash
> sudo apt clean && sudo apt update && sudo apt full-upgrade -y
> sudo apt install ros-dev-tools
> ```

---

### Step 6: Install ROS 2 Jazzy
Install the **Desktop** version. This includes ROS 2, RViz2, rqt, demo nodes, and tutorials like turtlesim — everything you need to get started:
```bash
sudo apt install ros-jazzy-desktop
```
*(This downloads a few gigabytes — it can take 10–30 minutes depending on your internet).*

> 📝 There is also `ros-jazzy-ros-base` (no GUI tools). That's meant for robots' onboard computers like a Raspberry Pi — **on your laptop, always install `desktop`**.

---

### Step 7: Verify Your Installation
Let's confirm everything installed correctly.

**Terminal 1** — Source ROS 2 and run the talker node:
```bash
source /opt/ros/jazzy/setup.bash
ros2 run demo_nodes_cpp talker
```
You should see: `[INFO] [talker]: Publishing: 'Hello World: 1'`

**Terminal 2** — Open a new terminal, source again, and run the listener:
```bash
source /opt/ros/jazzy/setup.bash
ros2 run demo_nodes_py listener
```
You should see: `[INFO] [listener]: I heard: [Hello World: 1]`

🎉 If both terminals are talking to each other, **ROS 2 is installed and working!** This also proves C++ and Python nodes can communicate. Press `Ctrl` + `C` in each terminal to stop the nodes.

> ❓ **`ros2: command not found` or `Package 'demo_nodes_cpp' not found`?** You either forgot the `source` line in that terminal, or Step 6 didn't finish. Re-run `sudo apt install ros-jazzy-desktop` and check the output for errors.

[Next → Set up your workspace so you don't have to `source` manually every time.](04-post-install-setup.md)

