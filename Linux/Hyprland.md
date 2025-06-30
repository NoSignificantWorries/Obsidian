
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

## __ZSH:__

Заменим bash на более хайповый zsh.
```
sudo pacman -S zsh

# смена оболочки
chsh -s $(which zsh)

# установка oh-my-zsh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

```