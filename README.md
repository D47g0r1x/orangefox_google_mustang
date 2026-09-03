# OrangeFox Recovery Project for Google Pixel 10 Pro XL (`mustang`)

```text
/*
 * Your warranty is now void.
 *
 * We are not responsible for bricked devices, dead flash memory, thermonuclear war,
 * or your alarm failing because the phone stayed in recovery mode.
 * YOU are choosing to make these modifications. Please backup your data first!
 */
```

Welcome to the official port of the **OrangeFox Recovery Project (OFRP)** for the **Google Pixel 10 Pro XL** (codename: `mustang`), based on the **Google Tensor G5** (`laguna`) platform running Android 16/17.

> 🦊 **IMPORTANT BETA DISCLAIMER**  
> This is an **experimental Beta build** brought up specifically for Pixel 10 Pro XL testers. While standard OrangeFox features (UI engine, theme customization, file manager, backup/restore, fastbootd, ADB, and Magisk survival) are operational, **Phase 2 Titan M2 user credential decryption (PIN/pattern/password unlock)** is under active testing. Always keep a complete backup of your device before testing!

---

## Device & Platform Specs

| Property | Details |
| :--- | :--- |
| **Device Model** | Google Pixel 10 Pro XL |
| **Codename** | `mustang` |
| **SoC / Platform** | Google Tensor G5 (`laguna`) |
| **Architecture** | ARM64 (`armv8-a` / `armv9-a` cores, 64-bit only) |
| **Kernel Type** | GKI (Generic Kernel Image) with Boot Header v4 |
| **Target Partition** | `vendor_boot` (`/dev/block/platform/3c400000.ufs/by-name/vendor_boot`) |
| **Firmware Base** | Android 17 (`CP2A.260805.005`, August 2026 patch level) |
| **Maintainer** | `D47g0r1x` |

---

## What Works & Current Beta Status

- [x] **OrangeFox UI & Engine**: Native display scaling for Pixel 10 Pro XL punch hole display (`1344x2992`).
- [x] **ADB & Fastbootd**: High-speed USB communication via `c400000.dwc3`.
- [x] **Touchscreen**: Touch input responsive across recovery menus.
- [x] **Storage & OTG**: External USB-OTG drives (FAT32, exFAT, NTFS) mount and browse properly.
- [x] **Dynamic Partitions**: Full support for logical partitions (`system`, `system_dlkm`, `system_ext`, `product`, `vendor`, `vendor_dlkm`).
- [x] **Phase 1 Decryption (DE)**: Hardware metadata encryption mounted via `/metadata/vold/metadata_encryption`.
- [x] **Titan M2 Citadel Daemons**: Bundled Citadel daemon (`citadeld`), KeyMint, and Weaver HAL daemons in recovery ramdisk.
- [/] **Phase 2 Decryption (CE)**: **[Active Beta Testing]** Unlocking user data (`/data`) with PIN/Pattern via Titan M2. Please report feedback and logs!

---

## Quick Flashing Guide

Because the Pixel 10 Pro XL uses GKI architecture, recovery is contained within `vendor_boot.img`.

### Testing on Inactive Slot (Recommended)
```bash
# 1. Boot to bootloader
adb reboot bootloader

# 2. Check active slot
fastboot getvar current-slot
# e.g., current-slot: a

# 3. Flash OrangeFox to the inactive slot
fastboot flash vendor_boot_b vendor_boot.img

# 4. Set slot b active and boot recovery
fastboot --set-active=b
fastboot reboot recovery
```

### Permanent Installation
```bash
fastboot flash vendor_boot_a vendor_boot.img
fastboot flash vendor_boot_b vendor_boot.img
fastboot reboot recovery
```

---

## How to Report Beta Issues & Logs

If decryption or touch requires troubleshooting:
1. Connect via USB while in OrangeFox.
2. Pull recovery logs:
   ```bash
   adb pull /tmp/recovery.log
   adb logcat -d | grep -iE "citadel|keymint|weaver|gatekeeper|vold" > orangefox_crypto.log
   ```
3. Open an issue on this repository with your logs attached.

---

## Credits
- **The OrangeFox Recovery Project team** for the recovery environment.
- **TeamWin (TWRP)** for the underlying recovery framework.
- **Google** for AOSP and Tensor platform files.
- The Pixel developer community for continuous testing.
