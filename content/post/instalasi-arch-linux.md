---
date: 2017-04-01T19:57:42+07:00
lastmod: 2017-04-01T19:57:42+07:00

title: "Instalasi Arch Linux"
description: "Memasang Arch linux dengan systemd boot"
author: "Imam Digmi"

weight: 50
draft: false
keywords: ["arch", "linux", "install"]
tags: ["archlinux"]
categories: ["Arch Linux"]
---
Mungkin bagi sebagian orang yang berkecimpung didunia linux pasti sudah tau salah satu distribusi linux yang satu ini, Archlinux memang menjadi salah satu distribusi linux yang banyak disukai oleh orang-orang karena kemudahannya dalam kustomisasi, karena distribusi linux ini dibangun _form scratch_ yang artinya Archlinux dibangun mulai dari awal dan tanpa mengadopsi atau turunan distribusi apapun ini berbeda dengan ubuntu yang dibangun dari distribusi debian atau fedora yang dibangun dari distribusi linux red hat, biasanya yang menggunakan archlinux ini salah satu orang yang bertipe anti-mainstream namun ada juga yang tidak menyukai Archlinux karena susah digunakan dan tidak direkomendasikan bagi orang yang baru menggunakan linux.

## Langkah-langkah
Agar mudah dipahami kita akan membagi proses instalasi ini menjadi tiga bagian yaitu:
1. Pre Instllation
2. Arch Linux Core Installation
3. Post Installation
Oke mari kita mulai instalasinya.

## Pre Installation
### Download image ISO archlinux
Anda dapat mengunduh file .iso archlinux [disini](http://mirror.devilzc0de.org/archlinux/iso/2017.04.01/) atau [disini](http://download.nus.edu.sg/mirror/arch/iso/2017.04.01/) atau anda juga dapat melihat daftar server yang menyediakan image ISO nya [disini](https://www.archlinux.org/download/) dan pilihlah file yang berekstensi dor iso (`*.iso`)

### Membuat bootable
Untuk membuat bootable anda dapat menggunakan [Etcher](https://etcher.io/) atau jika anda menggunakan windows anda dapat menggunakan [Rufus](https://rufus.akeo.ie/) atau anda juga dapat memilih aplikasi bootable maker sendiri, feel free to choose your favorite bootable maker tapi menurut pengalaman penulis iso archlinux tidak dapat dibuat dengan aplikasi [Universal Usb Installer](https://www.pendrivelinux.com/universal-usb-installer-easy-as-1-2-3/).

### Menjalankan bootable
Untuk menjalankan bootable tema-teman dapat mencari sendiri caranya di internet

## Arch Linux Core Installation
### Koneksi ke internet
Pertama kita koneksikan dulu ke Internet karena instalasi archlinux membutuhkan koneksi internet untuk mengunduh paket-paket yang dibutuhkan. Koneksi internet hanya dapat dilakukan pada direct wifi access, koneksi seperti Wifi Id tidak mendukung karen harus login dulu ke webnya.  
```
# wifi-menu
```

### Membuat partisi (Partitioning)
Setelah itu kita akan membuat partisi _Hard Drive_ kita
```
# cfdisk
```
Pilih GPT lalu kita buat partisinya, skema yang digunakan penulis seperti yang ada dibawah ini, namun anda dapat merubahnya sesuai keinginan

```
1. 800 MB, ESP (EFI System Partition)
2. 16 GB, swap
3. 70 GB, /
4. 150 GB, /home
```
Format partisi yang tadi kita buat

```
# mkfs.fat -F 32 /dev/sda1
# mkswap /dev/sda2
# mkfs.ext4 /dev/sda3 -L "ArchLinux"
# mkfs.ext4 /dev/sda4 -L "Home"
```
Lalu kita mount partisinya

```
# mount /dev/sda3 /mnt
# mkdir /mnt/{boot,home}
# mount /dev/sda1 /mnt/boot
# swapon /dev/sda2
# mount /dev/sda4 /mnt/home
```

### Memilih server mirror
Untuk melihat daftar mirror yang tersedia anda dapat melihatnya [disini](https://wiki.archlinux.org/index.php/Mirrors#Indonesia). Setelah memilih mirrornya edit file `/etc/pacman.d/mirrorlist` :

```
# nano /etc/pacman.d/mirrorlist
```

Lalu tambahkan mirror nya, penulis sendiri memilih mirror kambing dari kampus UI di jakarta karena lumayan cepat dan stabil

```
Server = http://kambing.ui.ac.id/archlinux/$repo/os/$arch
```

### Install base

```
# pacman -Syy
# pactsrap /mnt base base-devel
```

tunggu sampai proses sinkronisasi selesai, lalu generate `fstab` untuk _mounting_  semua partisi agar dapat di booting oleh system

```
# genfstab -L -p -P /mnt > /mnt/etc/fstab
```

### Set system (chroot)

```
# arch-chroot /mnt
```

### Hostname, locale and zoneinfo

```
# echo idiot > /etc/hostname
```

set locale:

```
# nano /etc/locale.gen
```

cari baris dua baris bahasa dibawah ini dengan cara menakan tombol `CTRL+w` lalu hilangkan tanda (`#`), contoh :

```
en_US.UTF-8 UTF-8
id_ID.UTF-8 UTF-8
```

setelah itu buat file `locale.conf`

```
# nano /etc/locale.conf
```

Pada file yang baru saja kita buat tambahkan

```
LC_COLLATE=C
LANG=en_US.UTF-8
LC_TIME=id_ID.UTF-8
```

Generate locale

```
# locale-gen
```

dan buat symboliclink untuk localtime

```
# ln -sf /usr/share/zoneinfo/Asia/Jakarta /usr/localtime
```

### Instalasi paket-paket untuk mendukungan jaringan

```
# pacman -S bash-completion
# pacman -S ntfs-3g wpa_supplicant dialog
```

### Membuat user
Buat grup user untuk `sudo`

```
# groupadd sudo
```

lalu buat usernya

```
# useradd -m -g users -G sudo,power,storage nama_user
```

edit file sudoers

```
# nano /etc/sudoers
```

hilangkan tanda (`#`) pada baris `%sudo ALL=(ALL)`, lalu buat password untuk user yang kita buat tadi

```
# passwd nama_user
# passwd root
```

### Create bootloader
Untuk bootloadernya penulis menggunakan `systemd-boot` karena lebih mudah, ringan dan simple daripada menggunakan `grub`
Buat initramfs:

```
# mkinitcpio -p linux
```

Jika komputer anda menggunakan processor intel maka instal paket `intel-ucode`

```
# pacman -S intel-ucode
```

Buat bootloader

```
# bootctl install
```

Buat entry untuk systemd-boot pada `/boot/loader/entries/`

```
# nano /boot/loader/entries/arhlinux.conf
```

Isikan file tersebut dengan

```
title Archlinux
linux /vmlinuz-linux
initrd /intel-ucode.img
initrd /initramfs-linux.img
options root=/dev/sda<X> rw
```
> Note: Jika anda tidak menggunalan `intel-processor` hapus barus ini `initrd /intel-ucode.img` dan ubah <X> dengan partisi **root** anda.

Finaly, `exit` dari chroot lalu `reboot`.

## Post Installation
### Install driver VGA
Sebelum kita menginstal dukunagn paket untuk kartu VGA ada baiknya kita memeriksa dulu VGA yang kita gunakan dengan cara

```
$ sudo lspci | grep VGA
```

setelah sudah diketahui kita instal paket dukungan VGA pilih salah satu saja yang anda gunakan pada komputer anda

- Untuk VGA Standard

```
$ sudo pacman -S xf86-video-vesa mesa mesa-demos
```

- Untuk VGA Intel

```
$ sudo pacman -S xf86-video-intel
```

- Untuk VGA Nvidia

```
$ sudo pacman -S xf86-video-nouveau
```

### Install Desktop Envirounment
Feel free to choose your favorite Desktop Envirounment bro, namun penulis lebih memilih XFCE karena ringan dan juga simple

```
$ sudo pacman -S xfce4 xfce4-goodies
```

instal NetworkManager

```
$ sudo systemctl enable NetworkManager
```

instal GDM (gnome display manager) untuk tampilan login user

```
$ sudo pacman -S gdm
$ sudo systemctl enable gdm
```

atau jika anda lebih menyukai login dengan terminal `TTY` atau tanpa tampilan anda dapat menggunakan `xorg-xinit`

```
$ sudo pacman -S xorg-xinit
```

lalu kita buat file `~/.xinitrc` agar ketika kita ingin masuk ke tampilan desktop kita dapat menggunakan `startx`

```
$ touch ~/.xinitrc
$ echo "exec xfce4-session" > ~/.xinitrc
```

lalu reboot

```
$ sudo reboot
```
> Note: Jika anda menggunakan `startx` maka untuk masuk ke tampilan desktop gunakan perintah `$ startx`

Tadaaaa, Archlinux sudah bisa digunakan.

**Enjoy!**
Semoga bermanfaat.