# ⚡ Instructions — Flash ROM on Aston & AstonC

> 📦 **Note:** Firmware is included in the build.

---

## 1️⃣ Method 1 — Clean Flash (Fastboot + Sideload)

> ✅ No slot or firmware requirements. You can flash this over any ROM.

**1.** 🔌 Reboot into bootloader mode.

**2.** 💾 Flash the boot, init_boot, vendor_boot, dtbo, vbmeta, and recovery images.

```bash
fastboot flash boot boot.img
fastboot flash dtbo dtbo.img
fastboot flash init_boot init_boot.img
fastboot flash vbmeta vbmeta.img
fastboot flash vendor_boot vendor_boot.img
fastboot flash recovery recovery.img
```

**3.** ♻️ Reboot into recovery.

**4.** 📲 Put the recovery into sideload mode, then sideload the ROM ZIP.

```bash
adb sideload <ROM-file>.zip
```

**5.** ❓ If recovery asks whether to install additional packages, select **No**, unless you plan to flash another package after the ROM.

**6.** 🧹 Format data / factory reset.

**7.** 🚀 Reboot to system.

---

## 🛠️ Fix for Any Sideload Error

**1.** ♻️ Reboot into recovery.

**2.** ⚡ Enter fastbootd from the recovery menu, or run:

```bash
fastboot reboot fastboot
fastboot wipe-super super_empty.img
```

**3.** 📲 Enter recovery from the menu, or run:

```bash
fastboot reboot recovery
```

**4.** 🧹 Format data.

**5.** 📂 Sideload the ROM ZIP.

```bash
adb sideload <ROM-file>.zip
```

**6.** 🚀 Reboot to system.

---

## 2️⃣ Method 2 — AERA Recovery

> Use OrangeFox instead if AERA does not boot or displays a black screen.

**1.** 🛠️ Flash AERA Recovery.

```bash
fastboot flash recovery recovery.img
```

**2.** 📂 Reboot into recovery, select the ROM ZIP, then swipe to flash it.

**3.** ♻️ After flashing completes, reboot back into recovery. AERA may not allow formatting immediately after the installation.

**4.** 🧹 Format data / factory reset.

> ⚠️ **Formatting may take two attempts.** If recovery restarts during formatting, wait for the menu to load, format data again, then reboot.

**5.** 🚀 Reboot to system.

---

## 3️⃣ Method 3 — OrangeFox Recovery

> Use OrangeFox only for custom ROMs. Do not flash stock firmware with this method.

**1.** 🛠️ Flash OrangeFox.

```bash
fastboot flash recovery recovery.img
```

**2.** ♻️ Reboot into OrangeFox Recovery.

```bash
fastboot reboot recovery
```

**3.** 📂 In OrangeFox, tap **Files**, select the ROM ZIP, then swipe to flash it.

**4.** ♻️ After the ROM finishes flashing, reboot back into recovery.

**5.** 🧹 Select **Wipe → Format Data**, type `yes`, then confirm.

**6.** 🚀 Reboot to system.

---

## 4️⃣ Method 4 — Fastboot Firmware Flasher

> Use **FastbootFirmwareFlasher v2.0.0.6 or newer** to flash stock firmware or a compatible custom ROM package. Use firmware made specifically for Aston / AstonC. This process will erase all data.

**1.** 📥 Download Fastboot Firmware Flasher and the required stock firmware or custom ROM ZIP.

**2.** 📦 Open Fastboot Firmware Flasher. Unpack the downloaded ZIP, then extract its `payload.bin` into image files.

**3.** 🔌 Boot the phone into bootloader mode and connect it to your PC.

**4.** 🛠️ In Fastboot Firmware Flasher, select the unpacked firmware folder, then start the flash.

**5.** ⏳ Wait for flashing to finish. Do not disconnect the phone during the process.

**6.** 🚀 Reboot to system after the tool reports success.
