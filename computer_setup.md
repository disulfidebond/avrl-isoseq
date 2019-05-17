## Overview
This writeup details the steps that were taken to setup a computer with the following specs to run Guppy GPU processing.

* Specs:
  * OS installed: Windows 10
  * RAM: 16 GB
  * CPU: Intel Xeon 
  * GPU: NVIDIA GeForce RTX 2080 Ti
  * Drives: 1TB with Windows installed, 2 TB empty (Ubuntu will be installed here).
  
## Preliminaries and Userful Tips
* Preliminaries:
  * Turn off Quickboot [as mentioned here](https://ubuntuforums.org/showthread.php?t=2091348&p=12397979#post12397979). Briefly, within Windows, go to Control Panel -> Power -> Power settings, then deselect fast boot.
  * Intel Rapid Response Technology did not appear to be enabled.
* Useful commands
  * To access the UEFI menu, tap the F2 key repeatedly immediately after the computer powers on.
    * Do *NOT* enable Legacy (BIOS) boot.
  * To access the One-Time boot menu and the GRUB loader for Linux, tap the F12 key repeatedly immediately after the computer powers on.
  * Information from Canonical about UEFI [can be found here](https://help.ubuntu.com/community/UEFI).
  
## Comments:
* I discovered that Windows was encrypted (this was not a surprise), and that Windows 10 had been installed on the NVME disk.
For our workflows, we want to use the NVME disk for read/write, so these steps will be a test runthrough. For the final installation, Windows 10 will be wiped, and only Ubuntu 16 will be installed.
* I followed the [steps described here](https://askubuntu.com/questions/726972/dual-boot-windows-10-and-linux-ubuntu-on-separate-hard-drives)
* You can view the partition table from the USB installer within Ubuntu with the command:

        lsblk -o NAME,FSTYPE,SIZE,MOUNTPOINT,LABEL

## Steps
1. Create the boot disk
Canonical has a series of [very helpful](https://tutorials.ubuntu.com/tutorial/tutorial-create-a-usb-stick-on-macos#6) tutorials on creating a bootable USB flash drive.
I followed a slightly different approach, and created one using Bash that [followed this guide](https://help.ubuntu.com/community/How%20to%20install%20Ubuntu%20on%20MacBook%20using%20USB%20Stick?_ga=2.230960155.542149009.1558114334-710168436.1558114334).
* The boot disk showed errors, and oddly showed 2 of the same volume in the One-Time Boot Menu. This appears to be a bug that will not affect the installation process.
2. Install Ubuntu 16
* Insert the USB drive, then reboot the computer and tap the F12 key repeatedly immediately after the computer powers on.
* You should see a screen that displays options for boot, starting with 'Windows'.  Select the UEFI Ubuntu option, and press Return
  * If the Ubuntu option is not preceeded by 'UEFI', stop and repeat Step 1.
* You should see the GRUB loader.  Select 'Install Ubuntu'
* Proceed through the steps until you are at the 'Installation Type' screen.
  * For the final installation, the steps will be simplified greatly (see 'Comments' above), since dual boot on 2 drives with UEFI will no longer need to be configured.
  * Select 'Something Else'
  * You will see the Windows installation as the nvme volume. The Ubuntu USB Installer *usually* is /dev/sdac, but verify this before proceeding!
  * Select the empty disk, which should be /dev/sda, formatted as NTFS
  * Create a new partition table for EFI /boot /root and /home 
  * Make sure to 
  * Select the /dev/sda partition, select /dev/sda as the EFI boot point, then select 'Install'. Verify that you are erasing the correct volume in the warning screen.
  
