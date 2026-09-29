# 1. Dual-Booting Ubuntu with Windows

Setting up Ubuntu alongside Windows is the standard and recommended way to work with ROS 2. Virtual Machines (VMs) and WSL2 (Windows Subsystem for Linux) can work, but a native dual-boot ensures you get full hardware performance (especially important for graphics and sensors).

---

## 🛠️ Prerequisites
Before you begin, make sure you have:
1. **A USB Flash Drive** (at least 8GB, data will be erased).
2. **Rufus** or **BalenaEtcher** (software to make the USB bootable).
3. **Ubuntu ISO File**: Download the recommended version (see [Chapter 2](02-ubuntu-versions.md)) from the [official Ubuntu website](https://ubuntu.com/download/desktop).
4. **IMPORTANT**: Back up any critical data on your Windows system just in case!

---

## 🛑 Crucial Pre-installation Steps (Windows)

### 1. Save your BitLocker Recovery Key! (For Windows Pro users mostly)
If your Windows drive is encrypted with BitLocker, changing boot settings might trigger a recovery screen. 
* Go to Windows search -> Type **Manage BitLocker**.
* Click **Back up your recovery key** and save it to a separate USB drive or your Microsoft account.

### 2. Disable Fast Startup
Fast Startup can cause issues with mounting Windows partitions in Linux.
* Open **Control Panel** -> Power Options -> Choose what the power buttons do.
* Click *Change settings that are currently unavailable*.
* Uncheck **Turn on fast startup** and Save changes.

### 3. Create Unallocated Space (Shrinking Windows Partition)
You need to free up space on your hard drive for Ubuntu.
* Right-click the Start Button -> **Disk Management**.
* Right-click your main drive (usually `C:`) and select **Shrink Volume**.
* Enter the amount of space to shrink in MB. 
  * **Minimum recommended:** 50,000 MB (50GB).
  * **Ideal for Robotics/ROS 2:** 100,000 MB (100GB) or more if you plan to install heavy simulation software like Gazebo.
* Leave this space as "Unallocated". Do not create a new volume!

---

## 💾 Creating the Bootable USB
1. Plug in your USB drive.
2. Open **Rufus**.
3. Select your USB drive under "Device".
4. Click **SELECT** and choose the Ubuntu ISO file you downloaded.
5. Click **START**. (If prompted about ISOHybrid, choose "Write in ISO image mode"). Wait for it to finish.

---

## ⚙️ BIOS Settings
You need to tell your computer to boot from the USB instead of Windows.
1. Restart your PC. As it turns on, repeatedly press your BIOS key (usually `F2`, `F12`, `DEL`, or `ESC` depending on your laptop brand).
2. Find the **Secure Boot** option (usually under Security or Boot tab) and **Disable** it. (Some Linux drivers don't play well with Secure Boot).
3. Change the **Boot Order** to put your "USB Flash Drive" at the very top.
4. Save and Exit (`F10` usually).

---

## 🐧 Installing Ubuntu
1. Your PC should now boot into the Ubuntu setup menu. Select **Try or Install Ubuntu**.
2. Follow the on-screen instructions (select language, keyboard layout).
3. **Connect to Wi-Fi** when prompted (helpful for downloading updates during installation).
4. **Updates and Other Software:** Choose *Normal Installation* and check *Install third-party software for graphics and Wi-Fi hardware*.
5. **Installation Type (CRITICAL STEP):**
   * Choose **Install Ubuntu alongside Windows Boot Manager**. This is the easiest and safest option. It will automatically use the unallocated space you created earlier.
   * *(Advanced users can choose "Something else" to manually configure Root, Swap, and Home partitions, but "Install alongside" is perfectly fine for beginners).*
6. Create your user account and wait for the installation to finish!
7. Restart the computer, remove the USB drive when prompted, and press Enter.

🎉 **Congratulations!** You now have a dual-boot system. Every time you turn on your PC, a menu (called GRUB) will appear asking whether you want to boot into Ubuntu or Windows.
