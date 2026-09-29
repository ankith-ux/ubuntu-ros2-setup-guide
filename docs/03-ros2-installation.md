# 3. ROS 2 Installation Steps

This guide assumes you are installing **ROS 2 Humble** on **Ubuntu 22.04**. 
Open a terminal (`Ctrl` + `Alt` + `T`) and carefully paste these commands one by one.

*(Reference: [Official ROS 2 Humble Docs](https://docs.ros.org/en/humble/Installation/Ubuntu-Install-Debians.html))*

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

### Step 3: Add the ROS 2 GPG Key
```bash
sudo apt update && sudo apt install curl -y
sudo curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg
```

### Step 4: Add the ROS 2 Repository
```bash
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(. /etc/os-release && echo $UBUNTU_CODENAME) main" | sudo tee /etc/apt/sources.list.d/ros2.list > /dev/null
```

### Step 5: Install ROS 2 Packages
Update your apt repository caches first:
```bash
sudo apt update
sudo apt upgrade -y
```

Install the **Desktop Install** (Recommended). This includes ROS, RViz, demos, and tutorials.
```bash
sudo apt install ros-humble-desktop
```
*(This step might take a while depending on your internet speed, as it downloads several gigabytes of packages).*

### Step 6: Verify Installation
Let's make sure it works! 
Open a terminal and run the setup script:
```bash
source /opt/ros/humble/setup.bash
```
Then, run a test node:
```bash
ros2 run demo_nodes_cpp talker
```
You should see it start publishing messages: `[INFO] [talker]: Publishing: 'Hello World: 1'`

Open a **second terminal**, source the setup script again, and run the listener:
```bash
source /opt/ros/humble/setup.bash
ros2 run demo_nodes_py listener
```
You should see it receiving the messages: `[INFO] [listener]: I heard: [Hello World: 1]`

🎉 If the talker and listener are communicating, your ROS 2 installation is successful!

[Next, let's configure your system so you don't have to type `source /opt/ros/humble/setup.bash` every single time.](04-post-install-setup.md)
