# 1. Dual-Booting Ubuntu with Windows

Setting up Ubuntu alongside Windows is the standard and recommended way to work with ROS 2. Virtual Machines (VMs) and WSL2 (Windows Subsystem for Linux) can work, but a native dual-boot ensures you get full hardware performance (especially important for graphics and sensors).

---

## 🛠️ Prerequisites
Before you begin, make sure you have:
1. **A USB Flash Drive** (at least 8GB, data will be erased).
2. **Rufus** (software to make the USB bootable).
3. **Ubuntu ISO File**: Download the Ubuntu 24.04 LTS version from the [Alternative Downloads page](https://ubuntu.com/download/alternative-downloads).
4. **IMPORTANT**: Back up any critical data on your Windows system just in case!

---

## 🛑 Crucial Pre-installation Steps (Windows)

### 1. Disable BitLocker (For Windows Pro users mostly)
If your Windows drive is encrypted with BitLocker, it can prevent Ubuntu from installing properly or cause annoying recovery loops. You must disable it before proceeding.
* Go to Windows search -> Type **Manage BitLocker**.
* Click **Turn off BitLocker** and wait for the decryption process to complete (this can take some time).
* *(Note: If your laptop is managed by an organization and you cannot turn it off, you must at least back up your recovery key to a USB/Microsoft account and select **Suspend Protection**).*

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
5. Under **Partition scheme**, make sure **GPT** is selected (and Target system is UEFI).
6. Click **START**. (If prompted about ISOHybrid, choose "Write in ISO image mode"). Wait for it to finish.

---

## ⚙️ BIOS Settings
You need to tell your computer to boot from the USB instead of Windows.
1. Restart your PC. As it turns on, repeatedly press your BIOS key (commonly `F2`, `F12`, `DEL`, or `ESC` depending on your motherboard/laptop brand).
2. **Disable Secure Boot**: Find the **Secure Boot** option (usually under the *Security* or *Boot* tab) and set it to **Disabled**. (Some third-party hardware drivers like NVIDIA won't load properly if Secure Boot is left on).
3. **Storage Controller (If drives aren't detected)**: If your laptop has Intel RST / RAID / VMD enabled, Ubuntu will not be able to see your NVMe/SSD drive. Ensure the SATA/Storage operation mode is set to **AHCI / NVMe**. *(Note: Changing this on an existing Windows install might require booting Windows in Safe Mode first).*
4. **Boot Order**: Change the boot sequence to put your **USB Flash Drive** at the top, or use the one-time boot menu (`F12` on most laptops).
5. Save changes and exit (`F10` on most systems).

---

## 🐧 Installing Ubuntu 24.04
1. Your PC will boot into the GRUB menu. Select **Try or Install Ubuntu**.
2. Select your language and keyboard layout.
3. **Connect to the Internet**: Connect to your Wi-Fi or Ethernet (recommended so Ubuntu can download updates and drivers during installation).
4. **Application Selection**:
   * Ubuntu 24.04 gives you two options: **Default selection** (minimal) and **Extended selection** (full installation).
   * **Choose Extended selection** to ensure essential utilities, developer libraries, and tools are available out of the box.
5. **Proprietary Software**:
   * Check the boxes for **Install third-party software for graphics and Wi-Fi hardware** and **Download and install support for additional media formats**.
6. **Installation Type (CRITICAL STEP)**:
   * Select **Install Ubuntu alongside Windows Boot Manager**. This is the safest automated option; Ubuntu will automatically detect the unallocated space you shrank earlier and create the root filesystem there.
   * *(If you choose "Manual partitioning", create an EFI partition if not automatically detected, and a root partition `/` formatted as `ext4` on the free space).*
7. **Identity & User Account**: Enter your name, computer name, username, and a memorable password (you will need this password constantly for `sudo` commands).
8. Once installation finishes, click **Restart Now**. When prompted with *"Please remove the installation medium, then press ENTER"*, unplug your USB flash drive and hit **Enter**.

🎉 **Congratulations!** Your dual-boot system is ready. Every time you turn on your PC, the GRUB bootloader will appear, letting you pick between Ubuntu 24.04 and Windows.

---

## ⏰ Post-Installation: Fix Windows Time Desync
After booting between Ubuntu and Windows, you will likely notice that your Windows clock is off by several hours. This happens because Ubuntu sets the motherboard hardware clock in UTC by default, while Windows expects it in local time. 

**Fix it in Windows by forcing Windows to use UTC:**
1. Boot into **Windows**.
2. Press the `Windows` key, type `cmd`, right-click **Command Prompt**, and choose **Run as administrator**.
3. Run the following registry command:
   ```cmd
   reg add "HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\TimeZoneInformation" /v RealTimeIsUniversal /d 1 /t REG_DWORD /f
   ```
4. Restart your computer back into Windows.
5. Right-click the clock in your taskbar -> **Adjust date/time** -> toggle **Set time automatically** off and back on to force a fresh sync.
