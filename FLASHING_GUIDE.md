# Flashing a custom ROM — OnePlus 12R (aston) & Ace 3 (astonc)

A short, beginner-safe guide. Two methods: an automated Windows tool (recommended) and manual fastboot.

**Time:** 20–30 minutes · **Difficulty:** easy · **Wipes all data:** yes

---

## Contents

1. [Before you flash](#before-you-flash)
2. [Method 1 — Firmware Flasher (recommended)](#method-1--firmware-flasher-recommended)
3. [Method 2 — Manual fastboot](#method-2--manual-fastboot)
4. [Troubleshooting](#troubleshooting)
5. [FAQ](#faq)
6. [Command cheat sheet](#command-cheat-sheet)

---

## Before you flash

> [!WARNING]
> **Read this first.**
>
> - Flashing **erases everything** — photos, chats, files, app data. Back up before you start.
> - `aston` and `astonc` share hardware but **not** firmware or partition layout. Flashing the wrong one can hard-brick the phone.
> - The **bootloader must be unlocked**. Unlocking also wipes data.
> - You flash at your own risk.

### 1. Check your codename

Turn on **USB debugging**, connect the phone, and run:

```bash
adb shell getprop ro.product.device
```

| Output | Device | Model numbers |
| :-- | :-- | :-- |
| `aston` | OnePlus 12R | CPH2585 (India), CPH2609 (Global/EU), CPH2611 (North America) |
| `astonc` | OnePlus Ace 3 | PJE110 (China) |

The ROM filename must contain your codename: `...-aston-...zip` is for the 12R only, `...-astonc-...zip` is for the Ace 3 only.

### 2. Requirements

- [ ] Unlocked bootloader
- [ ] ROM zip matching your codename
- [ ] Windows PC (Method 1) or Windows / macOS / Linux (Method 2)
- [ ] [Android platform-tools](https://developer.android.com/tools/releases/platform-tools) installed
- [ ] OnePlus USB drivers (Windows)
- [ ] A **data** USB cable — not a charge-only one
- [ ] Battery above 50%
- [ ] Full backup

### 3. Button combos

| Mode | Phone off — hold | From PC |
| :-- | :-- | :-- |
| Fastboot | Power + Volume Up + Volume Down | `adb reboot bootloader` |
| Recovery | Power + Volume Down until the logo appears | `adb reboot recovery` |
| FastbootD | — | `fastboot reboot fastboot` (from fastboot) |

---

## Method 1 — Firmware Flasher (recommended)

One Windows tool unpacks the ROM and flashes everything, including firmware. **No commands to type, no slot handling.**

At a glance: **unpack → flash → format data → boot.**

> [!TIP]
> Use **Fastboot Firmware Flasher by NeFeroN, v1.0.2.0 or newer**. The unpacker is a tab inside the same app — you do not need a second tool.

### Step 1 — Download

| File | Source | Notes |
| :-- | :-- | :-- |
| ROM zip | The ROM's official download page or support group | Must contain your codename |
| Firmware Flasher | NeFeroN's release page or the ROM's support group | v1.0.2.0 or newer |

> [!NOTE]
> The ROM zip must be a full package (it contains `payload.bin`). Current builds for these devices are. If there is no `payload.bin`, use Method 2.

### Step 2 — Prepare the PC

1. Extract the tool to a short path with no spaces or non-English characters, for example `C:\Flasher\`.
2. Right-click the `.exe` → **Run as administrator**.
3. If Windows blocks the download: right-click the ZIP → **Properties → Unblock → Apply**, then extract.
4. Antivirus may flag the `.exe`. It is an unsigned community tool, so this is usually a false positive — only continue if you trust the source.

### Step 3 — Unpack the ROM

1. Open the tool → **Firmware Unpacker** tab.
2. **File Picker** → select the ROM zip.
3. Set **Mode** to **Full**.
4. Click **Unpack** and wait for **Unpacking Completed**. Don't close the window while it runs.

### Step 4 — Flash

1. Go to the **Firmware Flasher** tab.
2. Click **START FLASHING**.
3. When it offers to reboot into **FastbootD**, click **OK**.
4. Don't touch the phone, the cable, or the PC until it finishes.
5. Wait for **Flashing completed**, then click **Reboot Device**.

### Step 5 — Format data

Skipping this bootloops the new ROM, because the old encryption key is still on the phone.

1. Reboot to **Recovery**.
2. **Factory Reset → Format Data** → confirm with `yes`.
3. Reboot to system.

> [!TIP]
> First boot can take 5–10 minutes while apps optimize. Do not force-restart. Once the setup screen appears, you're done.

---

## Method 2 — Manual fastboot

Flash the boot images yourself, then sideload the ROM in recovery. Use this if Method 1 fails or you are not on Windows.

> [!IMPORTANT]
> **Both prerequisites are mandatory:**
>
> 1. The phone already runs **OxygenOS or ColorOS 16.0.1.300 or newer**.
> 2. **Both slots have the same firmware.** If unsure, flash the full stock build once with Method 1, then come back.

Check the current build:

```bash
adb shell getprop ro.build.version.ota
```

### Step 1 — Get the images

Unpack the ROM and copy these six images into your platform-tools folder:

`boot.img` · `dtbo.img` · `init_boot.img` · `vendor_boot.img` · `vbmeta.img` · `recovery.img`

> [!TIP]
> If the images are still inside the zip, extract `payload.bin` with `payload-dumper-go payload.bin`, or use the Firmware Unpacker from Method 1.

### Step 2 — Enter fastboot

```bash
adb devices
adb reboot bootloader
fastboot devices
```

`fastboot devices` must list your serial number. If it prints nothing, fix the driver or cable before continuing — see [Troubleshooting](#troubleshooting).

### Step 3 — Flash the images

```bash
fastboot flash boot boot.img
fastboot flash dtbo dtbo.img
fastboot flash init_boot init_boot.img
fastboot flash vendor_boot vendor_boot.img
fastboot flash vbmeta vbmeta.img
fastboot flash recovery recovery.img
```

No `--slot` arguments are needed; fastboot writes to the active slot. If any command fails, stop and fix it — don't reboot and hope.

### Step 4 — Sideload the ROM

```bash
fastboot reboot recovery
```

In recovery on the phone: **Factory Reset → Format Data**, then **Apply update → Apply from ADB**. Then on the PC:

```bash
adb sideload rom.zip
```

> [!TIP]
> The phone's progress bar often stops at **47%** — that's normal. Watch the PC: it ends with `Total xfer: 1.00x`. If sideload fails immediately, update platform-tools.

### Step 5 — Finish

1. **Factory Reset → Format Data** again.
2. **Reboot to system.**

> [!WARNING]
> Never sideload a ROM for the other codename, and never relock the bootloader while a custom ROM is installed.

---

## Troubleshooting

| Problem | Fix |
| :-- | :-- |
| `waiting for any device` | Reinstall the OnePlus USB driver, try another cable and port (USB 2.0 is more reliable), run the terminal as administrator, re-authorize USB debugging |
| `fastboot devices` is empty | The phone isn't in fastboot or the driver isn't bound. Use the button combo table above and check Device Manager |
| `Flashing is not allowed in Lock State` | The bootloader is locked — unlock it first |
| Unpacker says `payload.bin` not found | Not a full ROM package. Use Method 2 or re-download the ROM |
| Unpacking fails midway | Bad download or low disk space. Re-download and keep 15 GB free |
| Tool crashes on launch | Path has spaces or non-English characters. Move it to `C:\Flasher\` and unblock the ZIP |
| Antivirus deletes the `.exe` | False positive on an unsigned tool. Restore it only if you trust the source |
| `adb: sideload connection failed` | Update platform-tools, change the USB port, restart sideload from recovery |
| Sideload appears stuck at 47% | It isn't stuck — the PC terminal is the source of truth |
| Bootloop after flashing | Boot to recovery → **Format Data** → reboot |
| Stuck in FastbootD | `fastboot reboot` or `fastboot reboot recovery`, then retry |
| Nothing on screen at all | Keep it charged and seek a service centre with MSM/EDL access. Modern OnePlus devices can't do this at home |

---

## FAQ

**Do I need root or a custom recovery?**
No. Both methods work on a stock, bootloader-unlocked device.

**Will I lose my data?**
Yes. Both methods wipe internal storage.

**Do the ROMs include firmware?**
Current builds do, which is why Method 1 works from any OxygenOS/ColorOS version. Method 2 still needs 16.0.1.300 or newer on both slots.

**Can I flash an `aston` ROM on an `astonc` phone?**
No. Same hardware, different firmware and partition layout.

**Does flashing relock the bootloader?**
No — and don't relock it manually while a custom ROM is installed.

**Will banking and UPI apps work?**
It depends on how the ROM handles Play Integrity. Check the ROM's support group before flashing.

**How do I go back to stock?**
Flash the official full OTA zip the same way as Method 1, then format data. Downgrades may be restricted, so check the firmware notes.

---

## Command cheat sheet

```bash
adb devices                           # is the phone connected?
adb shell getprop ro.product.device   # confirm aston / astonc
adb reboot bootloader                 # reboot to fastboot
fastboot devices                      # is fastboot connected?
fastboot reboot                       # leave fastboot
fastboot reboot recovery              # reboot to recovery
adb sideload rom.zip                  # push the ROM from recovery
```

**Five rules**

1. Match the ROM to your codename.
2. Read prompts before clicking OK.
3. Never unplug while flashing.
4. Format data after flashing, every time.
5. When in doubt, use Method 1 again.

---

## Credits

- **Fastboot Firmware Flasher** — by NeFeroN.
- **ROM maintainers** for `aston` and `astonc` — support them in their threads and groups.
- [Android platform-tools](https://developer.android.com/tools/releases/platform-tools)
- [PixelOS for OnePlus 12R / Ace 3 — XDA](https://xdaforums.com/t/rom-17-official-pixelos-for-oneplus-12r-ace3-aosp-01-10-26.4662225/)
- [Device tree — OnePlus Ace 3 (`astonc`)](https://github.com/EXlipsecore/android_device_oneplus_astonc)
- [Recovery device tree — 12R / Ace 3 (`aston`)](https://github.com/EXlipsecore/android_device_oneplus_aston-AERA)

---

*Flashing is at your own risk. Corrupted downloads, wrong codenames and skipped format-data steps cause most failures — read the guide once before you start.*
