Windows Install Guide From USB
======

Installing Windows 10 or 11 from a USB drive is a straightforward process. Before you begin, make sure you have a USB drive with at least 8 GB of storage. Follow along to get the Windows 10/11 installation media file, create the bootable USB, install Windows, and configure her for operation.

## #Create a UEFI-bootable USB drive

> **ADVANCED ONLY!** See [Using Rufus to Make a USB Bootable](#using-rufus-to-make-a-usb-bootable) at the end for advanced instructions.

1. Download the [Windows 10](https://www.microsoft.com/en-us/software-download/windows10) or [Windows 11](https://www.microsoft.com/en-us/software-download/windows11) Media Creation Tool directly off of the Microsoft website.

2. Run the Media Creation Tool and accept the license terms.

3. Select the `Create installation media (USB flash drive, DVD, or ISO file) for another PC` option and click `Next`.

4. Choose the edition `Home/Pro`, and architecture [^1] `x86/x64`, then click `Next`.

5. Select `USB flash drive` and click `Next`.

6. Choose the correct USB drive from the list.

> **WARNING** *THIS ACTION WILL FORMAT ALL THE DATA ON THE SELECTED DRIVE, ENSURE YOU HAVE SELECTED THE CORRECT ONE AND HAVE THE DATA BACKED UP BEFORE CONTINUING.*

7. Click `Next`, and the tool will download and create a bootable Windows 10/11 USB drive.

## #Prepare your computer

1. Turn off your computer.

2. Ensure the USB drive is still attached.

3. Turn the computer on and tap repeatedly one of the following keys: `F12, F11, F10, F9, or Del` until you see the boot menu.

>Refer to your computer's manual for the specific key needed.

>This may take a few tries to get right.

4. In the boot menu, select your USB drive to be the first boot device and press `Enter`.

## #Install Windows 10/11

> **ADVANCED ONLY!** [Using Diskpart to Clean Drive](#using-diskpart-to-clean-drive) at the end for advanced instructions.

1. Wait for the Windows 10/11 setup screen to appear.

2. If asked, select the appropriate language, time/currency format, and input method, then click `Next`.

3. Click `Install now`.

4. If you know the license key or have the sticker, then enter it now. If not, click on `I don't have a product key` to continue with the installation.

>You can enter the key later, after installation.

5. Choose the Windows 10/11 edition you want to install and click `Next`.

6. Accept the license terms and click `Next`.

7. Choose the `Custom: Install Windows only (advanced)` installation type.

8. Select the drive where you want to install Windows 10/11.

>It's typically the drive with the most unallocated space.

> **WARNING** *THIS ACTION WILL FORMAT ALL THE DATA ON THE SELECTED DRIVE, ENSURE YOU HAVE SELECTED THE CORRECT ONE AND HAVE THE DATA BACKED UP BEFORE CONTINUING.*

9. Click `Next`.

10. The installation process will begin, and your computer may restart several times during the process. Be patient, as this can take some time.

## Complete the setup:

1. After the installation, Windows 10/11 will guide you through the setup process.

2. Customize your settings, connect to a Wi-Fi network, and create or sign in with a Microsoft account.

>Logging in with a Microsoft account is the best way to store a digital license key for Windows on your account.

4. After the setup has been completed and has had another reboot, you will be at the Windows 10/11 desktop.

## Install drivers and updates:

1. Connect to the internet, if you haven't already.

2. Use Windows Update to install any necessary drivers for your computer's hardware.

 >If the drivers for WiFi/Ethernet are not loading, you may have to visit the manufacturer's support website for the motherboard model and download any available drivers that are applicable to your Windows OS and architecture.

3. Check for Windows updates by going to `Settings > Update & Security > Windows Update` and clicking `Check for updates`. Install any available updates.

Your Windows 10/11 installation is now complete. You can remove the USB drive and start using your computer.

# Custom Install Procedures

## #Using Rufus to Make a USB Bootable

1. Download the [Windows 10](https://www.microsoft.com/en-us/software-download/windows10) or [Windows 11](https://www.microsoft.com/en-us/software-download/windows11) Media Creation Tool directly off of the Microsoft website.

2. Run the Media Creation Tool as administrator.

4. Select the `Create installation media (USB flash drive, DVD, or ISO file) for another PC` option and click `Next`.

4. Choose a language, edition `Windows 10/11`, and architecture [^1] `64-bit (x64)`, then click `Next`.

5. Select `ISO file` and click `Next`.

6. Choose a location to save the ISO.

7. Download [Rufus](https://rufus.ie/en/) then run as administrator.

8. For `Device` at the top, choose the USB drive you want to use as the bootable drive.

> **WARNING** *THIS ACTION WILL FORMAT ALL THE DATA ON THE SELECTED DRIVE, ENSURE YOU HAVE SELECTED THE CORRECT ONE AND HAVE THE DATA BACKED UP BEFORE CONTINUING.*

9. Click on the `SELECT` box to open the file manager and select the newly created ISO.

10. Ensure the partition scheme is `GPT` for UEFI systems or `MBR` for BIOS.

11. Click `Start` to continue. You may have popups that require acknowledgment.

12. Next up is to [prepare your computer](#prepare-your-computer).

## #Using Diskpart to Clean Drive

>You must have completed up to [Install Windows 10/11](#install-windows-10/11) to continue this section.

1. Wait for the Windows 10/11 setup screen to appear.

2. Press the `SHIFT` + `F10` keys; a command prompt window will open.

3. Type the following and press enter after each new line.

> **WARNING** *THIS ACTION WILL FORMAT ALL THE DATA ON THE SELECTED DRIVE, ENSURE YOU HAVE SELECTED THE CORRECT ONE AND HAVE THE DATA BACKED UP BEFORE CONTINUING.

```cmd
diskpart
list disk
```

> **I CANNOT STRESS ENOUGH TO MAKE SURE THAT DISK 0 IS THE CORRECT DISK TO ERASE; THIS IS IRRIVERSABLE.

```cmd
select disk 0
clean
convert gpt
create part efi size 260
format fs fat32 quick label System
create part msr size 16
create part pri
shrink minimum 500
format fs ntfs quick label Windows 
create part pri
format fs ntfs quick label Recovery
set id="de94bba4-06d1-4d40-a16a-bfd50179d6ac"
gpt attributes=0x8000000000000001
list volume
exit
```

4. You may close the command prompt window and continue the installation of [Install Windows 10/11](#install-windows-10/11).

[^1]: This only applies to Windows 10, as Windows 11 is x64 only.