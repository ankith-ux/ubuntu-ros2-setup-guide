# 1. Dual-Booting Ubuntu with Windows

Setting up Ubuntu alongside Windows is the standard and recommended way to work with ROS 2. Virtual Machines (VMs) and WSL2 (Windows Subsystem for Linux) can work, but a native dual-boot gives you full hardware performance, which matters for robotics (cameras, GPU, sensors, USB devices).

> ⏱️ **Time needed:** About 1.5–2 hours total (most of it is waiting). Keep your laptop **plugged into the charger** the whole time.

---

## 🛠️ What You Need
Before you begin, make sure you have:
1. **A USB Flash Drive** — at least **8GB** (everything on it will be erased).
2. **Rufus** — free software to flash the USB. Download from [rufus.ie](https://rufus.ie/).
3. **Ubuntu 24.04 LTS ISO file** — download the **Desktop image (64-bit PC / AMD64)** from [releases.ubuntu.com/24.04](https://releases.ubuntu.com/24.04/).
   > ⚠️ **Careful:** The main [ubuntu.com/download](https://ubuntu.com/download/desktop) page now gives you **Ubuntu 26.04** by default. ROS 2 Jazzy does **not** install on 26.04. Make sure the file you download is named like `ubuntu-24.04.x-desktop-amd64.iso`.
4. **Back up important files** on your Windows system before starting. Just in case!
5. **Your BitLocker recovery key** (if your drive is encrypted) — saved somewhere *other than* this laptop, e.g. your phone. You can find it at [aka.ms/myrecoverykey](https://aka.ms/myrecoverykey).

---

## 🛑 Crucial Pre-installation Steps (Do these in Windows first!)

### 1. Disable BitLocker / Device Encryption
If your Windows drive is encrypted, Ubuntu will not be able to see the hard drive properly and will refuse to install alongside Windows. You **must** turn it off first. Many new laptops ship with encryption turned on **without telling you**, so check even if you never enabled it.
* **Windows Pro/Education:** Go to Windows search → Type **Manage BitLocker** → Open it → Click **Turn off BitLocker**.
* **Windows Home:** Go to **Settings** → **Privacy & security** → **Device encryption** → Turn it **Off**. *(If you don't see this option, your drive is not encrypted.)*
* Wait for decryption to finish completely. This can take a while (30+ minutes).
* *(If your laptop is school/organization-managed and you can't turn it off, at minimum select **Suspend Protection** and ensure your recovery key is saved).*

### 2. Disable Fast Startup
Fast Startup doesn't fully shut Windows down, which can lock the Windows drive and cause issues reading it from Linux.
* Open **Control Panel** → **Power Options** → **Choose what the power buttons do**.
* Click **Change settings that are currently unavailable**.
* Uncheck **Turn on fast startup** → Save changes.

### 3. Create Free Space (Shrinking Your Windows Partition)
Ubuntu needs its own space on the drive. We create this by shrinking the Windows partition.
* Right-click the **Start Button** → **Disk Management**.
* Right-click your `C:` drive → **Shrink Volume**.
* In the "Enter the amount of space to shrink in MB" box, type:
  * **Minimum:** `51200` (50 GB)
  * **Recommended for ROS 2 + Gazebo:** `102400` (100 GB)
* Click **Shrink**. Leave the new free space as **Unallocated** — do not format it or create a new volume.

> 💡 **Windows won't let you shrink enough?** Windows can't move certain system files. Try: run **Disk Cleanup** (include "system files"), temporarily disable **hibernation** (`powercfg /h off` in an admin Command Prompt), restart, and try shrinking again.

---

## 💾 Creating the Bootable USB with Rufus
1. Plug in your USB drive.
2. Open **Rufus**.
3. Under **Device**, select your USB drive (double-check — it will be erased!).
4. Click **SELECT** (next to "Boot selection") and choose the Ubuntu 24.04 ISO file.
5. Rufus will auto-detect the correct settings. Under **Partition scheme**, ensure **GPT** is selected and **Target system** shows **UEFI** (this is correct for modern laptops made after ~2014).
6. Leave all other settings at their defaults.
7. Click **START**.
   * If a dialog appears about **ISOHybrid image**, choose **"Write in ISO Image mode (Recommended)"** and click **OK**.
   * Confirm the warning that your USB data will be erased.
8. Wait for the progress bar to turn green and say **READY**. This usually takes about 10 minutes.

---

## ⚙️ BIOS/UEFI Settings
You need to tell your computer to boot from the USB, not Windows.

> ⚠️ **Do this only AFTER BitLocker/Device Encryption is fully off.** Changing BIOS settings on an encrypted drive will make Windows ask for the recovery key on the next boot.

1. Restart your PC. As it boots up, repeatedly press your BIOS key — usually **`F2`**, **`F12`**, **`DEL`**, or **`ESC`** (check your laptop brand; a brief message often shows which key to press).
   * *Can't catch the key?* In Windows, hold `Shift` while clicking **Restart** → **Troubleshoot** → **Advanced options** → **UEFI Firmware Settings**.
2. **Secure Boot (Recommended: Disable)**: Find the **Secure Boot** option (usually in the *Security* or *Boot* tab) and set it to **Disabled**. Ubuntu *can* run with Secure Boot on, but turning it off avoids extra driver-signing (MOK) steps, especially on laptops with NVIDIA GPUs.
   * *Note for gamers:* Some Windows games' anti-cheat (e.g. Valorant) require Secure Boot. If you need it on, you can leave it enabled — just choose a password when the Ubuntu installer asks about Secure Boot, and select **Enroll MOK** → enter that password on the blue screen after the first reboot.
3. **Intel RST / RAID (If drives are not detected)**: If your laptop uses Intel RST, RAID or VMD, Ubuntu may not see the drive. Look for a SATA/Storage Mode setting and change it to **AHCI**. *(If Windows was already installed in RAID mode, Windows will fail to boot after switching unless you boot it into Safe Mode once first — ask someone experienced for help with this step).*
4. **Boot from USB**: Either change the Boot Order to put your USB at the top, or use the one-time boot menu (usually **`F12`**, or `F9`/`ESC` on HP) to select your USB drive directly.
5. Save and exit (usually **`F10`**).

---

## 🐧 Installing Ubuntu 24.04 (Step-by-Step)

1. **Boot the installer.** Your PC should boot from the USB and show a black menu — choose **"Try or Install Ubuntu"**. Once the installer opens, select your **language** and any **accessibility settings** needed.

2. **Choose your keyboard layout.** The default (**English (US)**) is correct for most laptops.

3. **Connect to the internet.** Connect to Wi-Fi or Ethernet when prompted. This lets Ubuntu download updates and hardware drivers (like NVIDIA GPU drivers) during install.

4. **Choose "Install Ubuntu".** On the "Try or install Ubuntu" screen, click **Install Ubuntu** to proceed with installation. *(Choosing "Try Ubuntu" lets you preview the desktop without installing anything).*

5. **Choose Interactive Installation.** When asked between *Interactive installation* and *Automated installation*, select **Interactive installation** (the standard option).

6. **Choose Application Selection:**
   * **Default selection** — Minimal apps (you can add more later via the App Center).
   * **Extended selection** — Full suite of utilities and office tools, better for offline use.
   * Either works for robotics. **Default selection** keeps things lighter; choose **Extended** if you want office tools.

7. **Install Proprietary Software.** Check both boxes:
   * ✅ *Install third-party software for graphics and Wi-Fi hardware* (important for NVIDIA GPUs!)
   * ✅ *Download and install support for additional media formats*

8. **Installation Type (CRITICAL STEP):**
   * Select **"Install Ubuntu alongside Windows Boot Manager"**. This is the safest option — Ubuntu will automatically use the unallocated space you created earlier.
   * 🚨 **NEVER select "Erase disk and install Ubuntu"** — this will wipe Windows and all your files!
   * *(Advanced option: "Manual installation" lets you control every detail, but is not needed for most setups).*
   * **Don't see the "alongside" option?** Go back and check: BitLocker/Device Encryption is fully off, Fast Startup is off, and RST/RAID is switched to AHCI. See [Troubleshooting](#-troubleshooting) below.

9. **Create Your Login Details:**
   * Enter your **name**, a **computer name**, a **username**, and a **password**.
   * You will type this password every time you use `sudo` in the terminal, so pick something you remember.
   * Use a short, lowercase **username** with no spaces — it becomes part of all your file paths.

10. **Choose Your Location.** Select your timezone on the map (auto-detected if you're online).

11. **Review and Install.** You'll see a summary. Double-check that it says Ubuntu will be installed in the **free space**, and that Windows is **not** being erased. Click **Install** to begin. This takes 15–30 minutes.

12. **Restart.** When done, click **Restart Now**. When prompted, remove the USB drive and press **Enter**.

🎉 **Congratulations!** Your dual-boot is ready. On every startup, the **GRUB** bootloader (a purple/black menu) will appear, letting you choose between Ubuntu and **Windows Boot Manager**.

### ✅ First boot checklist
Once you're logged in to Ubuntu, open a terminal (`Ctrl` + `Alt` + `T`) and bring the system fully up to date:
```bash
sudo apt update && sudo apt upgrade -y
```
Then restart once, and also boot into **Windows** once from the GRUB menu to confirm it still works.

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

---

## 🧯 Troubleshooting

| Problem | Fix |
| --- | --- |
| PC boots straight into Windows, no GRUB menu | Enter BIOS → **Boot order** → move **ubuntu** above **Windows Boot Manager**. |
| "Install alongside Windows" option is missing | BitLocker/Device Encryption still on, Fast Startup still on, or storage mode is RST/RAID instead of AHCI. |
| USB doesn't show up in the boot menu | Re-flash with Rufus using **GPT / UEFI**, try another USB port, and make sure Secure Boot / "USB boot" is allowed in BIOS. |
| Windows asks for a BitLocker recovery key | Enter the key you saved from [aka.ms/myrecoverykey](https://aka.ms/myrecoverykey). This happens if encryption was still on when BIOS settings changed. |
| No Wi-Fi in Ubuntu after install | Connect via Ethernet or phone USB tethering, then open **Software & Updates** → **Additional Drivers** and install the recommended driver. |
| Black screen / freeze on boot (NVIDIA laptops) | At the GRUB menu press `e`, add `nomodeset` after `quiet splash`, press `F10` to boot, then install the NVIDIA driver from **Additional Drivers**. |

---

[Next → Choosing the right Ubuntu & ROS 2 versions](02-ubuntu-versions.md)
