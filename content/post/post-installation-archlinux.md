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

# Optional
## Monitor for XFCE
Download code
```
$ wget https://raw.githubusercontent.com/lightful/xfce-hkmon/master/xfce-hkmon.cpp
```

Compile
```
$ g++ -std=c++0x -O3 -lrt xfce-hkmon.cpp -o xfce-hkmon
```

Pindahkan hasil complie (executable) ke direktori `/usr/local/bin/`
```
$ sudo mv xfce-hkmon /usr/local/bin/
```

Tambahkan item __Generic Monitor__ pada panel :
1. Menu Settings
2. Klik menu Panel
3. Klik Tab Items
4. Klik Tombol __+__ warna hijau
5. Pilih __Generic Monitor__ lalu klik tombol __add__

![Generic Monitor](/images/post-installation-archlinux/add-generic-monitor.png)

> Jika tidak ada maka harus install dulu dengan cara `sudo pacman -S xfce4-genmon-plugin`

Klik kanan pada __Generic Monitor__ applet di panel lalu klik _properties_ lalu isikan kode di bawah ini pada kolom __Command__

```
/usr/local/bin/xfce-hkmon NET CPU TEMP IO RAM
```

![Generic Monitor Config](/images/post-installation-archlinux/generic-monitor-config.png)

Refresh panel
```
$ xfce4-panel -r
```

Hasilnya seperti gambar berikut

![Generic Monitor Result](/images/post-installation-archlinux/result-generic-monitor.png)