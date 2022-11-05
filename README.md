# **Arch Linux Installation Documentation**

In this tutorial, I will walkthrough on the steps that I did on installing Arch Linux including problems that I went through with the solutions that I was able to solve in proceeding. The document is composed of five sections into installing Arch Linux while also adding modifications after installing Arch Linux.

## **Downloading Arch Linux File from ArchLinux Wiki**

The first thing we need to do is to install the Arch Linux File. Without this, no point in looking at this documentation lol. In this section, I did not had any problem going through the process of downloading the file since Codi provided a video for the section.

Source: https://archlinux.org/download/

1. Go to the ArchLinux Wiki that is provided in the assignment and click the Download button.
2. Scroll all the way down till you see the United States section and download any of the one (Downloaded mit.edu since that was what Professor West used)
3. Click archlinux-2022.10.01-x86 64.iso and wait till it finish downloading
4. Make sure the checksums are correct just to make sure no one is installing a virus.

## **Creating the ArchLinux Virtual Machine**

For this section, we need to create the Virtual Machine so we can do Arch Linux so we can do all the modifications and download packages. Again, did not had a problem in this section since I followed Professor West walkthrough.

1. Since we are installing it into a virtual machine, download VMWare Workstation
2. In the VMWare Workstation, Create a New Virtual Machine
3. Click advanced button
4. For the “Choose the Virtual Machine Hardware Compatibility”, make sure the Hardware compatibility is up to date
5. For “Installer disc image file”, put the Arch Linux file that you downloaded.
6. In “Select a Guest Operating System”, select “Other Linux 5.x kernel 64-bit” in the Version section
7. Name the Virtual Machine in the next section
8. In “Processor Configuration”, set the Number of processors to 1 and Number Of Cores Per Processor to 2.
9. For “Memory for the Virtual Machine”, set the storage to 2 GB.
10. For the last few sections, leave everything to default.
11. Right click the Arch Linux in the Library section and click “Open VM Directory”
12. After doing that right click the Arch Linux.vmx and edit it by using notepad. Under “encoding = “windows-1252”, add firmware=“efi” and save it.

## **Installing the ArchLinx Modifications**

Now you have Arch Linux up and running, its time to install the modifications to start, we need to enter into Arch Linux.

1. Click Enter on `Arch Linux install medium (x86_64, UEFI)

### 1.1 Connect to the Internet

1. To make sure you have internet connection, type `ip link`

### 1.2 Update Clock and the Language

1. type `timedatectl status` to update the clock for the Arch Linux
2. type `loadkeys us` to set the keyboard to US keyboard. 

### 1.3 Partition the disks

1. Manually partition the virtual hard drives by first modifying the partition tables by typing `fdisk /dev/sda`
2. Once doing that, type `n`. We do this to add a new partition. Type `p` for primary. For partition number, leave it to default. For First Sector, set it to 2048. Finally, for the Last Sector, put +500M. You completed adding the first partition
3. For the second partition, leave everything to default value
   (Note: We are ignoring the swap)

### 1.4 Format The Partition

1. For the first sda1, we are formatting this as a FAT32 file since the size is at least 300 MiB and under 512 MiB. Type `mkfs.fat -F 32 /dev/sda1`
2. For the second sda2, we are formatting this to Ext4 file since the file is over 500 MiB. Type `mkfs.ext4 /dev/sda2`
3. Type `lsblk -f` to check if it is formatted correctly. 

   (Note: I accidentally set this a a Ext4 file and was able to fix it by overriding it to the Ext4 file by calling the command)

### 1.5 Mount the file systems

1.  For sda1, given that the file is a FAT32, mount the file by typing `mount --mkdir /dev/sda1 /mnt/boot`
2.  For sda2, type `mount /dev/sda2 /mnt`, given that it is a Ext4 file.
3.  Just like the last section, type `lsblk -f` to make sure both sda1 and sda2 are mounted correctly

(Note: One problem that I occured was accidentally mounting the wrong sda file. One solution that I did to fix it by correcting the mount and overriding the original mount that I accidentally commit it to)

![Screenshot 2022-11-05 020242](https://user-images.githubusercontent.com/87620828/200138345-11954fee-17dc-4d68-bff3-40c294328d7e.jpg)

### 2.1 Installation Packages

1. To install the packages that we would need for the last steps, type `pacstrap -K /mnt base linux linux-firmware nano grub dhcpcd efibootmgr`. This would install the nano commmand, grub command, dhcpcd command, and the efibootmgr command.

   (Note: I did had trouble when I installed the package that the Arch Linux Installation guide provided but it only installed the basic packages and made it difficult when trying to edit the Localization since it did not provided nano. One solution was to find the installation to install the packages that I would need to help me install Arch Linux)

### 3.1 Configuring The System

For this next step, we need to configure the system so we can install our bootloader and set the root password. 

1. First generate an fstab file, do this by typing `genfstab -U /mnt >> /mnt/etc/fstab` 
2. After that, we need to change root into a new system. We can do this by typing `arch-chroot /mnt`
3. Next, we need to set the timezone. We first type `ln -sf /usr/share/zoneinfo/America/Chicago /etc/localtime` (that the case if you are attending University of Tulsa)
4. For the last step for this section, we would type `hwclock --systohc`. This would make the command assume that the hardware clock is set to UTC

(Note: Did not had any problem in this step since this was straightfoward in the ArchLinux Wiki but the only problem I would say is that it does not provide the Region and city to when trying to set the timezone)

### 3.2 Localization

For this section, this was difficult when using the basic installation and that it didn't describe that we needed to use nano, making it difficult for people new to Linux. I had to redownload packages given that I only downloaded the base linux packages.

1. To start, type nano /etc/locale.gen and uncomment en_US.UTF-8 UTF-8 by erasing the #. ctrl+x and save it by clicking the ctrl+x, y, and then ENTER key
2. Generate the locales by typing `locale-gen`. It should display US.UTF-8 when calling that command.
3. After that, type `nano /etc/locale.conf` and type `LANG=en_US.UTF-8`. ctrl+x and save by clicking the ENTER key twice

### 3.3 Network Configuration

1. For this section, we need to create the hostname file, start by creating the hostname by typing `touch /etc/hostname`
2. After that we need to edit the host name. Type `nano /etc/hostname`. In there, type the hostname of your choice. An example would be `tylertran`
3. Save it by ctrl+x, y, and ENTER key.
4. Next we need to edit the host files in NANO editor. Type `nano /etc/hosts`.
5. Type the following `127.0.0.1 localhost ::1 localhost 127.0.1.1 username`. (ex: username = tylertran) 

(Note: Out of all the steps, the steps since it did not give the specific step on what we needed to type.)

### 3.4 Initramfs

1. While this section is not required, its better to have this just in case since the Arch Linux Wiki provided it as one of the steps, given that mkinitcpio was run on installation of the kernel package with pacstrap. For this section, we need to recreate the initramfs image by typing `mkinitcpio -P`

(Note: Was not difficult since it exactly what was said on the Arch Linux Wiki)

### 3.5 Bootloader

1. For this section, we need to download some packages to download the bootloader. Type `pacman -S man-pages texinfo sudo man-db sof-firmware dosfstools amd-ucode`

Problem: For some reason when I tried installing the packages, it kept saying that the packages were corrupted. If you run into this problem do these steps because it could be a arch linux key problem. 

- `pacman-key --init`
- `pacman-key --populate archlinux`

Source: https://www.reddit.com/r/archlinux/comments/7wrwns/pacman_always_return_package_is_corrupted/

(Note: What this does is that we are removing the arch-linux keys and repopulate it since some of the keys are corrupted)

### Updating the Packages

1. After finish downloading the packages, we need to update the packages before installing the NetworkManager. Do these following step-by-step
   - `sudo pacman -Syu`
   - `sudo pacman -S wpa_supplicant wireless_tools networkmanager`
   - `sudo pacman -S nm-connection-editor network-manager-applet`
   - `sudo systemctl enable NetworkManager.service`
   - `sudo systemctl enable wpa_supplicant.service`

### 3.6 Changing the password

1. The second to last step, we need to change the password of the root so we can login into ArchLinux. Type `passwd` and enter the password you want it. Retype it to verify that is the password that you wanted.

### 3.7 Downloading the bootloader

1. For the final step into installing the ArchLinux, we need to install the bootloader. First do this by typing `grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB`
2. Next, type `grub-mkconfig -o /boot/grub/grub.cfg` to update the processor microcodes
3. Second to last, type `mount /dev/sda1 /mnt` to mount the boot disk as the mounted partition
4. Lastly, type `grub-install --efi-directory=/dev/sda1` to install the EFI directory to the boot disk via GRUB.
5. Restart your Virtual Machine my typing `reboot`. Once doing that, you should be having a new screen that displays GUB on the top. This means that you installed your Arch Linux correctly.

   (Note: One of the Virutual Machine, I never noticed that I accidentally installed de-latin1 keyboard until entering my login on the Arch Linux. A solution that I did was after signing into the Arch Linux. I typed `loadkeys us`. This enable me to use the US keyboard.)

   (Problem: The only problmem I had was that I did not was able to know how to install the grub until looking into Arch Linux Wiki)

## **Arch Linux VM Modifications**

### 4.1 Adding and granting sudo to the users

For this requirements, we need to makes some modifications to our Arch Linux. To start we need to add some users. We need to create two users. One for the user and one for codi

1. Type `sudo useradd -m tyler`. Repeat for codi
2. Type `sudo passwd tyler` and enter the password to your choosing
3. Type `sudo passwd codi` and set the password as GraceHopper1906
   Once we do that, we need to add sudo permission for both the user and codi. We can do that by creating a group.
4. Type `sudo groupadd sudo`
5. Type `sudo usermod -a -G sudo codi` to add the user codi to the group `sudo`. Repeat the same step for user
6. Just in case if you wanted to make sure that all two users exist, type `less /etc/passwd` to list the passwd files of each users.

![Screenshot 2022-11-05 022019](https://user-images.githubusercontent.com/87620828/200138711-91f49999-dba4-4af7-bb3e-88a6ec1a65ed.jpg)

(Note: Did not run into any problem into this section at all)

### Checking for update

While this is not necessary when booting up everytime, it is highly recommended to do so if you are logging in for the first type.

1. Type `pacman -Syu`

### Installing The Boot Enviroment

1. For this step, we need to install a Desktop Enviroment to the function. To start, type `pacman -S xorg`. Click ENTER key to install all the packages
2. After waiting and finish downloading, type `pacman -S xorg-server`. This will install the display server package.

(Note: If installation was not active, type `ping google.com` to see if it works. If not type `sudo systemctl disable dhcpcd.service` and `sudo systemctl start NetworkManager.service`

### Installing KDE

For this step, you will need to download a compatible DE of your choice. Given from Codi that KDE was the best, we will use this. You will need to download the KDE Plasma Desktop Enviroment from a website.

Source: https://kubuntu.org/

1. After downloading it, type `pacman-S plasma plasma-meta` to install the KDE Plasma package (For everything they ask, click ENTER to install everything)
2. Type `pacman -S xf86-video-intel` afterward. Only for Intel. 
3. Once doing that, type `pacman -S plasma-wayland-session kde-applications` to isntall the Wayland small display server protocol for our KDE Plasma that we installed in the last step.
4. Finally, type `systemctl enable sddm.service` to enable the simple desktop display for our KDE Plasma DE
5. Reboot your system by typing `reboot` 

(Problem: I ran into a problem where it did not boot the screen when I reboot it. It was stuck in the start screen and never boot. Had reinstall a new virtual machine) 

![Screenshot 2022-11-05 024458](https://user-images.githubusercontent.com/87620828/200138323-220932ca-c2d1-4780-85df-20bc4a7b6a6c.jpg)

### Changing the Shell

![Screenshot 2022-11-05 025714](https://user-images.githubusercontent.com/87620828/200138723-61f1f962-37bc-494c-8c5f-6146b02bc4aa.jpg)

1. Type `sudo pacman -S zsh`. This would install the zsh shell package given that we wanted to install a different shell not was not bash. Since we do not have root access, type `su root` and type the password you set for root access.
2. Type `zsh` and foolow the following step to make it correct
   - Type `1`: Continue to the the main menu
   - Type `2`: Configure the auto-complete system
   - Type `1`: Set to the default option
   - Type `0`: to exit and save the configuration 
   - Type `exit`

### Customizing zsh

![Screenshot 2022-11-05 031003](https://user-images.githubusercontent.com/87620828/200138738-34608db8-2685-4638-b9dc-5f13b5ffb8c1.jpg)

1. Type `sudo pacman -S git` to install the Git
2. After the installation of the Git, type `sh -c `$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)` to install the zsh
3. After that, type `touch ~/.zshrc` to create the zsh config file
4. Once creating that, edit it by typing `nano ~/.zshrc` and change the theme where it says `ZSH_THEME=robbyrussell` to `ZSH_THEME=awesomepanda`.
5. Finally, to change the color of certain characters, type `source ~/.zshrc to apply the changes.

### Installing the SSH

1. Type `pacman -S openssh` to install openssh 
2. Check the status by typing `systemctl status sshd`. If it is not active, type `systemctl start sshd`
3. After installing the putty, type `ssh -p53997 sysadmin@129.244.245.111`

### :Color coding and Creating the aliases

![Screenshot 2022-11-05 031954](https://user-images.githubusercontent.com/87620828/200138744-bb7baf2d-f66f-454c-a038-03317aa1b5fe.jpg)

For the final step, we need to add some color to our konsole. To start, let first add some color to our code. 

1. Call the coomand `nano ~/.zshrc` to edit the zshrc file since we are running zsh 
2. Type `PS1="%{$fg[blue]%}%n%{$reset_color%}@%{$fg[green]%}%m %{$fg[yellow]%}%~ %{$reset_color%}%% "`
3. Next, we want to add some alias to our code to make it faster to code in linux. Below the code that we typed in the last step, add these steps (or anything else of your choice)
   - `alias clr='clear'`: a quicker way to call clear 
   - `alias rb='sudo reboot'`: a quicker way to call reboot 
   - `alias gh='history | grep'`: a way to check the history of grep being used
   - `alias c='clear'`: another quick way to call clear.
5. TCTRL+X, Y, and ENTER to save your changes 
