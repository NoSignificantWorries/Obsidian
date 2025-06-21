
## __Настройка wifi подключения в nmcli:__

### Простое быстрое подключение
Для начала работы надо, очевидно, настроить интернет.

Проверяем список доступных интерфейсов и их статус:
```
nmcli device status
```
Смотрим чтобы было wifi устройство

Теперь выводим список доступных сетей:
```
nmcli device wifi list
```

Находим нужную сеть и подключаемся:
```
sudo nmcli device wifi connect <SSID> password <password>
```


### Создание профиля
Лучше будет создать профиль подключения:
```
sudo nmcli con add type wifi ifname <имя_устройства> con-name <имя_подключения> ssid <SSID>

sudo nmcli con modify <имя_подключения> wifi-sec.key-mgmt wpa-psk

sudo nmcli con modify <имя_подключения> wifi-sec.psk <пароль>

sudo nmcli con up <имя_подключения>
```

### Подключение ethernet
```
sudo nmcli device connect <имя_устройства>
```

### Базовые команды для управления
```
# список поодключений
nmcli connection show

# активация подключения
sudo nmcli connection up <имя_подключения>

# деактивация подключения
sudo nmcli connection down <имя_подключения>

# удаление подключения
sudo nmcli connection delete <имя_подключения>

# изменение пароля
sudo nmcli connection modify <имя_подключения> wifi-sec.psk <новый_пароль>

# включение автоподключения
sudo nmcli connection modify <имя_подключения> connection.autoconnect yes
```


## __Пакеты, скачивание и установка файлов из интернета:__

### Обновляем систему:
```
sudo pacman -Syu
```

### Установка _curl_ и _wget_:
```
sudo pacman -S wget curl
```

### Установка YaY:
```
sudo pacman -S base-devel
git clone https://aur.archlinux.org/yay.git
cd yay
makepkg -si
```

## __Видеодрайверы:__

Ставим драйверы для видеокарты:
1) Для _nvidia_:
	`sudo pacman -S nvidia`
	`sudo reboot`
	`nvidia-smi`
	`sudo pacman -S nvtop`

## __Брэндмауэр:__
```
sudo pacman -S ufw
sudo ufw enable
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow ssh
```
