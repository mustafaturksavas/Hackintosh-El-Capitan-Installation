# Hackintosh El Capitan Installation

This is a guide for myself, but if you have the same build or if you know what you are doing, this guide might also be helpful to you.

## My Build
- GA-Z77-DS3H
- i7 2600K
- GeForce GTX 660

## Installation Steps

- [Create Installation Disk](#disk)
- [Backup](#backup)
- [Change BIOS Settings](#bios)
- [Install El Capitan](#install)
- [Install Bootloader & Drivers](#bootloader)
- [More](#more)
- [Notes](#notes)


### <a name="disk"></a>Create Installation Disk

- Download El Capitan from Mac App Store
- Insert USB drive
- Open Disk Utility
  - Select the USB drive
  - Select Erase
    - Name: USB
    - Format: OS X Extended (Journaled)
    - Scheme: GUID Partition Map
    - Erase
- Run [UniBeast](http://www.tonymacx86.com/downloads.php?do=cat&id=3)
- Select **USB** in Destination
- Select **El Capitan** in Installation Type
- Select **UEFI Boot Mode** in Bootloader Options
- Let **UniBeast** finish the process
- Add necessary files to USB drive
  - [Clover Configurator](http://mackie100projects.altervista.org/download/)
  - [audio_CloverALC](https://github.com/toleda/audio_cloverALC)
  - [Multibeast 8.x](http://www.tonymacx86.com/downloads.php?do=cat&id=3)
  - [Multibeast 5.4.3](http://www.tonymacx86.com/downloads.php?do=cat&id=6)

### <a name="backup"></a>Backup

Backup all essential files and settings

- Users/user/.ssh
- Users/user/Library/Application Support/Karabiner
- Users/user/Library/Group Containers/UBF8T346G9.Office
- Library/Desktop Pictures

- Users/user/Desktop
- Users/user/Downloads
- Users/user/Projects
- Users/user/Dropbox

### <a name="bios"></a>Change BIOS Settings

- Set SATA Mode to AHCI from **Peripherals**

### <a name="install"></a>Install El Capitan

- Press Fn + F12 when you see the logo
  - Select USB drive from the boot list
- Select USB drive from Clover Bootloader
- To install it cleanly, previous installation must be removed
  - Open Disk Utility
  - Choose Macintosh HD
  - Select Erase
    - Name: Macintosh HD
    - Format: OS X Extended (Journaled)
    - Scheme: GUID Partition Map
    - Erase
  - Close Disk Utility
- Install El Capitan
- Boot the computer from USB drive and select **Macintosh HD**

### <a name="bootloader"></a>Install Bootloader & Drivers

1. Open MultiBeast 8.x
  - Select Quick Start > UEFI Boot Mode
  - Select Drivers > Network > Atheros > AtherosE2200Ethernet
  - Build

2. Open MultiBeast 5.4.3
  - Select Drivers & Bootloaders > Drivers > Audio > Realtek ALC8xx > Without DSDT > ALC887/ALC888b > 100302 Current

3. Remove USB drive (so the system doesn't start from USB) and restart the system from Macintosh HD

4. Install [audio_CloverALC](https://github.com/toleda/audio_cloverALC)
  - Mount EFI using Clover Configurator
  - Run audio_cloverALC-110.command
  - Select y for everything

5. Check if the sound input and output are working

6. Check if Clover config is the same as the backup
  - Everything must be the same as the backup config, except for **Kernel and Kext Patches**
  - Only get **ssdTrimEnabler** from **Kernel and Kext Patches**
  - Don't forget to install the theme if using a custom one

### <a name="more"></a>More

- Prevent Backup and Windows disks from mounting
  - Open Terminal
    - List disks using `diskutil list`
    - Get UUID of the disks to be prevented from mounting with `diskutil info diskxsx`
    - Create fstab to list the disks `sudo pico /etc/fstab`
    - Add lines for the disks `UUID=XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX  none hfs rw,noauto 0 0` or `UUID=XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX  none ntfs rw,noauto 0 0` for ntfs drives
    - Press CTRL + X and save

- Restore backed up files

- If everything is ok after a couple restarts, backup installation using [SuperDuper!](http://www.shirt-pocket.com/SuperDuper/SuperDuperDescription.html)

- Install essential Apps
  - [Chrome](https://www.google.com/chrome/browser/desktop/)
  - [Dropbox](https://www.dropbox.com/install)
  - [1password](https://agilebits.com/onepassword/mac)
  - [Skype](http://www.skype.com/en/download-skype/skype-for-mac/)
  - [Sync](https://www.sync.com/install/)
  - [Brackets](http://brackets.io/)
  - [SourceTree](https://www.sourcetreeapp.com/)
  - [ImageOptim](https://imageoptim.com/)
  
### <a name="notes"></a>Notes

- XtraFinder's "System Integrity Protection is enabled" doesn't have a solution. Do not change **CsrActiveConfig** to **0x14**, because after a while it gives "Missing Bluetooth Controller Transport" error and you can't fix it unless you change it back from clover settings (It happened after many restarts). _Setting **CsrActiveConfig** to **0x7** seems to work for now._