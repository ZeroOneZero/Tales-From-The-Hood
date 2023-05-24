# Windows Install Guide From USB

Installing Windows 10 or 11 from a USB drive is a straightforward process. Before you begin, make sure you have a USB drive with at least 8 GB of storage. Follow along to get the Windows installation media file, create the bootable USB, install Windows, and configure your system for operation.

## Create a UEFI-bootable USB drive

> **ADVANCED ONLY**: See [Using Rufus to Make a USB Bootable](#using-rufus-to-make-a-usb-bootable) at the end for advanced instructions.

1. Download the [Windows 10](https://www.microsoft.com/en-us/software-download/windows10) or [Windows 11](https://www.microsoft.com/en-us/software-download/windows11) Media Creation Tool directly from the Microsoft website.
2. Run the Media Creation Tool.
3. Read and accept the license terms, then click `Next`.
4. Leave the `Use the recommended options for this PC` box checked, then click `Next`.
5. Select `USB flash drive` when asked to choose which media to use, then click `Next`.
6. Choose the correct USB drive from the list.

> **WARNING**: THIS ACTION WILL FORMAT ALL THE DATA ON THE SELECTED DRIVE. ENSURE YOU HAVE SELECTED THE CORRECT ONE AND HAVE THE DATA BACKED UP BEFORE CONTINUING.

7. Click `Next`, and the tool will download and create a bootable Windows USB drive.

## #Prepare your computer

1. Turn off your computer.
2. Ensure the installer USB and the drive you are going to install Windows on are installed in the PC.
3. Unplug all other HDDs, SSDs, or USB Drives from your PC to prevent accidentally formatting them.
4. To bring up the boot menu, press the appropriate function key immediately after turning the PC back on. This key varies by brand. Refer to the table below.

| Manufacturer | Boot Menu Hotkey(s) |
|--------------|---------------------|
| Acer         | F12, F9, F2, ESC    |
| ASUS         | F8, ESC             |
| Dell         | F12                 |
| HP           | F9, ESC             |
| Lenovo       | F12, Novo Button, F8, F10 |
| Sony         | F11, F12, ESC, F8   |
| Samsung      | ESC, F12, F2        |
| Toshiba      | F12                 |
| Apple        | Hold Option key     |
| Intel        | F10                 |
| MSI          | F11                 |
| Gigabyte     | F12                 |
| Fujitsu      | F12, F2             |

> Refer to your computer's manual for the specific key needed.

5. In the boot menu, select your USB drive and press `Enter`.

## #Install Windows

> **ADVANCED ONLY**: See [Using Diskpart to Clean Drive](#using-diskpart-to-clean-drive) at the end for advanced format instructions.

1. Wait for the Windows setup screen to appear.
2. If prompted, select the appropriate language, time/currency format, and input method, then click `Next`.
3. Click `Install now`.
4. If you have the license key or have the sticker, enter it now. If not, click on `I don't have a product key` to continue with the installation.

> You can enter the key later, after installation.

5. Choose the Windows edition you want to install and click `Next`.
6. Read and accept the license terms, then click `Next`.
7. Select the `Custom: Install Windows only (advanced)` option for the installation type.
8. Locate the target drive where you wish to install Windows.

> Generally, only two drives will be displayed: one is the USB from which you're installing Windows, and the other is your target disk. The target disk may show 2-3 partitions.
> If the target is a new drive or you've cleaned it using the diskpart command, it will be the one with the most unallocated space.

9. On the target drive, select each partition one by one and click `Delete`. Continue this process until only the USB drive and unallocated space on the target disk remain.

> **WARNING**: THIS ACTION WILL FORMAT ALL THE DATA ON THE SELECTED DRIVE. ENSURE YOU HAVE SELECTED THE CORRECT ONE AND HAVE THE DATA BACKED UP BEFORE CONTINUING.

10. Click `Next` and the installation process will begin. Your computer may restart several times during this process. Be patient, as it can take some time.


## Complete the Setup

1. After the installation, Windows will guide you through the setup process.
2. Customize your settings, connect to a Wi-Fi network, and create or sign in with a Microsoft account.

> Logging in with a Microsoft account is the best way to store a digital license key for Windows on your account.

3. After the setup has been completed and the system has rebooted, you will be at the Windows desktop.

## Install Drivers and Updates

1. Connect to the internet, if you haven't already.
2. Use Windows Update to install any necessary drivers for your computer's hardware.

> If the drivers for Wi-Fi/Ethernet are not loading, you may have to visit the manufacturer's support website for the motherboard model and download any available drivers that are applicable to your Windows OS and architecture.

3. Check for Windows updates by navigating to `Settings > Update & Security > Windows Update` and clicking `Check for updates`. Install any available updates.

> Your Windows installation is now complete. You can remove the USB drive and start using your computer.

# Custom Install Procedures

## Using Rufus to Make a USB Bootable

1. Download the [Windows 10](https://www.microsoft.com/en-us/software-download/windows10) or [Windows 11](https://www.microsoft.com/en-us/software-download/windows11) Media Creation Tool directly from the Microsoft website.
2. Run the Media Creation Tool as administrator and accept the license terms.
3. Choose `Use the recommended options for this computer` and then click `Next`.
4. Select `ISO file` and click `Next`.
5. Choose a location to save the ISO.
6. Download and run [Rufus](https://rufus.ie/en/).
7. For `Device` at the top, choose the USB drive you want to use as the bootable drive.

> **WARNING**: THIS ACTION WILL FORMAT ALL THE DATA ON THE SELECTED DRIVE. ENSURE YOU HAVE SELECTED THE CORRECT ONE AND HAVE THE DATA BACKED UP BEFORE CONTINUING.

8. Click on the `SELECT` button to open the file manager and select the newly created ISO.
9. Ensure the partition scheme is `GPT` for UEFI systems or `MBR` for BIOS systems.
10. Click `Start` to continue. You may have pop-ups that require acknowledgment.
11. The next step is to [prepare your computer](#prepare-your-computer).

## Using Diskpart to Clean Drive

> You must have completed up to but not started [Install Windows](#install-windows) to proceed with this section.

1. Wait for the Windows setup screen to appear.
2. Press the `SHIFT` + `F10` keys. A command prompt window will open.
3. Enter the following commands, pressing `Enter` after each line.

> **WARNING**: THIS ACTION WILL FORMAT ALL THE DATA ON THE SELECTED DRIVE. ENSURE YOU HAVE SELECTED THE CORRECT ONE AND HAVE THE DATA BACKED UP BEFORE CONTINUING.

```cmd
diskpart
list disk

> **IMPORTANT**: MAKE SURE THAT DISK 0 IS THE CORRECT DISK TO CLEAN. THIS ACTION IS IRREVERSIBLE.

```cmd
select disk 0
clean
convert gpt
create part efi size=260
format fs=fat32 quick label=System
create part msr size=16
create part pri
shrink minimum=500
format fs=ntfs quick label=Windows 
create part pri
format fs=ntfs quick label=Recovery
set id="de94bba4-06d1-4d40-a16a-bfd50179d6ac"
gpt attributes=0x8000000000000001
list volume
exit

4. You may close the command prompt window and continue the installation at [<kbd>Install Windows</kbd>](#install-windows).
