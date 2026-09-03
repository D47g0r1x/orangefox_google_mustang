# OrangeFox Recovery Project R12.1 Beta for Google Pixel 10 Pro XL (`mustang`)

```text
/*
 * Your warranty is now void.
 * You are choosing to make these modifications. Please back up your data first!
 */
```

Initial **Beta** release of the **OrangeFox Recovery Project (OFRP)** for the **Google Pixel 10 Pro XL** (codename: `mustang`), based on the **Google Tensor G5** (`laguna`) platform running Android 16/17.

---

## Device & Platform Specifications

* **Device:** Google Pixel 10 Pro XL (`mustang`)
* **Platform / SoC:** Google Tensor G5 (`laguna`) / ARM64
* **Kernel & Architecture:** GKI (Generic Kernel Image) with Boot Header v4
* **Target Partition:** `vendor_boot`
* **Stock Firmware Base:** Android 17 (`CP2A.260805.005`, August 2026 security patch level)
* **Maintainer:** `D47g0r1x`
* **OrangeFox Version:** `R12.1_Beta`

---

## What's Working

* **OrangeFox UI & Engine:** Native display layout tuned for the Pixel 10 Pro XL display (`1344x2992`) with punch-hole status bar padding.
* **Touchscreen:** Fully responsive touch interface.
* **USB Connectivity:** High-speed ADB and Fastbootd via USB controller (`c400000.dwc3`).
* **Dynamic Partitions:** Full logical partition recognition (`system`, `system_dlkm`, `system_ext`, `product`, `vendor`, `vendor_dlkm`).
* **Storage & OTG:** Full USB-OTG drive mounting and management (FAT32, exFAT, NTFS).
* **Core Functions:** Flashing zips, formatting, wiping, and terminal operations.
* **Built-in Tools:** `nano`, `bash`, `sgdisk`, `f2fs-tools`, and `ntfs-3g` included.
* **Magisk Integration:** Native `magiskboot` support and Magisk survival engine.
* **Phase 1 Decryption (DE):** Hardware metadata encryption mounted via `/metadata/vold/metadata_encryption`.
* **Security Daemons:** Recovery ramdisk packages Citadel daemon (`citadeld`), KeyMint (`android.hardware.security.keymint-service.citadel`), and Weaver HAL.

---

## Known Issues & Beta Limitations

* **Phase 2 Decryption (CE):** Hardware-backed user storage (`/data`) PIN/pattern/password unlock via Titan M2 / Weaver is currently under active testing. Testing with screen lock disabled in Android is recommended for evaluating file system access.

---

## Installation & Flashing Instructions

Because the Pixel 10 Pro XL uses Boot Header v4 GKI architecture, OrangeFox is installed to the `vendor_boot` partition.

### Option 1: Safe Test on Inactive Slot (Recommended)
```bash
# 1. Reboot to fastboot / bootloader
adb reboot bootloader

# 2. Identify active slot
fastboot getvar current-slot

# 3. Flash to the inactive slot (example assuming current-slot is A)
fastboot flash vendor_boot_b vendor_boot.img
fastboot --set-active=b
fastboot reboot recovery
```

### Option 2: Permanent Installation (Both Slots)
```bash
fastboot flash vendor_boot_a vendor_boot.img
fastboot flash vendor_boot_b vendor_boot.img
fastboot reboot recovery
```

---

## Bug Reports & Logs

To report issues or provide test logs:
```bash
adb pull /tmp/recovery.log recovery.log
adb logcat -d | grep -iE "citadel|keymint|weaver|gatekeeper|vold|fbe" > orangefox_crypto.log
```
