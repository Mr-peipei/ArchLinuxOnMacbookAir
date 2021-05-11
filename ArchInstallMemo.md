
# Arch Install Settings

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

