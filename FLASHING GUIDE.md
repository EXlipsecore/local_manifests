<div align="center">

# ⚡ Flash a Custom ROM on OnePlus 12R &amp; Ace 3

**The clean, beginner-safe flashing guide for `aston` (12R) and `astonc` (Ace 3) — no slot headaches, no guesswork.**

![Device](https://img.shields.io/badge/OnePlus-12R%20%7C%20Ace%203-EB0029?style=for-the-badge&logo=oneplus&logoColor=white)
![Codename](https://img.shields.io/badge/Codename-aston%20%7C%20astonc-6C5CE7?style=for-the-badge)
![Difficulty](https://img.shields.io/badge/Difficulty-Beginner%20Friendly-00C853?style=for-the-badge)

<!-- 📸 BANNER — replace USERNAME/REPO with your GitHub username and repo name, and upload assets/banner.jpg -->
<img src="https://raw.githubusercontent.com/USERNAME/REPO/main/assets/banner.jpg" alt="OnePlus 12R / Ace 3 custom ROM flashing guide" width="100%">

[⚡ Method 1](#method-1) · [🛠️ Method 2](#method-2) · [🩺 Troubleshooting](#troubleshooting) · [❓ FAQ](#faq) · [⌨️ Cheat Sheet](#cheat-sheet)

</div>

---

> [!CAUTION]
> **Read this before touching anything.**
> - Flashing **erases everything** on the phone — photos, chats, files, app data. Back up first.
> - `aston` (12R) and `astonc` (Ace 3) share hardware but **not** firmware or partition layout. Flashing an `aston` ROM on an `astonc` device (or the reverse) can hard-brick it. Always match the ROM to your codename.
> - An **unlocked bootloader is required**. Unlocking also wipes data and may void warranty.
> - You flash at your own risk. Neither the tool authors nor the ROM maintainers are responsible for a bricked device.

### 🔍 First, confirm your codename

Don't guess — check it. With USB debugging enabled, run:

```bash
adb shell getprop ro.product.device
```

| Output | Your device | Model numbers |
| :-- | :-- | :-- |
| `aston` | **OnePlus 12R** | CPH2585 (India) · CPH2609 (Global/EU) · CPH2611 (North America) |
| `astonc` | **OnePlus Ace 3** | PJE110 (China) |

> Whatever the guide or a download page says, **the ROM filename must contain your codename**. `PixelOS-...-aston-...zip` → 12R only. `...-astonc-...zip` → Ace 3 only.

---

## 🧭 Which method should I use?

| | ⚡ **Method 1 — Firmware Flasher** | 🛠️ **Method 2 — Manual Fastboot** |
| :-- | :-- | :-- |
| **Difficulty** | ⭐ Easy | ⭐⭐ Moderate |
| **Fastboot commands** | None — fully automated | 6 commands, typed by hand |
| **Firmware requirement** | None. Firmware is bundled in the ROM | OxygenOS / ColorOS **16.0.1.300+** on **both slots** |
| **Slot management** | Handled automatically | You must ensure both slots match |
| **Operating system** | Windows | Windows · macOS · Linux |
| **Best for** | Everyone — this is the default | Advanced users, and as a fallback if Method 1 fails |

**Pick Method 1.** Only reach for Method 2 if you know why you need it.

---

# ✅ Before You Start

### Requirement checklist

- [ ] OnePlus **12R (`aston`)** or **Ace 3 (`astonc`)** with an **unlocked bootloader**
- [ ] ROM zip matching your **codename**
- [ ] A Windows PC (Method 1) — Windows, macOS or Linux (Method 2)
- [ ] Latest [Android Platform Tools](https://developer.android.com/tools/releases/platform-tools) installed
- [ ] OnePlus USB drivers installed (Windows)
- [ ] A **data** USB cable — charge-only cables cause hour-long debugging sessions
- [ ] Battery **above 50%**
- [ ] 20–30 uninterrupted minutes
- [ ] A full backup (cloud or PC)

### 💾 Back up before you flash

Flashing wipes internal storage. At minimum, get Google backup on, copy photos to your PC/cloud, export authenticator seeds, and note your banking/UPI re-login details. There is no undo.

### ⌨️ Know your button combos

| Mode | Phone off — hold… | From PC (USB debugging on) |
| :-- | :-- | :-- |
| **Fastboot** | Power + Volume Up + Volume Down | `adb reboot bootloader` |
| **Recovery** | Power + Volume Down (until the logo shows) | `adb reboot recovery` |
| **FastbootD** | — (from recovery: *Advanced → Reboot to fastboot*) | `fastboot reboot fastboot` (from fastboot) |

---

<a id="method-1"></a>

# ⚡ Method 1 — Firmware Flasher *(Recommended)*

The easiest and safest route: a single tool unpacks the ROM, flashes it, and handles the slot logic for you. **No fastboot commands to type, and the firmware is already inside the ROM zip.**

### What you're doing

> **Unpack the ROM → flash it with the tool → format data → boot.**

### Step 1 — Download the right files

| File | Where to get it | Watch out for |
| :-- | :-- | :-- |
| **ROM zip** | Your ROM's official download page or Telegram support group | Filename must contain `aston` (12R) or `astonc` (Ace 3) |
| **Firmware Flasher** | NeFeroN's *Fastboot Firmware Flasher* — **v1.0.2.0 or newer** | The unpacker is a tab *inside* the flasher; you don't need a separate app |

> [!NOTE]
> The ROM zip must be a **full OTA–style package** (it contains `payload.bin`). Practically every current custom ROM build for these devices is. If the unpacker can't find `payload.bin`, skip to [Method 2](#method-2).

### Step 2 — Prepare the PC

1. Extract the tool to a short folder path with **no spaces or non-English characters**, e.g. `C:\Flasher\`.
2. **Right-click → Run as administrator.**
3. If Windows blocks the download: right-click the ZIP → **Properties → Unblock → Apply** before extracting.
4. Windows Defender may flag the `.exe` — it's a false positive from an unsigned community tool. Only proceed if you downloaded it from a trusted source.

### Step 3 — Unpack the ROM

1. Open the tool and go to the **Firmware Unpacker** tab (📦 archive icon).
2. Click **File Picker** and select the ROM zip.
3. Set **Mode** to **Full** — not Incremental.
4. Click **Unpack** and wait for **“Unpacking Completed.”**
5. Don't close the window while it's still extracting.

### Step 4 — Flash the firmware

1. Go to the **Firmware Flasher** tab (⚡ lightning icon).
2. Click **START FLASHING**.
3. When the tool asks to reboot into **FastbootD**, click **OK** and let it happen.
4. **Don't touch the phone, the cable, or the PC** until it finishes.
5. Wait for **“Flashing completed”**, then click **Reboot Device**.

### Step 5 — Format data *(do not skip)*

A dirty encryption key from the old ROM will bootloop the new one.

1. Reboot into **Recovery** (table above, or *Advanced → Reboot to recovery* from FastbootD).
2. **Factory Reset → Format Data** → type `yes` to confirm.
3. Reboot to system.

> [!TIP]
> The **first boot can take 5–10 minutes** (“Optimizing apps”). Do not panic, and do not force-power-off. Once you see the setup screen, you're done. 🎉

### Why this method wins

- ✅ **Firmware included** — flash from any OxygenOS/ColorOS version, no version checks
- ✅ **No slot worries** — the tool writes firmware and boot images correctly
- ✅ **No typos** — nothing to type, nothing to mistype
- ✅ **Recoverable** — if something goes sideways, re-run the same tool

---

<a id="method-2"></a>

# 🛠️ Method 2 — Manual Fastboot + Sideload

The classic, transparent route: flash the boot images yourself, then sideload the ROM in recovery. Use it if Method 1 fails, you're not on Windows, or you simply want to see every step.

> [!IMPORTANT]
> **Prerequisites — both are mandatory:**
> 1. Your device is already running **OxygenOS/ColorOS 16.0.1.300 or newer**.
> 2. **Both slots carry the same firmware.** If you're unsure, flash the full stock build once with [Method 1](#method-1) — that writes the firmware properly — then come back here.

Check your build version under **Settings → About device → Version**, or:

```bash
adb shell getprop ro.build.version.ota
```

### Step 1 — Extract the images

Unpack the ROM first, then copy these six images into your **platform-tools** folder.

> [!TIP]
> Still inside the ROM zip? Extract it, then dump `payload.bin` with `payload-dumper-go` (`payload-dumper-go payload.bin`). On Windows, the Firmware Unpacker from [Method 1](#method-1) does the same thing.

The images you need:

| Image | What it carries |
| :-- | :-- |
| `boot.img` | Kernel + ramdisk |
| `dtbo.img` | Device tree overlays |
| `init_boot.img` | First-stage ramdisk (Android 13+) |
| `vendor_boot.img` | Vendor ramdisk + modules |
| `vbmeta.img` | Verified Boot (AVB) metadata |
| `recovery.img` | Recovery |

### Step 2 — Boot to fastboot

Enable **USB debugging**, connect the phone, and confirm it's visible:

```bash
adb devices
adb reboot bootloader
fastboot devices
```

> `fastboot devices` must print your serial number. If it's blank, stop and fix the driver/cable first — see [Troubleshooting](#troubleshooting).

### Step 3 — Flash the images

```bash
fastboot flash boot boot.img
fastboot flash dtbo dtbo.img
fastboot flash init_boot init_boot.img
fastboot flash vbmeta vbmeta.img
fastboot flash vendor_boot vendor_boot.img
fastboot flash recovery recovery.img
```

> No `--slot` arguments are needed — fastboot targets the **active slot** automatically. If any line fails, **stop**. Don't reboot and hope for the best.

### Step 4 — Recovery + sideload

```bash
fastboot reboot recovery
```

In recovery, on the phone:

1. **Factory Reset → Format Data** → confirm with `yes`
2. Back to the main menu → **Apply update → Apply from ADB**
3. On the PC, from your platform-tools folder:

```bash
adb sideload rom.zip
```

> [!TIP]
> - Replace `rom.zip` with the real filename (you can drag the file into the terminal to autofill the path).
> - The phone's progress bar often **stalls at 47%** — that is normal. Watch the PC instead; it finishes with `Total xfer: 1.00x`.
> - If it stops with a “cannot read” / signature error, update your **platform-tools** — older `adb` breaks on newer recovery.

### Step 5 — Finish

1. Back in recovery: **Factory Reset → Format Data** again.
2. **Reboot to system.**
3. First boot takes a few minutes. Enjoy. 🚀

> [!WARNING]
> Never sideload a zip built for the other codename, and never relock the bootloader while a custom ROM is installed — that's the fastest way to brick the phone.

---

<a id="troubleshooting"></a>

# 🩺 Troubleshooting

| Symptom | Fix |
| :-- | :-- |
| `waiting for any device` | Reinstall OnePlus USB drivers · try another cable/port (USB 2.0 preferred) · run the terminal as admin · revoke & re-authorize USB debugging |
| `fastboot devices` shows nothing | Phone isn't in fastboot, or the driver isn't bound. Try the button combo from the table. On Windows, check Device Manager for unknown devices. |
| `Flashing is not allowed in Lock State` | Bootloader is locked. Unlock it first — that's a separate procedure and it wipes data. |
| Unpacker: “payload.bin not found” | The zip isn't a full OTA package. Use [Method 2](#method-2), or re-download the ROM from its official source. |
| Unpacking fails midway | Corrupted download or low disk space. Verify the zip's size, re-download, and keep 15+ GB free. |
| Tool won't launch / instant crash | Path contains spaces or non-English characters — move it to `C:\Flasher\`. Also unblock the zip (Properties → Unblock). |
| Antivirus deletes the `.exe` | Unsigned community tool → false positive. Restore it and add an exclusion, **but only if you trust the source**. |
| `adb: sideload connection failed` | Update platform-tools · try another USB port · restart the sideload from recovery |
| Stuck at 47% during sideload | Not stuck. Wait — check the PC terminal for completion. |
| Bootloop after flashing | Boot to recovery → **Format Data** → reboot. You almost certainly skipped it. |
| Stuck in FastbootD | `fastboot reboot` or `fastboot reboot recovery`, then retry the step. |
| Blank screen / total brick | Stop. Keep the battery charged and seek a service centre with MSM/EDL access — modern OnePlus tools need authorized access, so DIY recovery here is limited. |

---

<a id="faq"></a>

# ❓ FAQ

**Do I need root or a custom recovery already installed?**
No. Both methods work on a stock, bootloader-unlocked device.

**Will I lose my data?**
Yes — both methods wipe internal storage. That's not a bug, it's Android's encryption.

**Do the ROMs include firmware now?**
The current builds do, which is why [Method 1](#method-1) works from any OxygenOS/ColorOS version. Method 2 still needs **16.0.1.300+ on both slots**.

**Can I switch between `aston` and `astonc` ROMs?**
No. Same hardware, different firmware and partition layout — cross-flashing risks a hard brick.

**Does flashing relock the bootloader?**
No. Relocking while a custom ROM is installed can brick the device. Don't.

**Will banking / UPI apps work?**
It varies. Some ROMs pass Play Integrity, some need extra steps. Check your ROM's support group before flashing if this matters to you.

**How do I go back to OxygenOS/ColorOS?**
Flash the official full OTA zip the same way as Method 1, then format data. (Downgrades may be restricted — check the official firmware notes first.)

**Which ROM should I pick?**
That's a personal choice — read the ROM's thread and support group, check the codename, and pick an active build with recent security patches.

---

<a id="cheat-sheet"></a>

# ⌨️ Cheat Sheet

```bash
adb devices                          # is the phone connected?
adb reboot bootloader                # → fastboot
fastboot devices                     # is fastboot connected?
fastboot reboot                      # leave fastboot
fastboot reboot recovery             # → recovery
adb sideload rom.zip                 # push the ROM in recovery
adb shell getprop ro.product.device  # confirm aston / astonc
```

**Golden rules**

1. Match the ROM to your **codename**, always.
2. Read the prompt before clicking **OK**.
3. Never unplug during flashing.
4. **Format Data** after flashing — every time.
5. When in doubt, flash **Method 1** again.

---

# 🔗 Credits & Useful Links

- **Fastboot Firmware Flasher** — by **NeFeroN**; the unpack + flash engine behind Method 1.
- **ROM maintainers** — the people building and testing ROMs for `aston` / `astonc`. Support them in their threads and groups.
- [Android Platform Tools](https://developer.android.com/tools/releases/platform-tools) — the official `adb` / `fastboot`.
- [PixelOS for OnePlus 12R / Ace 3 — XDA thread](https://xdaforums.com/t/rom-17-official-pixelos-for-oneplus-12r-ace3-aosp-01-10-26.4662225/)
- [Device tree — OnePlus Ace 3 (`astonc`)](https://github.com/EXlipsecore/android_device_oneplus_astonc)
- [Recovery device tree — 12R / Ace 3 (`aston`)](https://github.com/EXlipsecore/android_device_oneplus_aston-AERA)

# 🤝 Contributing

Found a mistake, a broken command, or a step that needs clarifying? Open an **issue** or send a **PR** — keep the guide accurate for the next person.

---

<div align="center">

### 📜 Disclaimer

Flashing is done **at your own risk**. A bricked phone is your responsibility, not the maintainers'.
Nothing here voids anything you didn't already void by unlocking your bootloader. 😉

**Made for the OnePlus 12R / Ace 3 community.**
If this guide saved your device, drop a ⭐ on the repo.

</div>
