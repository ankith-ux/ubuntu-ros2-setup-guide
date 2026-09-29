# 2. Recommended Ubuntu & ROS 2 Versions

Unlike standard software, you cannot just install "the latest version of ROS" on "the latest version of Ubuntu". 
**ROS distributions are strictly tied to specific Ubuntu distributions.** 

If you install the wrong version of Ubuntu, you will not be able to install the ROS 2 version you want.

---

## 🔄 Understanding LTS (Long Term Support)
Ubuntu releases standard versions every 6 months and **LTS (Long Term Support)** versions every 2 years (in April of even-numbered years). 
ROS 2 is heavily based around these LTS releases. **You should always use an LTS version of Ubuntu for robotics.**

---

## 🎯 Which one should you pick?

For current TRC juniors starting out, we strongly recommend **Ubuntu 22.04 with ROS 2 Humble**.

### 1. The Standard (Highly Recommended)
* **OS:** Ubuntu 22.04 LTS (Jammy Jellyfish)
* **ROS 2 Version:** ROS 2 Humble Hawksbill
* **Why?** This is currently the most widely used and stable version in the robotics community. Most tutorials, third-party packages, and TRC codebases are currently built for Humble.

### 2. The Bleeding Edge
* **OS:** Ubuntu 24.04 LTS (Noble Numbat)
* **ROS 2 Version:** ROS 2 Jazzy Jalisco
* **Why?** This is the newest release. While it will be the standard of the future, as a beginner, you might find fewer tutorials and some community packages might not be fully ported yet.

### 3. The Outdated
* **OS:** Ubuntu 20.04 LTS (Focal Fossa)
* **ROS 2 Versions:** Foxy Fitzroy / Galactic Geochelone
* **Why?** Foxy and Galactic have reached End of Life (EOL). **Do not use these** for new projects unless you are explicitly working on an old legacy robot that hasn't been updated.

---

## 📝 Summary Checklist before you download the ISO:
- [ ] I am downloading **Ubuntu 22.04 LTS**.
- [ ] I will be following tutorials for **ROS 2 Humble**.

[Head to the next chapter to install ROS 2!](03-ros2-installation.md)
