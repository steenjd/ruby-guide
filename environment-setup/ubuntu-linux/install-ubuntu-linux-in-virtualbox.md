# How to Install Ubuntu Linux in Oracle VM VirtualBox

Versions of software used in this guide:
- Oracle VM VirtualBox 6.0.4
- Ubuntu 18.10
- Windows 10 Pro as the host OS (but these instructions should work with any host OS)

## Download the Ubuntu Linux installation disk image

https://www.ubuntu.com/download/desktop

In this guide, we'll use the desktop version of Ubuntu 18.10 64-bit. This is the non-LTS version, which will have the latest versions of packaged software and will be upgradable to the next Ubuntu release within six months.

Later, you will attach the downloaded ISO disk image to your Ubuntu virtual machine (VM) as a virtual optical disc.

## Install VirtualBox

Download and install VirtualBox from Oracle.

https://www.oracle.com/technetwork/server-storage/virtualbox/downloads/index.html

## Create a virtual machine for Linux

Launch VirtualBox.

From the Machine menu, choose New.

Name your new VM "Ubuntu" or whatever you would like to call it.

Choose Type "Linux" and Version "Ubuntu 64-bit". These will be selected automatically if you entered a virtual machine name containing "Ubuntu".

Memory (RAM): Minimum 2048 MB (2 GB), preferably 3 GB if your host system can spare it. When the Ubuntu VM starts, this amount of RAM will be taken from the available free RAM of your host OS. (In Windows, you can check available free RAM in Task Manager.)

Create a virtual hard disk with a size of 20 GB. Choose one of the recommended (bold) hard disk file types. VDI is native to VirtualBox, and VHD is a disk image format that Windows can operate on through Computer Management. You can convert between formats later if needed.

## Customize the virtual machine settings

Open the Settings for your new VM.

System category:
- Motherboard tab:
  - Boot Order: Move the Hard Disk entry so it's higher in the list than Optical. This will prevent you from accidentally booting from the attached CD/DVD image after your Ubuntu OS is installed.
  - Enable EFI (optional). This is a more modern system boot loader technology than the older variant, MBR.
- Processor tab:
  - Processor(s): Drag the slider as high as it can go in the green region (optional). This lets your VM make use of multiple CPU cores.
  - Enable PAE/NX. If the guest OS supports it, regions of data memory can be marked "no execute" (NX) to prevent certain types of malware attacks such as buffer overruns.
- Acceleration tab:
  - Paravirtualization Interface: KVM. This is [recommended for Linux guest OSes](https://www.virtualbox.org/manual/ch10.html#gimproviders).

Storage category:
- Storage Devices: Select the CD icon (probably labeled "Empty").
- Attributes: Optical Drive: Tap the CD icon to open its drop-down menu, and choose the Choose Virtual Optical Disk File option. Browse to and select the Ubuntu ISO image file you downloaded at the start of this guide, provided the download has completed.

Tap OK.

## Install Ubuntu Linux to the virtual machine

Start your new Ubuntu VM. It should boot from the virtual optical drive and start the Ubuntu installer.

Make a note of the "host key" displayed in the bottom-right corner of the VirtualBox VM window. On Windows, it's Right Ctrl. While the VM window has focus, your keyboard input will go to the VM. To release the VM's keyboard capture, press the host key once, and then you can use host OS key combos like Alt-Tab to switch to another app. You can also click with your mouse on another app to release the keyboard capture.

At the GNU GRUB menu, choose the Install Ubuntu option.

Choose your preferred language and keyboard layout.

Choose the "Minimal installation" option for which set of apps to install.

Installation type: Erase disk and install Ubuntu. "Use LVM" is optional; LVM makes it easier to add space to your Linux virtual disk later if it fills up, and it's a good technology to learn for server maintenance.

Choose a username and password for signing in to your Ubuntu system for everyday use. This will be a limited user account, but it will have `sudo` privileges for running commands as `root`.

Installation will take several minutes. Restart when prompted. The installer will ask you to remove the installation medium, but the virtual optical disc should have ejected automatically and you can just press Enter.

Choose Ubuntu at the GRUB boot menu to boot into your new Linux VM.

## Configure Ubuntu

At the login screen, tap the gear icon and choose the Wayland option. The initially default, non-Wayland option has buggy screen drawing routines with some combinations of video drivers. The Wayland option will remain the new default across reboots.

Log in with the username and password you created during Ubuntu installation.

Whenever Software Updater pops up, let it install all available updates.

When prompted for your password for administrative tasks such as installing software, enter the password you used to sign in to Ubuntu.

To increase the VM's display resolution, launch the Settings app, go to the Devices category, Displays subcategory, and change Resolution to a larger size that fits inside your physical screen. You can choose a resolution that matches that of your physical screen, but then you will need to toggle fullscreen view on your VM to avoid scrolling the VM display: press the host key plus F (RightCtrl-F) to toggle fullscreen, or choose the option from the VM window's View menu.

Optionally, you can install [VirtualBox Guest Additions](https://www.virtualbox.org/manual/ch04.html) in Ubuntu. This allows the Linux guest display resolution to resize automatically when you resize the VM window (including fullscreen), lets you share clipboard contents between your host OS and the Ubuntu guest OS, and provides folder sharing between host and guest. To install Guest Additions, go to the Devices menu of your VM window and select "Insert Guest Additions CD image". Choose Run when prompted to automatically run software from the "VBox_GAs_…" CD. The installer will display errors about not being able to build kernel modules, but this is okay; the aforementioned host/guest integration features will still work.

Pin the Terminal app to the "favorites" launcher bar so the app is easier to start. In the apps menu (icon with nine dots at bottom-left), choose Utilities, right-click (or long-tap) Terminal, and choose "Add to favorites".

## Next steps

[Install development tools](install-dev-tools.md)
