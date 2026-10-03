# 2. Recommended Ubuntu & ROS 2 Versions

Unlike standard software, you cannot just install "the latest version of ROS" on "the latest version of Ubuntu". 
**ROS distributions are strictly tied to specific Ubuntu distributions.** 

If you install the wrong version of Ubuntu, you will not be able to install the ROS 2 version you want.

---

## 🔄 Understanding LTS (Long Term Support)
Ubuntu releases standard versions every 6 months and **LTS (Long Term Support)** versions every 2 years (in April of even-numbered years). 
ROS 2 follows the same pattern: a new **LTS ROS 2 distribution** is released every two years in May, supported for 5 years, and built for the newest Ubuntu LTS. **You should always use an LTS version of Ubuntu for robotics.**

---

## 📊 Version Compatibility Table

| ROS 2 Distro | Ubuntu Version | Type | Supported Until | Status |
| --- | --- | --- | --- | --- |
| **Jazzy Jalisco** | **24.04 LTS (Noble Numbat)** | LTS | May 2029 | ✅ **Recommended** |
| Lyrical Luth | 26.04 LTS (Resolute Raccoon) | LTS | May 2031 | 🆕 Very new — not yet widely adopted |
| Kilted Kaiju | 24.04 LTS | Non-LTS (short-lived) | Late 2026 | ❌ Don't use |
| Humble Hawksbill | 22.04 LTS (Jammy Jellyfish) | LTS | May 2027 | ⚠️ Legacy robots only |
| Foxy / Galactic | 20.04 LTS (Focal Fossa) | — | Already End of Life | ❌ Don't use |

---

## 🎯 Which one should you pick?

For beginners starting out, we strongly recommend **Ubuntu 24.04 with ROS 2 Jazzy**.

### 1. The Standard (Highly Recommended)
* **OS:** Ubuntu 24.04 LTS (Noble Numbat)
* **ROS 2 Version:** ROS 2 Jazzy Jalisco
* **Why?** This is the most widely used modern LTS setup: tutorials, courses, and packages (Nav2, MoveIt 2, Gazebo Harmonic via `ros_gz`) are mature and well-tested on it, and it is supported until 2029. It's the most common setup in the community, so it's the easiest to get help with.

### 2. The Newest (Not yet recommended)
* **OS:** Ubuntu 26.04 LTS (Resolute Raccoon)
* **ROS 2 Version:** ROS 2 Lyrical Luth (released May 2026)
* **Why not yet?** It's brand new, so many third-party packages and tutorials haven't caught up. Unless your mentor or project specifically requires it, stick with Jazzy.

### 3. The Older Standard (Legacy)
* **OS:** Ubuntu 22.04 LTS (Jammy Jellyfish)
* **ROS 2 Version:** ROS 2 Humble Hawksbill
* **Why?** This was the previous standard. You might still encounter it on some older robots, but for new setups, you should stick to Jazzy.

### 4. The Outdated
* **OS:** Ubuntu 20.04 LTS (Focal Fossa)
* **ROS 2 Versions:** Foxy Fitzroy / Galactic Geochelone
* **Why?** Foxy and Galactic have reached End of Life (EOL). **Do not use these** for new projects unless you are explicitly working on an old legacy robot that hasn't been updated.

> 💡 **Following an online tutorial?** Check which ROS 2 distro it uses. Commands are mostly the same, but you must replace the distro name — e.g. `ros-humble-...` → `ros-jazzy-...` and `/opt/ros/humble` → `/opt/ros/jazzy`. Always prefer docs at [docs.ros.org/en/jazzy](https://docs.ros.org/en/jazzy/).

---

## 📝 Summary Checklist before you download the ISO:
- [ ] I am downloading **Ubuntu 24.04 LTS** (file name starts with `ubuntu-24.04`), **not** 26.04.
- [ ] I will be following tutorials for **ROS 2 Jazzy**.

Already installed Ubuntu? Confirm your version by running this in a terminal:
```bash
lsb_release -a
```
It should say `Ubuntu 24.04.x LTS` and `Codename: noble`.

[Head to the next chapter to install ROS 2!](03-ros2-installation.md)
