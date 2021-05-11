
# Arch Install Settings

### Install Fish bash

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

