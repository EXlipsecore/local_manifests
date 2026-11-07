# ⚡ Instructions — Flash ROM on Aston & AstonC

> 📦 **Note:** firmware is included in the build

---

## 1️⃣ Method 1 — Clean Flash (Fastboot + Sideload)

> ✅ No slot or firmware requirements — you can flash this over any ROM.

**1.** 🔌 Reboot to bootloader

**2.** 💾 Flash boot, init_boot, vendor_boot, dtbo, vbmeta and recovery images

```bash
fastboot flash boot boot.img
fastboot flash dtbo dtbo.img
fastboot flash init_boot init_boot.img
fastboot flash vbmeta vbmeta.img
fastboot flash vendor_boot vendor_boot.img
fastboot flash recovery recovery.img
```

**3.** ♻️ Reboot to recovery

**4.** 📲 Put the recovery in sideload mode and push the ROM zip

```bash
adb sideload Rom*.zip
```

> 🛠️ Sideload failing with any error? Use the fix below, then start Method 1 again.

**5.** 🧹 Format data / Factory reset

**6.** 🚀 Reboot to system

---

### 🛠️ Fix for Any Sideload Error

**1.** ♻️ Reboot to recovery

**2.** ⚡ Enter fastbootd

```bash
fastboot wipe-super super_empty.img
```

**3.** 📲 Enter recovery

```bash
fastboot reboot recovery
```

**4.** 🧹 Format data

**5.** 📂 Sideload the ROM zip

```bash
adb sideload Rom*.zip
```

**6.** 🚀 Reboot

---

## 2️⃣ Method 2 — AERA Recovery

**1.** 🛠️ Flash AERA Recovery to both slots

```bash
fastboot flash --slot=all recovery recovery.img
```

**2.** 📂 Reboot to recovery → select the ROM ZIP → swipe to flash

**3.** ♻️ Reboot to recovery again — AERA won't format right after flashing

**4.** 🧹 Format data / Factory reset

> ⚠️ **Formatting may take two attempts.** If recovery restarts while formatting, wait for the menu to load again, format data once more, then reboot.

**5.** 🚀 Reboot to system
