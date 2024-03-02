## Arch Linux Install Notes ##

# In the ISO

 - 1: loadkeys de
 - 2: lsblk
 - 3: gdisk /dev/nvme0n1
      - x
      - z
 - 4: reboot

 - 5: loadkeys de
 - 6: lsblk
 - 7: cfdisk /dev/nvme0n1
      - p1 = 1GiB, EFI
      - p2 = 16GiB, Linux Swap
      - p3 = ENTER, Linux Filesystem
 - 8: mkfs.fat -F 32 /dev/nvme0n1p1
 - 9: mkswap /dev/nvme0n1p2
 - 10: swapon /dev/nvme0n1p2
 - 11: mkfs.ext4 /dev/nvme0n1p3
 
 - 12: mount /dev/nvme0n1p3 /mnt
 - 13: mount --mkdir /dev/nvme0n1p3 /mnt/home
 - 14: mount --mkdir /dev/sda1 /mnt/media
 
 - 15: pacstrap -K /mnt base base-devel linux linux-firmware sof-firmware linux-headers nano networkmanager grub efibootmgr intel-ucode bash-completion
 - 16: genfstab -U /mnt >> /mnt/etc/fstab
 
 - 17: arch-chroot /mnt

# In the install

 - 1: sudo systemctl enable NetworkManager
 - 2: nano /etc/pacman.conf
      - ILoveCandy
      - Parallel Downloads
      - Multilib
 - 3: pacman -Syy
 - 4: ln -sf /usr/share/zoneinfo/Europe/Berlin /etc/localtime
 - 5: hwclock --systohc
 - 6: nano /etc/locale.gen
      - de_DE.UTF-8
 - 7: locale-gen
 - 8: echo "LANG=de_DE.UTF-8" >> /etc/locale.conf
 - 9: echo "arch" > /etc/hostname
 - 10: echo "KEYMAP=de" > /etc/vconsole.conf
 - 11: useradd -m -G wheel,power -s /bin/bash justus
 - 12: passwd justus
 - 13: EDITOR=nano visudo
      - %wheel

# Nvidia stuff

 - 1: pacman -S nvidia-dkms libglvnd nvidia-utils opencl-nvidia nvidia-settings
 - 2: nano /etc/mkinitcpio.conf
      - MODULES (nvidia nvidia_modeset nvidia_uvm nvidia_drm)
 - 3: exec /usr/bin/mkinitcpio -P
 - 4: arch-chroot /mnt
 - 5: nano /etc/default/grub
      - GRUB_CMDLINE_LINUX_DEFAULT="loglevel=3 quiet nvidia_drm.modeset=1"

# Grub

 - 1: grub-install --target=x86_64-efi --efi-directory=/boot --bootload-id=grub --removable
 - 2: grub-mkconfig -o /boot/grub/grub.cfg
 - 3: reboot

# Post configuration

 - 1: sudo pacman -S pipewire pipewire-pulse pipewire-jack
 - 2: systemctl --user enable pipewire pipewire-pulse
 - 3: sudo localectl set-keymap de

# Installing a DE

 - 1: sudo pacman -S gnome gnome-tweaks gdm gst-libav
 - 2: sudo systemctl enable gdm
 - 3: reboot

[OPTIONAL]

 - 1: sudo pacman -S i3-wm polybar rofi alacritty
 - 2: reboot

[ONLY FOR ME]

 - 1: pactl list sources
 - 2: pactl set-default-source alsa_input.usb-Solid_State_System_Co._Ltd._LCS_USB_Audio_000000000000-00.mono-fallback

 - sudo pacman -S xf86-input-evdev
 - sudo mv /usr/share/X11/xorg.conf.d/40-libinput.conf ~/
 - sudo cp ~/50-mouse-acceleration.conf /etc/X11/xorg.conf.d/
 - sudo reboot
 - sudo pacman -S lxappearance-gtk3

[50-mouse-acceleration.conf]
```
Section "InputClass"
	Identifier "My Mouse"
	MatchIsPointer "yes"
	Option "AccelerationNumerator" "1"
	Option "AccelerationDenominator" "1"
	Option "AccelerationThreshold" "0"
EndSection
```
