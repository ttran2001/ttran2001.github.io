# Arch Linux Installation Documentation 

## Downloading Arch Linux File from ArchLinux Wiki 
1.	Go to the ArchLinux Wiki that is provided in the assignment and click the Download button. 
2.	Scroll all the way down till you see the United States section and download any of the one (Downloaded mit.edu since that was what Professor West used) 
3.	Click archlinux-2022.10.01-x86 64.iso and wait till it finish downloading
4.	Make sure the checksums are correct just to make sure no one is installing a virus. 
## Creating the ArchLinux Virtual Machine
1.	Since we are installing it into a virtual machine, download VMWare Workstation 
2.	In the VMWare Workstation, Create a New Virtual Machine 
3.	Click advanced button
4.	For the “Choose the Virtual Machine Hardware Compatibility”, make sure the Hardware compatibility is up to date
5.	For “Installer disc image file”, put the Arch Linux file that you downloaded.
6.	In “Select a Guest Operating System”, select “Other Linux 5.x kernel 64-bit” in the Version section 
7.	Name the Virtual Machine in the next section 
8.	In “Processor Configuration”, set the Number of processors to 1 and Number Of Cores Per Processor to 2. 
9.	For “Memory for the Virtual Machine”, set the storage to 2 GB. 
10.	For the last few sections, leave everything to default.  
11.	Right click the Arch Linux in the Library section and click “Open VM Directory”
12.	After doing that right click the Arch Linux.vmx and edit it by using notepad. Under “encoding = “windows-1252”, add firmware=“efi” and save it.
## Installing the ArchLinx Modifications 
1.	Click Enter on "Arch Linux install medium (x86_64, UEFI) 
### 1.1 Connect to the Internet
1.	To make sure you have internet connection, type "ip link" 
### 1.2 Update Clock 
1. type "timedatectl status" to update the clock for the Arch Linux
### 1.3 Partition the disks 
1. Manually partition the virtual hard drives by first modifying the partition tables by typing "fdisk /dev/sda"
2. Once doing that, type "n". We do this to add a new partition. Type "p" for primary. For partition  number, leave it to default. For First Sector, set it to 2048. Finally, for the Last Sector, put +500M. You completed adding the first partition
3. For the second partition, leave everything to default value 
(Note: We are ignoring the swap)
### 1.4 Format The Partition 
1. For the first sda1, we are formatting this as a FAT32 file since the size is at least 300 MiB and under 512 MiB. Type "mkfs.fat -F 32 /dev/sda1"
2. For the second sda2, we are formatting this to Ext4 file since the file is over 500 MiB. Type "mkfs.ext4 /dev/sda2
(Note: I accidentally set this a a Ext4 file and was able to fix it by overriding it to the Ext4 file by calling the command)
### 1.5 Mount the file systems
1.  For sda1, given that the file is a FAT32, mount the file by typing "mount --mkdir /dev/sda1 /mnt/boot"
2.  For sda2, type "mount /dev/sda2 /mnt", given that it is a Ext4 file. 
### 2.1 Installation Packages
1. To install the packages that we would need for the last steps, type "pacstrap -K /mnt base linux linux-firmware nano grub dhcpcd efibootmgr. This would install the nano commmand,  grub command, dhcpcd command, and the efibootmgr command. 
(Note: I did had trouble when I installed the package that the Arch Linux Installation guide provided but it only installed the basic packages and made it difficult when trying to edit the Localization) 
### 3.1 Configuring The System 
1. For this section we must first generate an fstab file, do this by typing "genfstab -U /mnt >> /mnt/etc/fstab" 
2. After that, we need to change root into a new system. We can do this by typing "arch-chroot /mnt" 
3. Next, we need to set the timezone. We first type ln -sf /usr/share/zoneinfo/America/Chicago /etc/localtime 
4. For the last step for this section, we would type "hwclock --systohc". This would make the command assume that the hardware clock is set to UTC
### 3.2 Localization 
For this section, this was difficult when using the basic installation and that it didn't describe that we needed to use nano, making it difficult for people new to Linux. 
1. To start, type nano /etc/locale.gen and uncomment en_US.UTF-8 UTF-8 by erasing the #. ctrl+x and save it by clicking the ENTER key twice
2. Generate the locales by typing "locale-gen" 
3. After that, type "nano /etc/locale.conf" and type "LANG=en_US.UTF-8". ctrl+x and save by clicking the ENTER key twice 
### 3.3 Network Configuration 
1. For this section, we need to create the hostname file, start by typing "nano /etc/hostname". In there, type the hostname of your choice. An example would be "tylertran". Save it by ctrl+x and ENTER key twice 
### 3.4 Initramfs 
1. While this section is not required, its better to have this just in case, given that mkinitcpio was run on installation of the kernel package with pacstrap. For this section, we need to recreate the initramfs image by typing "mkinitcpio -P" 
### 3.5 Password
1. We need to set a password for the root. We can do this by typing "passwd". This allows us to create a password for our root.  
### 3.6 Bootloader 

