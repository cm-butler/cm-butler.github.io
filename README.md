# Arch Linux Installation
- **Preparation**
	- First, I prepared a live usb by downloading the latest Arch Linux ISO and using Rufus to write it
	- I then booted from the live USB in non-UEFI BIOS mode
- **Live Environment**
	- Once I booted into the live environment, I connected to my phone hotspot using `iwctl`
	- Ran `timedatectl` to ensure my time was synced
	- Formatting disk and creating filesystems:
		- Fdisk:
			1. `fdisk -l`
			2. `fdisk /dev/sda`
			3. MBR Partition Table `o` (non EFI system)
			4. Create 4gb swap partition: `n` , `p`, `1`, enter for default starting sector, `+4G`
			5. Partition the rest for primary install partition: `n`, `p`, `2`, enter, enter
			6. changed /dev/sda1 to swap with `t`, changing the hex code to `82`
			7. `w` write changes to disk
		- formatted /dev/sda2 as ext4 with `mkfs.ext4 /dev/sda2`
		- formatted /dev/sda1 as swap with `mkswap /dev/sda1`
		- ran `mount /dev/sda2 /mnt` to mount the root volume
		- enabled swap volume with `swapon dev/sda1`
	- now for the big install: `pacstrap -K /mnt base linux linux-firmware nano sudo networkmanager man`
	- generated fstab with `genfstab -U /mnt >> /mnt/etc/fstab`
	- chroot into file system with `arch-chroot /mnt`:
		- set the time zone with `ln -sf /usr/share/zoneinfo/America/Chicago /etc/localtime`
		- ran `hwclock --systohc`
		- set hostname with `echo clarkarch > /etc/hostname`
		- changed root password with `passwd`
		- ran `pacman -S grub` to get `grub-install`
		- ran `grub-install --target=i386-pc /dev/sda`
		- exited chroot and used `reboot now`, then chose to boot from hard drive in BIOS
		- didn't boot... boooo...
		- boot back into live environment and `fdisk -l` to ensure everything was correct
		- things are not correct, root partition is not flagged as bootable
		- ran `fdisk /dev/sda` ,`2`(root is sda2), `w`
		- realize i also didn't configure grub
		- chroot in: `mount /dev/sda2 /mnt`, `arch-chroot /mnt`
		- ran `grub-mkconfig -o /boot/grub/grub.cfg` - completed without errors
		- `reboot now`
		- yay it booted!
- **Arch Linux**(no DE)
	- login as root and use password i set
	- connect to TUdevices:
		- used `ip link` to find my MAC address and add it to TUdevices
		- start NetworkManager service with `systemctl start NetworkManager.service`
		- start NetworkManager for CLI with `nmcli radio wifi on`
		- see list of networks with `nmcli dev wifi list`
		- connect to wifi with `nmcli dev wifi connect "TUdevices"`
	- add sudoers clark and codi with `useradd -m -G wheel clark` and `useradd -m -G wheel codi`
	- uncommented line that gives wheel sudo privileges with `visudo`
	- Desktop Environment:
		- decided to install xorg with xcfe for a light desktop environment
		- used `pacman -S xorg xfce4 xfce4-goodies` to install DE
		- installed lightdm as display manager with `pacman -S lightdm light-dm-gtk-greeter`
		- reboot
- **Arch Linux** (DE)
	- installed zsh with `sudo pacman -S zsh`
	- changed colors in shell with preferences GUI
	- added `alias update='sudo pacman -Syu'`
