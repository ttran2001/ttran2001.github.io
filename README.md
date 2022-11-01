# ttran2001.github.io

Arch Linux Walkthrough
Downloading Arch Linux File from ArchLinux Wiki 
1.	Go to the ArchLinux Wiki that is provided in the assignment and click the Download button. 
2.	Scroll all the way down till you see the United States section and download any of the one (Downloaded mit.edu since that was what Professor West used) 
3.	Click archlinux-2022.10.01-x86 64.iso and wait till it finish downloading
4.	Make sure the checksums are correct just to make sure no one is installing a virus. 
Creating the ArchLinux Virtual Machine
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
Installing the ArchLinx Modifications 
1.	Type 
