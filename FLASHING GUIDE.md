===== Instructions =====
October 27 at 03:06 PM
Note: firmware is included in the build
Clean flash:
1, Reboot to bootloader
2. Flash boot, init_boot, vendor_boot, dtbo and recovery images
$ fastboot flash boot boot.img
$ fastboot flash init_boot init_boot.img
$ fastboot flash vendor_boot vendor_boot.img
$ fastboot flash dtbo dtbo.img
$ fastboot flash recovery recovery.img
3. Reboot to recovery
4. Put the recovery in sideload mode and push the ROM zip
$ adb sideload Rom*.zip
5. Format data/Factory reset
6. Reboot to system
Updating to a newer build (dirty flash):
Flash it via recovery, by sideloading the ROM without wiping/formatting data
If sideloading the rom gives you error 7:
• Reboot to recovery
• Enter fastbootd
$ fastboot wipe-super super_empty.img
• Enter recovery
• Sideload the rom zip
• Format data
• Reboot
If after sideloading the rom, the device boots to bootloader:
• Do clean flash steps again
• When it asks for Yes/No prompt for installing additional packages, select Yes
• After device reboots recovery, sideload again.
• This time when it asks for Yes/No prompt for installing additional packages, select No
• Format and Reboot
