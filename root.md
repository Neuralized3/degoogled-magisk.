# Professional Android Customization & Hardening Guide

This guide provides the standard operating procedure for unlocking, flashing, and rooting a modern Android device. This process transitions a commercially locked phone to a privacy-focused, custom operating system.

> ⚠️ **Important Device Notice:** This guide applies to standard Android devices that use standard Fastboot protocols (e.g., Google Pixel, OnePlus, Motorola, Nothing). *Samsung devices use a proprietary tool called Odin and require a entirely different process.*

## 🛑 Pre-Flight Checks & Warnings

Before you begin, ensure you meet the following requirements to prevent permanently damaging (bricking) your device:
* **Data Loss is Mandatory:** Unlocking the bootloader will permanently erase all data, photos, and apps on the phone. Back up everything first.
* **Cable Quality Matters:** Use a high-quality data cable. Plug it directly into the motherboard ports on the back of your computer. USB hubs and front-panel ports can cause data drops during flashing, which causes hard bricks.
* **Battery Level:** Ensure your phone is charged to at least 70%.

---

## Phase 1: Setting Up Your Computer

You need the official tools from Google to send commands to your phone. Do not use third-party "1-click installers," as they are outdated and cannot handle modern Android partition sizes.

1. **Download Platform-Tools:** Download the official [Android SDK Platform-Tools](https://developer.android.com/tools/releases/platform-tools) for your operating system (Windows, Mac, or Linux).
2. **Extract the Folder:** Unzip the downloaded file to a safe place (like `C:\platform-tools`).
3. **Open Terminal/Command Prompt:** Navigate into that extracted folder, type `cmd` into the address bar (on Windows), and hit Enter. This opens a command window right where your tools are.
4. **Prepare Your Files:** You will need specific files from your custom ROM's developer. Download the ROM package (`.zip`). From inside that package, or from the developer's site, gather these image files and place them in your `platform-tools` folder:
   * `ROM.zip` 
   * `recovery.img`
   * `vbmeta.img`
If you cannot find them in the Custom ROM's developer's files, search your phone's model in XDA Forums, and go into the development tab, you'll definitely find roms, and the following packages. Download vbmeta patched and also Download the recovery image from either TWRP or Orangefox. I recommend TWRP.
---

## Phase 2: Unlocking the Bootloader

The bootloader is the security gatekeeper of your phone. You must unlock it to install a custom operating system.

### Step A: Prepare the Phone
1. Go to **Settings > About Phone**.
2. Tap **Build Number** 7 times quickly to unlock Developer Options.
3. Go back to Settings, navigate to **System > Developer Options**.
4. Turn on **OEM Unlocking** (This requires an internet connection on some devices).
5. Turn on **USB Debugging**.

### Step B: The Unlock Command
1. Connect your phone to your computer. Look at your phone screen and check the box to **"Always allow from this computer"** when prompted.
2. In your computer's command window, reboot the phone into "Fastboot Mode" by typing:
   ```bash
   adb reboot bootloader
   ```
3. Verify your computer sees the phone:
   ```bash
   fastboot devices
   ```
4. Send the unlock command:
   ```bash
   fastboot flashing unlock
   ```
   *Note: Your phone screen will change. Use the physical **Volume Keys** to select "Unlock the Bootloader," and press the **Power Key** to confirm. The phone will wipe itself and restart.*

---

## Phase 3: Flashing Custom Recovery & Bypassing AVB

Android Verified Boot (AVB) checks if your operating system is official. We must disable this check, or the phone will refuse to boot your custom ROM.

1. Reboot your phone back to the bootloader:
   ```bash
   adb reboot bootloader
   ```
2. Disable the AVB security checks by flashing the `vbmeta.img` file with special flags:
   ```bash
   fastboot flash vbmeta vbmeta.img --disable-verity --disable-verification
   ```
3. Flash your custom recovery environment (like TWRP or Lineage Recovery).
    *(For devices with a dedicated recovery partition):*
     ```bash
     fastboot flash recovery recovery.img
     ```
5. **CRITICAL STEP:** Do not let the phone boot up normally yet! Use the volume buttons to select **"Recovery Mode"** on the bootloader screen and press the Power button. If the phone boots normally, it will delete your custom recovery, and you will have to repeat this phase.

---

## Phase 4: Installing the Custom OS

Modern Android devices group multiple system parts into a single "dynamic partition." You will use ADB Sideloading, which safely streams the installation file from your PC directly to the phone's memory.

1. **Format Data:** On your phone's custom recovery screen, tap **Factory Reset** > **Format Data / Factory Reset**. This removes the factory encryption so your new OS can start fresh.
2. **Enable Sideloading:** Go back to the main recovery menu. Select **Apply Update** > **Apply from ADB**.
3. **Install the OS:** In your computer's command window, type the following command, replacing the file name with the actual name of your ROM file:
   ```bash
   adb sideload your_custom_rom.zip
   ```
   *Note: The command terminal will likely pause at 47%. This is completely normal and means the PC has finished sending the file, and the phone is currently installing it. Wait for the phone screen to say "Step 2/2 completed."*
4. Reboot your phone. Welcome to your new Custom OS!

---

## Phase 5: Rooting with Magisk (Systemless Root)

The safest way to get Root access on modern devices is to let the Magisk app patch your phone's kernel directly.

1. **Transfer the Kernel File:** Copy the `boot.img` (or `init_boot.img` for Android 13+) file you saved earlier onto your phone's internal storage.
2. **Patch the File:** 
   * Download and install the[Magisk APK](https://github.com/topjohnwu/Magisk) on your phone.
   * Open Magisk, tap **Install** (next to Magisk).
   * Choose **Select and Patch a File**.
   * Find and select the `boot.img` or `init_boot.img` file. Magisk will create a new file named `magisk_patched.img` in your Downloads folder.
3. **Transfer Back to PC:** Copy the newly created `magisk_patched.img` from your phone back to your computer's `platform-tools` folder.
4. **Flash the Rooted Kernel:** Reboot the phone back to the bootloader:
   ```bash
   adb reboot bootloader
   ```
5. Flash the patched image to the correct partition.
   * *If you patched a boot image:*
     ```bash
     fastboot flash boot magisk_patched.img
     ```
   * *If you patched an init_boot image (Android 13+):*
     ```bash
     fastboot flash init_boot magisk_patched.img
     ```
6. Type `fastboot reboot`. Your device is now securely rooted.

---

## Phase 6: Post-Install Hardening

To make your device secure and fully functional for daily use, perform these final steps.

### 1. Fix Banking Apps & Google Wallet
Because your bootloader is unlocked, banking apps and Google Pay will fail security checks (Play Integrity). 
* Open Magisk, go to Settings, and enable **Zygisk**. 
* Download the [Play Integrity Fix module](https://github.com/chiteroman/PlayIntegrityFix) on your phone.
* In Magisk, go to the **Modules** tab, tap **Install from storage**, and flash the downloaded module. Reboot the phone. This tricks apps into thinking the bootloader is locked.

### 2. Block Ads and Trackers System-Wide
* Go to Android **Settings > Network & Internet > Private DNS**.
* Select **Private DNS provider hostname**.
* Enter `dns.adguard-dns.com` (for basic ad-blocking) or your personal NextDNS URL. This routes all traffic through an encrypted tunnel, stripping out telemetry and advertisements before they load.

### 3. Close Physical Attack Vectors
* Go back to **Developer Options** and turn off **USB Debugging**. 
* *Why?* Leaving this on is a massive security risk. If a bad actor physically steals your phone and plugs it into a PC while debugging is on, they can bypass lock screens and extract your data. Keep it disabled unless you actively need it.
