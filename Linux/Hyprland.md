
## __Шаг 1. "Установка + проверка"__
Всё нужное должно установиться автоматически командой:
```
sudo pacman -S hyprland kitty

# запуск
hyprland
```

Теперь необходимы шрифты:
Ссылка с примерами nerd шрифтов https://www.nerdfonts.com/font-downloads.
```
# создаём нужные директории
mkdir -p ~/Downloads
mkdir -p ~/.fonts

# скачиваем шрифты
cd ~/Downloads
wget https://github.com/ryanoasis/nerd-fonts/releases/download/v3.4.0/FiraCode.zip
wget https://github.com/ryanoasis/nerd-fonts/releases/download/v3.4.0/JetBrainsMono.zip

# распаковывем
unzip ~/Downloads/FiraCode.zip -d ~/.fonts/
unzip ~/Downloads/JetBrainsMono.zip -d ~/.fonts/

# ставим ещё один
sudo pacman -S ttf-font-awesome

# обновляем cache
fc-cache -f -v
```

Всё должно запустится, а на `Super+Q` откроется терминал.


## __SDDM:__

Установка sddm и темы для него:
```
sudo pacman -S sddm

wget https://github.com/catppuccin/sddm/releases/download/v1.1.0/catppuccin-mocha.zip

sudo unzip catppuccin-mocha.zip -d /usr/share/sddm/themes/
```


## __GRUB:__

Установим тему для grub menu:
```
git clone https://github.com/catppuccin/grub.git && cd grub
sudo cp -r src/* /usr/share/grub/themes/

# добавляем строку в конфиг
sudo -E nvim /etc/default/grub
GRUB_THEME="/usr/share/grub/themes/catppuccin-<flavor>-grub-theme/theme.txt"

sudo grub-mkconfig -o /boot/grub/grub.cfg
```


## __ZSH:__

Заменим bash на более хайповый zsh.
```
sudo pacman -S zsh

# смена оболочки
chsh -s $(which zsh)

# установка oh-my-zsh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

# тема
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k

nvim ~/.zshrc
ZSH_THEME="powerlevel10k/powerlevel10k"

# плагины
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting

nvim ~/.zshrc
plugins=(git zsh-autosuggestions zsh-syntax-highlighting)

source ~/.zshrc

```
