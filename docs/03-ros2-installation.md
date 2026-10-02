# 3. ROS 2 Installation Steps

This guide assumes you are installing **ROS 2 Jazzy** on **Ubuntu 24.04**. 
Open a terminal (`Ctrl` + `Alt` + `T`) and carefully paste these commands one by one.

*(Reference: [Official ROS 2 Jazzy Docs](https://docs.ros.org/en/jazzy/Installation/Ubuntu-Install-Debs.html))*

---

### Step 1: Set Locale
Ensure your system locale supports UTF-8.
```bash
locale  # check for UTF-8

sudo apt update && sudo apt install locales
sudo locale-gen en_US en_US.UTF-8
sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
export LANG=en_US.UTF-8

locale  # verify settings
```

### Step 2: Enable Universe Repository
```bash
sudo apt install software-properties-common
sudo add-apt-repository universe
```

### Step 3: Configure ROS 2 Repository
The method for adding ROS repositories has changed in Jazzy. We now use the `ros2-apt-source` package.
```bash
sudo apt update && sudo apt install curl -y
export ROS_APT_SOURCE_VERSION=$(curl -s https://api.github.com/repos/ros-infrastructure/ros-apt-source/releases/latest | grep -F "tag_name" | awk -F'"' '{print $4}')
curl -L -o /tmp/ros2-apt-source.deb "https://github.com/ros-infrastructure/ros-apt-source/releases/download/${ROS_APT_SOURCE_VERSION}/ros2-apt-source_${ROS_APT_SOURCE_VERSION}.$(. /etc/os-release && echo ${UBUNTU_CODENAME:-${VERSION_CODENAME}})_all.deb"
sudo dpkg -i /tmp/ros2-apt-source.deb
```

### Step 4: Install Development Tools (Recommended)
You will need development tools like `colcon` and `rosdep` for building packages and configuring ROS 2.
```bash
sudo apt update && sudo apt install ros-dev-tools
```
> **⚠️ Note for Ubuntu 24.04:** If you get dependency conflicts installing `ros-dev-tools`, verify that your Ubuntu apt suite includes updates and backports:
> ```bash
> grep Suites /etc/apt/sources.list.d/ubuntu.sources
> ```
> If `noble-updates` or `noble-backports` is missing, edit `/etc/apt/sources.list.d/ubuntu.sources` to make sure it reads:
> `Suites: noble noble-updates noble-backports`
> Then run `sudo apt update && sudo apt install ros-dev-tools`.

### Step 5: Install ROS 2 Packages
Update your repository index and ensure packages are up-to-date:
```bash
sudo apt update
sudo apt upgrade -y
```

Install the **Desktop Install** (Recommended for your laptop/workstation). This includes ROS 2, RViz, interactive demos, and tutorials:
```bash
sudo apt install ros-jazzy-desktop
```
*(For headless robots or SBCs like a Raspberry Pi without a monitor, you can install the barebones version instead: `sudo apt install ros-jazzy-ros-base`)*.

### Step 6: Verify Installation
Let's make sure it works! 
Open a terminal and run the setup script:
```bash
source /opt/ros/jazzy/setup.bash
```
Then, run a test node:
```bash
ros2 run demo_nodes_cpp talker
```
You should see it start publishing messages: `[INFO] [talker]: Publishing: 'Hello World: 1'`

Open a **second terminal**, source the setup script again, and run the listener:
```bash
source /opt/ros/jazzy/setup.bash
ros2 run demo_nodes_py listener
```
You should see it receiving the messages: `[INFO] [listener]: I heard: [Hello World: 1]`

🎉 If the talker and listener are communicating, your ROS 2 installation is successful!

[Next, let's configure your system so you don't have to type `source /opt/ros/jazzy/setup.bash` every single time.](04-post-install-setup.md)
