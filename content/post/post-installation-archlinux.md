---
date: 2018-02-01T16:36:07Z
lastmod: 2018-02-01T16:36:07Z

title: "Post-Installation Arch Linux"
description: "Beberapa hal yang baik dilakukan setelah instalasi Arch Linux"
author: "Imam Digmi"

weight: 50
draft: false
keywords: [arch, linux, post-installation]
tags: [archlinux]
categories: [Arch Linux]
---

# Disable Beep Sound (PC speaker)
PC speaker dapat di-_disabled_ dengan cara meng-unload `pcspkr` kernel module
```
sudo rmmod pcspkr
```

Blacklist module `pcspkr` akan mencegah __udev__ pada saat booting sistem
```
echo "blacklist pcspkr" > /etc/modprobe.d/nobeep.conf
```

Selengkapnya bisa dibaca di dokumentasi resminya [disini](https://wiki.archlinux.org/index.php/PC_speaker#Disable_PC_Speaker)

# File Manager Support
Flashdisk dan Device Android tidak terbaca dan ekstrak file tidak bisa di thunar
```
$ sudo pacman -S gvfs file-roller libmtp
```

Selengkapnya bisa dibaca di dokumentasi resminya [disini](https://wiki.archlinux.org/index.php/MTP)

# Cursor Size
Buat file `.Xresources` di direktori home
```
$ nano ~/.Xresources
```

Definisikan ukuran cursor
```
!Xcursor.theme: cursor-theme
Xcursor.size: 16
```

Selengkapnya bisa dibaca di dokumentasi resminya [disini](https://wiki.archlinux.org/index.php/Cursor_themes#X_resources)

# Crash Netctl
Jika menggunakan `wifi-menu` tapi muncul error seperti berikut

```
Job for netctl@wlp6s0\x2dF48EDF4B5991C93D.service failed because the control process exited with error code.
See "systemctl status "netctl@wlp6s0\\x2dF48EDF4B5991C93D.service"" and "journalctl -xe" for details.
```

karena service `NetworkManager` telah aktif, menonaktifkan networkmanager dapat dilakukan dengan menggunakan `systemctl`

```
$ sudo systemctl stop NetworkManager
```

Jika ingin secara otomatis mendeteksi dan mengkoneksikan `wifi` dapat menggunakan applet networkmanager dan meng-_enable_ service NetworkManager

```
sudo pacman -S networkmanager network-manager-applet
```

# Xinit Config
Buat file `.xinitrc` di direktori home
```
$ nano ~/.xinitrc
```

Buat beberapa exec session DE atau WM
```
session=${1:-xfce}

case $session in
    i3|i3wm           ) exec i3;;
    xfce|xfce4        ) exec startxfce4;;
    *                 ) exec $1;;
esac
```

Konfigurasi diatas secara default akan mengaktifkan session XFCE4 pada saat mengeksekusi `startx`, jika ingin menggunakan session lain (DE/WM) maka tambahkan dibagian `case` lalu ekseskusi dengan cara `startx <nama session>`, contoh :
```
$ startx i3
```

atau

```
$ startx ~/.xinitrc session
```
Selengkapnya bisa dibaca di dokumentasi resminya [disini](https://wiki.archlinux.org/index.php/Xinit)

# TTY Font
Buat file `/etc/vconsole.conf`
```
$ sudo nano /etc/vconsole.conf
```

Definisikan nama font dan map nya
```
FONT=latarcyrheb-sun32
FONT_MAP=8859-2
```

Selengkapnya bisa dibaca di dokumentasi resminya [disini](https://wiki.archlinux.org/index.php/Fonts#Console_fonts)
