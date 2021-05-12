
# Arch Install Settings
## create usb
download iso file from https://www.archlinux.org/download
following the command by mac terminal
```
$ hdiutil convert -format UDRQ -o arch archlinux......iso
$ diskutil list 
$ diskutil umount
$ sudo dd if=arch.dmg of=/dev/disk2 bs=1m
```

## Partisioning

#### diskutility
use mac os diskutility, change os partisioning.



#### cgdisk command
after reboot alt/option.
```
# cgdisk /dev/sda
```

#### format
```
# mkfc.ext4 /dev/sda5
# mkfc.ext4 /dev/sda6
```

#### mount
```
# mount /dev/sda6
# mount /mnt/boot
# mount /dev/sda5 /mnt/boot
```


## Install
#### package install
```
# vim /etc/pacman.d/mirrolist
# pacstrap /mnt base 
# genfstab -p /mnt >> /mnt/etc/fstab
```

#### system settings
```
# arch-chroot /mnt /bin/bash
# echo murakami > /etc/hostname
# ln -s /usr/share/zoneinfo/Asia/Tokyo /etc/localtime
# hwclock --systonic --utc

# sudo vim /etc/local.gen
# locale-gen
# echo LANG=en_US.UTF-8 > /etc/locale.conf
# export LANG=en_US.UTF-8

# mkinitcpio -p linux
# passwd
```

## Bootloader

#### create boot.efi file
```
# pacman -S grub-efi-x86_64
# vim /etc/default/grub
```
```
GRUB_CMDLINE_LINUX_DEFAULT="quiet rootflags=data=writeback"
```
```
# mkdir /boot/grub/
# grub-mkconfig -o /boot/grub/grub.cfg
# grub-mkstandalone -o boot.efi -d usr/lib/grub/x86_64-efi -O x86_64-efi --compress=xz boot/grub/grub.cfg
```
copy boot.efi file in your USB (anything is fine)
 usb name: sdd1
```
# dmesg | tail 
# mkdir /mnt/usbdisk && mount /dev/sdd1 /mnt/usbdisk
# sudo cp boot.efi /mnt/usbdisk/
# umount /mnt/usbdisk
# pacman -S iw wireless-tools wpa_supplicant dialog networkmanager
# exit
# reboot
```

#### mac bootloader settings
use diskutility, format dev/disk04 (HFS+ 拡張ジャーナル)
```
$ cd "/Volumes/Boot Arch from the Apple boot loader"
$ mkdir System mach_kernel
$ cd System
$ mkdir Library
$ cd Library
$ mkdir CoreServices
$ cd CoreServices
$ vim SystemVersion.plist
```
```
<xml version="1.0" encoding="utf-8"?>
<plist version="1.0">
<dict>
    <key>ProductBuildVersion</key>
    <string></string>
    <key>ProductName</key>
    <string>Linux</string>
    <key>ProductVersion</key>
    <string>Arch Linux</string>
</dict>
</plist>
```

and use recovery mood(reboot, cmd+r), open a ternimal
```
# csrutil disable
```

and return normal terminal
```
$ sudo bless --device /dev/disk04 --setBoot
```


## add user and sudo group
```
# useradd -m -g wheel -s bash murakami
# passwd murakami
# pacman -S sudo
# usermod --append --groups wheel murakami
# visudo
```
```
## Uncomment to allow members of group wheel to execute any command
%wheel ALL=(ALL) ALL
```

test
```
# su - murakami
$ sudo whoami
```

## Install Display Settings
```
$ sudo pacman -Syu
$ sudo pacman -S xf86-video-intel
$ sudo pacman -S xorg-server xterm xorg-xinit
$ sudo pacman -S lightdm lightdm-gtk-greeter
```
```
-----------------------------------------------------
[Seat:*]
# Add Settings
greeter-session=lightdm-gtk-greeter
# End
```

```
$ sudo systemctl enable lightdm.service
$ pacman -S xfce4 xfce4-goodies gvfs pulseaudio pavucontrol
$ yay -S gamin
```
```
$ startxfce4
```

## Install Japanase Settings
```
$ yay -S fcitx-im fcitcx-configtool fcitx-mozc
$ echo -e "export GTK_IM_MODULE=fcitx\nexport QT_IM_MODULE=fcitx\nexport XMODIFIERS=@im=fcitx" >> ~/.xprofile
```
add Japanase Mozc In keybord Settings

## Install bluetooth
```
$ pacman -S blueman
```


## Install wi-fi
I cannnot use macbook air wi-fi (broadcom)
so I will use tp-link wi-fi.
```
$ sudo pacman -S networkmanager
$ sudo systemctl enable NetworkManager.service
$ sudo pacman -S network-manager-applet xfce4-notifyd gnome-keyring
```
after install, logout and re-login


## Install yay
```
$ git clone https://aur.archlinux.org/yay.git
$ cd yay
$ makepkg -si
```
なにかディレクトリを作ったあとに、makepkg -siとする


## Customize Arch Linux theme

#### wallpaper
```
$ sudo pacman -S archlinux-wallpaper
```
インストール後、デスクトップ右クリックで  
/usr/share/archlinux/wallpaper配下を選択し、壁紙を変更する

##### theme
```
$ sudo pacman -S arc-gtl-theme
```
Settings>Appearance>Style
Settings>Appearange>Window Manager

#### icon
```
$ git clone https://github.com/horst3180/arc-icon-theme
$ ./autogen.sh --prefix=/usr
$ sudo make install
```

#### menu-icon
https://commons.wikimedia.org/wiki/File:Archlinux-icon-crystal-64.svg
install
```
sudo cp Archlinux-icon-crystal-64.svg.png /usr/share/pixmaps/archicon.svg
```

## Install vimrc from my git
```
$ git clone https://github.com/Mr-peipei/dotfile.git
$ cd dotfile
$ cp .vimrc ~/.vimrc
```

## Install chromium
```
$ sudo pacman -Syu
$ sudo pacman -S chromium
```

## Install Fish bash

```
yay -S fish
curl https://git.io/fisher --create-dirs -sLo ~/.config/fish/functions/fisher.fish
fisher install oh-my-fish/theme-bobthefish
yay -S powerline-fonts
fisher install jethrokuan/z
yay -S fzf
fisher install jethrokuan/fzf
```

and add in ~/.config/fish/config.fish

```
set -U FZF_LEGACY_KEYBINDINGS 0
```

