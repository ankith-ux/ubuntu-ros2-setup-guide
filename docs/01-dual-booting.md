# 1. Dual-Booting Ubuntu with Windows

Setting up Ubuntu alongside Windows is the standard and recommended way to work with ROS 2. Virtual Machines (VMs) and WSL2 (Windows Subsystem for Linux) can work, but a native dual-boot gives you full hardware performance, which matters for robotics (cameras, GPU, sensors).

---

## 🛠️ What You Need
Before you begin, make sure you have:
1. **A USB Flash Drive** — at least **12GB** (everything on it will be erased).
2. **Rufus** — free software to flash the USB. Download from [rufus.ie](https://rufus.ie/).
3. **Ubuntu 24.04 LTS ISO file** — download from the [Alternative Downloads page](https://ubuntu.com/download/alternative-downloads).
4. **Back up important files** on your Windows system before starting. Just in case!

---

## 🛑 Crucial Pre-installation Steps (Do these in Windows first!)

### 1. Disable BitLocker
If your Windows drive is encrypted with BitLocker, Ubuntu will not be able to see the hard drive properly and will refuse to install alongside Windows. You **must** turn it off first.
* Go to Windows search → Type **Manage BitLocker** → Open it.
* Click **Turn off BitLocker** and wait for decryption to finish. This can take a while.
* *(If your laptop is school/organization-managed and you can't turn it off, at minimum select **Suspend Protection** and ensure your recovery key is saved).*

### 2. Disable Fast Startup
Fast Startup can cause issues reading the Windows drive from Linux.
* Open **Control Panel** → **Power Options** → **Choose what the power buttons do**.
* Click **Change settings that are currently unavailable**.
* Uncheck **Turn on fast startup** → Save changes.

### 3. Create Free Space (Shrinking Your Windows Partition)
Ubuntu needs its own space on the drive. We create this by shrinking the Windows partition.
* Right-click the **Start Button** → **Disk Management**.
* Right-click your `C:` drive → **Shrink Volume**.
* In the "Enter the amount of space to shrink" box, type:
  * **Minimum:** `50000` (50 GB)
  * **Recommended for ROS 2 + Gazebo:** `100000` (100 GB)
* Click **Shrink**. Leave the new free space as **Unallocated** — do not format it or create a new volume.

---

## 💾 Creating the Bootable USB with Rufus
1. Plug in your USB drive.
2. Open **Rufus**.
3. Under **Device**, select your USB drive (double-check — it will be erased!).
4. Click **SELECT** (next to "Boot selection") and choose the Ubuntu 24.04 ISO file.
5. Rufus will auto-detect the correct settings. Under **Partition scheme**, ensure **GPT** is selected (this is correct for modern UEFI laptops made after ~2014).
6. Leave all other settings at their defaults.
7. Click **START**.
   * If a dialog appears about **ISOHybrid image**, choose **"Write in ISO Image mode"** and click **OK**.
   * Confirm the warning that your USB data will be erased.
8. Wait for the progress bar to turn green and say **READY**. This usually takes about 10 minutes.

---

## ⚙️ BIOS/UEFI Settings
You need to tell your computer to boot from the USB, not Windows.
1. Restart your PC. As it boots up, repeatedly press your BIOS key — usually **`F2`**, **`F12`**, **`DEL`**, or **`ESC`** (check your laptop brand; a brief message often shows which key to press).
2. **Disable Secure Boot**: Find the **Secure Boot** option (usually in the *Security* or *Boot* tab) and set it to **Disabled**. *(Some drivers like NVIDIA won't load with Secure Boot on).*
3. **Intel RST / RAID (If drives are not detected)**: If your laptop uses Intel RST or RAID, Ubuntu cannot see the drive. Look for a SATA/Storage Mode setting and change it to **AHCI**. *(If Windows was already installed in RAID mode, you may need to boot Windows into Safe Mode first before switching).*
4. **Boot from USB**: Either change the Boot Order to put your USB at the top, or use the one-time boot menu (usually **`F12`**) to select your USB drive directly.
5. Save and exit (usually **`F10`**).

---

## 🐧 Installing Ubuntu 24.04 (Step-by-Step)

1. **Boot the installer.** Your PC should boot from the USB. Select your **language** and any **accessibility settings** needed.

2. **Connect to the internet.** Connect to Wi-Fi or Ethernet when prompted. This lets Ubuntu download updates and hardware drivers (like NVIDIA GPU drivers) during install.

3. **Choose "Try or Install Ubuntu".** On the next screen, click **Install Ubuntu** to proceed with installation. *(Clicking "Try Ubuntu" lets you preview the desktop without installing anything).*

4. **Choose Interactive Installation.** When asked between *Interactive installation* and *Automated installation*, select **Interactive installation** (the standard option).

5. **Choose Application Selection:**
   * **Default selection** — Minimal apps (you can add more later via the App Center).
   * **Extended selection** — Full suite of utilities and office tools, better for offline use.
   * For robotics development, **Extended selection** is recommended.

6. **Install Proprietary Software.** Check both boxes:
   * ✅ *Install third-party software for graphics and Wi-Fi hardware* (important for NVIDIA GPUs!)
   * ✅ *Download and install support for additional media formats*

7. **Installation Type (CRITICAL STEP):**
   * Select **"Install Ubuntu alongside Windows Boot Manager"**. This is the safest option — Ubuntu will automatically use the unallocated space you created earlier.
   * *(Advanced option: "Manual partitioning" lets you control every detail, but is not needed for most setups).*

8. **Create Your Login Details:**
   * Enter your **name**, a **computer name** (e.g., `ankith-laptop`), a **username**, and a **password**.
   * You will type this password every time you use `sudo` in the terminal, so pick something you remember.

9. **Choose Your Location.** Select your timezone on the map (auto-detected if you're online).

10. **Review and Install.** You'll see a summary. Click **Install** to begin. This takes 15–30 minutes.

11. **Restart.** When done, click **Restart Now**. When prompted, remove the USB drive and press **Enter**.

🎉 **Congratulations!** Your dual-boot is ready. On every startup, the **GRUB** bootloader will appear, letting you choose between Ubuntu 24.04 and Windows.

---

## ⏰ Post-Installation: Fix Windows Time Desync
After switching between Ubuntu and Windows, your Windows clock will likely show the wrong time (off by several hours). This is because Ubuntu stores the hardware clock in UTC, while Windows expects it in local time.

**Fix it in Windows (one-time setup):**
1. Boot into **Windows**.
2. Press `Windows` key → type `cmd` → right-click **Command Prompt** → **Run as administrator**.
3. Paste this command and press **Enter**:
   ```cmd
   reg add "HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\TimeZoneInformation" /v RealTimeIsUniversal /d 1 /t REG_DWORD /f
   ```
4. Restart Windows.
5. Right-click the clock in the taskbar → **Adjust date/time** → toggle **Set time automatically** off and back on to force a sync.

