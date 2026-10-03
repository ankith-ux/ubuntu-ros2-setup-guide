# Ubuntu + ROS 2 Setup Guide

Welcome to the **Ubuntu & ROS 2 Setup Guide**! 🚀

This repository is created as a comprehensive, beginner-friendly reference for anyone starting their journey in robotics. Whether you have zero experience with Linux or are just looking for a refresher on ROS 2, this guide will walk you through everything step-by-step.

**Recommended setup:** 🐧 **Ubuntu 24.04 LTS** + 🤖 **ROS 2 Jazzy Jalisco**

## ✅ Before You Start

* **A 64-bit Windows laptop** (Intel/AMD) with at least **8 GB RAM** (16 GB recommended for Gazebo) and **50 GB+ of free disk space** (100 GB recommended).
* **An 8 GB+ USB flash drive** you don't mind erasing.
* **A stable internet connection** — you'll download roughly 10 GB in total. Mobile hotspot works if campus Wi-Fi is slow.
* **2–3 hours** of time and your **charger plugged in**.
* **Your files backed up.** Dual-booting is safe when you follow the steps, but mistakes happen.

> ⚠️ **Download Ubuntu 24.04, not 26.04!** Ubuntu's main download page now offers 26.04 by default, which does **not** work with ROS 2 Jazzy. Chapter 1 links to the correct download.

> 🍎 **On a Mac?** This guide is written for Windows laptops. Ask someone experienced before starting — Apple Silicon Macs can't dual-boot Ubuntu in the usual way.

## 📖 Table of Contents

We have broken down the guide into easy-to-digest chapters. Please follow them in order if you are setting up your system from scratch.

1. **[Dual-Booting Ubuntu with Windows](docs/01-dual-booting.md)**
   Learn how to install Ubuntu alongside your Windows operating system without losing your data.
2. **[Recommended Ubuntu & ROS 2 Versions](docs/02-ubuntu-versions.md)**
   Understand which version of Ubuntu you need for different ROS 2 distributions.
3. **[ROS 2 Installation Steps](docs/03-ros2-installation.md)**
   Step-by-step instructions to install ROS 2 on your newly setup Ubuntu system.
4. **[Essential Post-Install Setup](docs/04-post-install-setup.md)**
   Crucial configurations, installing build tools (`colcon`), and setting up your first workspace.
5. **[Basic Commands & Packages](docs/05-basic-commands.md)**
   The fundamental ROS 2 commands you will use every day.
6. **[Common Errors & Fixes](docs/06-common-errors.md)**
   Ran into an issue? Check here for solutions to the most common beginner problems.
7. **[Useful Tips & Tricks](docs/07-tips-and-tricks.md)**
   Productivity boosts, terminal setups, and aliases to make your workflow faster.

## 🧪 Is My Setup Working?

After finishing Chapter 4, open a **new** terminal and check all of these:

| Check | Command | Expected result |
| --- | --- | --- |
| Ubuntu version | `lsb_release -rs` | `24.04` |
| ROS 2 is sourced | `printenv ROS_DISTRO` | `jazzy` |
| Build tool installed | `colcon -h` | Help text, no errors |
| ROS 2 runs | `ros2 run demo_nodes_cpp talker` | `Publishing: 'Hello World: 1'`, ... |
| GUI tools work | `ros2 run turtlesim turtlesim_node` | A window with a turtle |

If all five pass, you're ready to go! 🎉

## 🆘 Getting Help

1. Check **[Common Errors & Fixes](docs/06-common-errors.md)** first — most problems are already covered there.
2. Search the exact error message online. [Robotics Stack Exchange](https://robotics.stackexchange.com/questions/tagged/ros2) and the [official ROS 2 Jazzy docs](https://docs.ros.org/en/jazzy/) are the best sources.
3. Ask someone experienced (a mentor, senior, or community forum). Share the **exact command** you ran and the **full error text** (copy-paste it, don't send a blurry photo of your screen).

## 🎯 Goal
The main goal of this repository is to maintain a standard reference that beginners can rely on. If you learn something new or find a fix to a frustrating bug, feel free to contribute by opening a Pull Request!

Happy building! 🤖
