
#### __Подготовка:__
1) Подключение к wifi через iwctl:
Если подключение проводное, то ничего не требуется.
```
iwctl
device list

# Если выключен wifi адаптер, то
device wlan0 set-property Powered on

# Запускаем сканирование и выводим список доступных сетей
station wlan0 scan
station wlan0 get-networks

station wlan0 connect <SSID>
# Вводим пароль, когда будет запрошено

# Проверяем подключение
station wlan0 show
exit

# Проверяем соединение
ping google.com
```

2) Проверяем синхронизацию системных часов:
```
timedatectl set-ntp true
# Эта команда ничего не выводит

timedatectl status
# Тут всё должно быть красиво и написано, что синхронизация есть и активна
```

3) Разбиение диска:
Проверяем диски
```
# Команды для выывода существующих дисков и разделов
# компактный вывод
lsblk
# более подробный
fdisk --list
```

Создаём и изменяем таблицу разделов на выбранном диске
```
# Подключаемся к нужному диску
fdisk /dev/nvme0n1

# Проверяем существующую таблицу разделов
p

# Если диск нужно полностью стереть, то переформатируем в gpt
g
# ВНИМАНИЕ!!! Не заускать бездумно, потеряются данные

# Если ещё нет EFI раздела
n
<enter>
<enter>
+550M
# Подтверждаем если надо

# Раздел подкачки (swap)
n
<enter>
<enter>
+8G

# Основной файловвый раздел
n
<enter>
<enter>
<enter>

# Проверяем что всё норм
p
```

Форматируем разделы
```
# Если был создан EFI раздел
t
<номер раздла в таблице>
1  # EFI Sysytem

# Для подкачки
t
<номер раздела>
19  # linux swap
```

Сохраняем изменения
```
w  # записать изменения
q  # выйти из fdisk
```

4) Создание файловых систем на разделах
```
# Если был создан EFI
mkfs.fat -F32 /dev/nvme0n1p1  # заменить на свой диск и номер раздела

# Для swap
mkswap /dev/nvme0n1p5
swapon /dev/nvme0n1p5

# Для основного раздела
mkfs.ext4 /dev/nvme0n1p6
```

5) Создание точек монтирования для разделов
```
# Монтируем главный раздел
mount /dev/nvme0n1p6 /mnt

# Создаём папку и монтируем туда EFI раздел
mkdir -p /mnt/boot/efi
mount /dev/nvme0n1p1 /mnt/boot/efi
```

6) Проверяем точки монтирования через вывод `lsblk`

#### __Установка:__
7) Устанавливаем ядро и остальную базу arch linux
```
pacstrap /mnt base linux linux-firmware
# ждём завершения...
```

8) Создаём таблицы файловых систем на примонтированных разделах и заходим в систему
```
genfstab -U /mnt >> /mnt/etc/fstab

# Входим в arch
arch-chroot /mnt
```

#### __Настройка:__
9) Настраиваем всякое в системе
```
# Временная зона
ln -sf /usr/share/zoneinfo/Asia/Novosibirsk

# Синхронизируем время
hwclock --systohc

# Устанавливаем текстовый редактор
pacman -S neovim

# Добавляем локали в систему
nvim /etc/locale.gen
# раскомментируем строки en_US.UTF-8 UTF-8 и ru_RU.UTF-8 UTF-8
# устанавливаем локали
locale-gen

# Создамём имя компьютеру
nvim /etc/hostname

# Создаём файл hosts
nvim /etc/hosts
# там должно быть что-то такое
127.0.0.1    localhost
::1          localhost
127.0.1.1    <hostname>.localdomain    <hostname>
# если нет, то добавляем
# <hostname> заменить на своё

# Задаём пользователей и пароли
# сначала для root
passwd
# создаём пользователя
useradd -m <username>
passwd dmitry
# добавляем пользователя в группы
usermod -aG wheel,audio,video,storage,scanner <username>
# пока хватит и этих

# Добавляем привелегии sudo
# скачиваем sudo
pacman -S sudo
# изменяем конфиг sudo
EDITOR=nvim visudo
Раскомментируем строку под "Uncomment to allow members of group wheel to execute any command" (Почти в самом низу файла)
# обычно выглядит как %wheel ALL=(ALL:ALL) ALL

# Ставим NetworkManager
pacman -S networkmanager
# включаем
systemctl enable NetworkManager
```

#### __Завершение:__
10) Стваим grub
```
# Устанавливаем
pacman -S grub efibootmgr os-prober

# Ставим grub на загрузочный раздел
grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=GRUB

# Включаем os-prober
nvim /etc/default/grub
# находим и изменяем или добавляем такую строку
GRUB_DISABLE_OS_PROBER=false
# создаём конфиг
grub-mkconfig -o /boot/grub/grub.cfg
```

11) Перезагрузка и первый запуск
```
# выходим из chroot
exit

# размонтируем диски
umount -R /mnt

# перезагрузка
reboot
```

12) Переходим к [[Первичная настройка Arch Linux и полезные утилиты]]
