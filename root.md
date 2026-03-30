# Universal Android Hardening & Deployment Roadmap

This document outlines the standard operating procedure for migrating a modern, commercially locked Android device (utilizing A/B partition schemes, Dynamic Partitions, and AVB 2.0) to a custom, hardened, de-Googled environment.

## ⚠️ Critical Advisories & Pre-Flight Checks

* **Hardware Interface Integrity:** You are transmitting sparse system images that directly write to the device's eMMC or UFS storage. Use a high-throughput, high-quality USB-C cable. Connect directly to a native motherboard I/O port. USB hubs, front-panel headers, or damaged cables introduce latency and packet loss, which can result in incomplete partition writes and hard bricks.
* **Cryptographic Wipe (Data Volatility):** Unlocking the bootloader is a severe state change. To prevent a malicious actor from stealing an encrypted device and unlocking it to flash a custom recovery to pull data, Android's security model dictates a mandatory cryptographic wipe. The `metadata` and `userdata` partitions are formatted, permanently destroying all encryption keys and user data.
* **A/B Partition Awareness:** Modern Android devices do not have a single `system` or `boot` partition. They have `boot_a` and `boot_b`, `system_a` and `system_b`. Updates are applied to the inactive slot. When flashing custom binaries, you must be aware of which slot is currently active.

---

## 📂 Phase 1: Environment Preparation

The foundation of universal Android modification relies entirely on the official Android Debug Bridge (ADB) and Fastboot protocols, managed via the Google Platform-Tools suite.

### 1. The Workstation Stack
* **Platform-Tools:** Download the latest `platform-tools` ZIP directly from the Android Developers portal. Do not use third-party "15-second ADB installers," as they rely on outdated binaries that cannot parse modern dynamic partitions.
* **System PATH:** Extract the tools and append the directory path to your OS's System Environment Variables. This allows you to invoke `adb` or `fastboot` from any directory without pathing errors.
* **Drivers:** On Windows, ensure the Google USB Driver is installed via Device Manager. Linux/macOS typically handle these interfaces natively via standard USB protocols.

### 2. Payload Acquisition & Extraction
You need specific `.img` files from your chosen hardened ROM. Modern ROMs are packaged as a `payload.bin` file rather than a collection of loose images. You may need a tool like `payload-dumper-go` to extract the necessary binaries:
* `boot.img` or `init_boot.img`: Handles the initial kernel initialization. (Crucial for rooting).
* `vendor_boot.img` or `recovery.img`: Depending on the device, custom recoveries are now often packaged into the `vendor_boot` partition rather than a dedicated recovery partition.
* `vbmeta.img`: The Verified Boot Metadata image containing the cryptographic hash trees.

---

## 🔓 Phase 2: The Silicon Handshake (Bootloader)

You must explicitly authorize the bootloader to accept unsigned images. 

### 1. Stripping Factory Reset Protection (FRP)
Navigate to **Settings > About Phone** and tap *Build Number* 7 times to unhide Developer Options. 
* Enable **OEM Unlocking**: This toggle communicates with a secure enclave/TrustZone on the device to drop Factory Reset Protection. If you do not do this, the bootloader will reject the unlock command.
* Enable **USB Debugging**: Opens the ADB daemon.

### 2. Executing the State Change
Connect to the workstation, authorize the RSA key prompt on the phone screen, and reboot to the bootloader:
```bash
adb reboot bootloader
```

Once in bootloader mode, verify the connection:
```bash
fastboot devices
```

Execute the unlock protocol (for devices 2015 and newer):
```bash
fastboot flashing unlock  
```
*Note: You must physically press the volume and power keys on the device to confirm. This intentionally breaks the hardware-backed Root of Trust. The device will wipe itself and reboot.*

---

## 💉 Phase 3: Integrity Override (AVB 2.0) & Recovery

Android Verified Boot (AVB) checks the cryptographic signature of the boot, system, and vendor partitions against a hardware-stored key. If you flash a custom OS, the signatures won't match, and the bootloader will halt the boot process.

### 1. Neutering AVB
You must flash a nullified `vbmeta` image and append flags to instruct the bootloader to ignore signature and hash mismatches.
```bash
fastboot flash vbmeta vbmeta.img --disable-verity --disable-verification
```

### 2. Flashing the Custom Recovery Environment
Depending on your device architecture, flash your custom recovery (TWRP, Lineage Recovery) to the appropriate partition:
```bash
# For devices with dedicated recovery partitions
fastboot flash recovery recovery.img

# For newer devices (Android 12+) using vendor_boot for recovery
fastboot flash vendor_boot vendor_boot.img
```

### 3. The Critical Handover
**Do not reboot to the system.** Unplug the device and use hardware key combinations to boot *directly* from the bootloader into your newly flashed Recovery. If the stock OS boots, its `init` scripts will detect the modified recovery and automatically overwrite it with the factory `recovery.img`.

---

## 💿 Phase 4: OS Deployment (Dynamic Partitions)

Modern Android uses "Dynamic Partitions" (a logical volume manager for Android). The `system`, `vendor`, and `product` partitions are grouped inside a single physical `super` partition. This makes traditional "wiping" obsolete and potentially dangerous.

### 1. Cryptographic Sanitization
In your Custom Recovery, navigate to the Wipe menu and select **Format Data** (you will usually have to type "yes" to confirm). This completely destroys the File-Based Encryption (FBE) layout left by the stock ROM, giving your new OS a clean, unencrypted block to format upon first boot.

### 2. OS Sideloading
Put the recovery into **ADB Sideload** mode. This opens a pipeline to stream the ROM zip directly to the device's RAM, bypassing the need to store the massive file on the device's internal storage first.
```bash
adb sideload path/to/os_package.zip
```
*Note: The terminal will usually stall at 47%. This is completely normal; 47% marks the end of the streaming phase and the beginning of the device-side extraction and installation phase.*

---

## ⚡ Phase 5: Escalating Privileges (Magisk Architecture)

*Correction from previous iteration: Sideloading Magisk.zip via custom recovery is no longer recommended by the Magisk developers for modern A/B devices, as it relies on legacy scripting that can fail to properly identify the active slot, resulting in a soft brick.*

### 1. Payload Extraction & Transfer
Extract the `boot.img` (or `init_boot.img` on Android 13+ devices) from your custom ROM's zip file. Transfer this file to your device's internal storage.

### 2. App-Based Patching
Boot into your newly installed custom ROM. Install the Magisk `.apk`. Open Magisk, select **Install**, and choose **"Select and Patch a File."** Select the `boot.img` you transferred. Magisk will unpack the image, inject the `magiskinit` binaries into the ramdisk, and repack it into a file named `magisk_patched.img`.

### 3. Flashing the Patched Kernel
Transfer `magisk_patched.img` back to your workstation. Reboot the phone to the bootloader.
```bash
adb reboot bootloader
```
Flash the patched image directly to the boot (or init_boot) partition:
```bash
fastboot flash boot magisk_patched.img
```
Reboot. You now have robust, systemless root access securely anchored in the kernel ramdisk.

---

## 🛡️ Phase 6: Post-Install Hardening & Network Sanitization

### 1. Evading Hardware Attestation (Play Integrity API)
Apps (banking, DRM-heavy media) use Google's Play Integrity API to check if the bootloader is unlocked. 
* **Zygisk:** Enable Zygisk in Magisk settings. This injects Magisk into the `zygote` daemon (the parent process of all Android apps), allowing you to modify app behavior before they fully load.
* **Play Integrity Fix (PIF):** Flash the latest PIF module via Magisk. This intercepts the attestation request and feeds it a valid, spoofed fingerprint from an older device that relies on software-backed (basic) attestation rather than hardware-backed attestation.

### 2. Network-Level Telemetry Blocking
* **Private DNS (DoT):** Navigate to Network Settings > Private DNS. Enter a provider like `dns.adguard-dns.com` or a configured NextDNS profile (`your-id.dns.nextdns.io`). This routes all DNS requests through an encrypted TLS tunnel, blocking ad-serving domains and analytics trackers OS-wide before they even connect.

### 3. Closing Physical Attack Vectors
Once your deployment is complete, return to Developer Options and **Disable USB Debugging**. Leaving the ADB daemon running is a massive physical security risk. If disabled, a bad actor plugging into your phone cannot access a shell or pull data.

---

