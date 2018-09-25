---
date: 2018-02-03T06:30:26Z
lastmod: 2018-02-03T06:30:26Z

title: "Setup NVIDIA di Arch Linux"
description: "Setup NVIDIA Driver di Archlinux"
author: "Imam Digmi"

weight: 50
draft: false
keywords: [archlinux, nvidia, setup]
tags: [archlinux]
categories: [Arch Linux]
---

# Memeriksa hardware 
Cek driver hardware komputer
```
$ lspci -k | grep -A 2 -E "(VGA|3D)"
```

Outputnya kurang lebih akan seperti ini
```
00:02.0 VGA compatible controller: Intel Corporation 4th Gen Core Processor Integrated Graphics Controller (rev 06)
        Subsystem: Dell 4th Gen Core Processor Integrated Graphics Controller
        Kernel driver in use: i915
--
02:00.0 3D controller: NVIDIA Corporation GK107GLM [Quadro K1100M] (rev a1)
        Subsystem: Dell GK107GLM [Quadro K1100M]
        Kernel driver in use: nvidia
```

# Instalasi Driver
Install NVIDIA driver
```
$ yaourt -S nvidia nvidia-utils cuda
```

Install cUDNN
```
$ yaourt -S cudnn
```

Cek NVIDIA GPU info
```
$ sudo nvidia-xconfig --query-gpu-info
```

Kurang lebih ouputnya akan seperti ini
```
Number of GPUs: 1

GPU #0:
    Name      : Quadro K1100M
    UUID      : GPU-6e0ecd29-e6da-a3cd-30bc-9c2d3fe458ce
    PCI BusID : PCI:2:0:0

    Number of Display Devices: 0
```

# Konfigurasi Driver
Generate konfigurasi NVIDIA untuk X secara otomatis
```
$ sudo nvidia-xconfig
```

Jika menggunakan konfigurasi hasil generate tersebut pada komputer saya NVIDIA tidak berjalan lancar maka perlu tambahan konfigurasi pada file `/etc/X11/xorg.conf`. Konfigurasinya kurang lebih seperti berikut
```
Section "ServerLayout"
    Identifier     "layout"
    Screen      0  "nvidia"
    Inactive	   "intel"
EndSection

Section "Device"
    Identifier      "intel"
    Driver	        "modesetting"
    BusID           "PCI:0:2:0"
    Option          "AccelMethod"   "None"
EndSection

Section "Device"
    Identifier      "nvidia"
    Driver          "nvidia"
    BusID           "PCI:2:0:0"
    Option          "ConstrainCursor"   "off"
EndSection

Section "Device"
    Identifier      "Device0"
    Driver          "nvidia"
    VendorName      "NVIDIA Corporation"
    BoardName       "Quadro K1100M"
EndSection

Section "Screen"
    Identifier      "intel"
    Device          "intel"
EndSection

Section "Screen"
    Identifier      "nvidia"
    Device          "nvidia"
    Option          "AllowEmptyInitialConfiguration"    "on"
    Option          "IgnoreDisplayDevices"  "CRT"
EndSection

Section "Screen"
    Identifier      "Screen0"
    Device          "Device0"
    Monitor         "Monitor0"
    DefaultDepth    24
    SubSection      "Display"
        Depth       24
    EndSubSection
EndSection

Section "InputDevice"
    # generated from default
    Identifier      "Mouse0"
    Driver          "mouse"
    Option          "Protocol"          "auto"
    Option          "Device"            "/dev/psaux"
    Option          "Emulate3Buttons"   "no"
    Option          "ZAxisMapping"      "4 5"
EndSection

Section "InputDevice"
    # generated from default
    Identifier      "Keyboard0"
    Driver          "kbd"
EndSection

Section "Monitor"
    Identifier      "Monitor0"
    VendorName      "Unknown"
    ModelName       "Unknown"
    HorizSync       28.0 - 33.0
    VertRefresh     43.0 - 72.0
    Option          "DPMS"
EndSection

Section "Module"
    Load            "modesetting"
EndSection
```

> Note: sesuaikan tipe NVIDIA yang anda miliki

Buat konfigurasi untuk driver Intel
```
$ sudo nano /etc/X11/xorg.conf.d/20-intel.conf
```

Tambahkan konfigurasi berikut
```
Section "Device"
     Identifier    "Intel Graphics"
     Driver        "intel"
     Option        "AccelMethod"
EndSection
```

Tambahkan mode setting pada `~/.xinitrc`
```
xrandr --setproviderouputsource modesetting NVIDIA-0
xrandr --auto
```

## Konfigurasi Kernel Module
Tambahkan kernel module nvidia pada bagian `MODULES=()`. <sup>[Dokumentasi](https://wiki.archlinux.org/index.php/Mkinitcpio#MODULES)</sup>
```
MODULES=(nvidia nvidia_modeset nvidia_drm nvidia_uvm)
```

Untuk menghindari kemungkinan lupa update pada `initramfs` setelah upgrade driver NVIDIA, anda mungkin ingin menggunakan hook pacman :
```
$ sudo mkdir -p /etc/pacman.d/hooks && nano /etc/pacman.d/hooks/nvidia.hook
```

Tambahkan konfigurasi berikut
```
[Trigger]
Operation=Install
Operation=Upgrade
Operation=Remove
Type=Package
Target=nvidia-dkms

[Action]
Depends=mkinitcpio
When=PostTransaction
Exec=/usr/bin/mkinitcpio -P
```

Tambahkan DRM kernel pada file bootloader `/boot/loader/entries/archlinux.conf`
```
nvidia-drm.modeset=1
```

Update initramfs
```
$ sudo mkinitcpio -p linux
```

Restart komputer
```
sudo reboot
```

# Post-installation
Untuk melihat hasil
```
$ sudo pacman -S nvidia-settings; nvidia-settings
```

![NVIDIA Setting](/images/nvidia-setup-archlinux/nvidia-xsettings.png)

> Note: Jika error  dapat dilihat di log `cat /var/log/Xorg.0.log | grep nvidia`

Untuk melihat informasi NVIDIA yang terinstal dapat menggunakan `nvidia-smi`
```
$ sudo nvidia-smi
```

Kurang lebih outputnya akan seperti berikut
```
Sat Feb  3 06:57:07 2018       
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 387.34                 Driver Version: 387.34                    |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|===============================+======================+======================|
|   0  Quadro K1100M       Off  | 00000000:02:00.0 Off |                  N/A |
| N/A   58C    P5    N/A /  N/A |    394MiB /  2001MiB |      0%      Default |
+-------------------------------+----------------------+----------------------+
                                                                               
+-----------------------------------------------------------------------------+
| Processes:                                                       GPU Memory |
|  GPU       PID   Type   Process name                             Usage      |
|=============================================================================|
|    0       581      G   /usr/lib/xorg-server/Xorg                    201MiB |
|    0       859      G   ...u-secondary-device-ids=0x0416 --gpu-act   120MiB |
|    0      3039      G   ...=0x0416 --service-request-channel-token    69MiB |
+-----------------------------------------------------------------------------+
```

Untuk melihat temperatur GPU
```
$ sudo nvidia-smi -q -d TEMPERATURE
```

Kurang lebih outputnya akan seperti berikut
```
==============NVSMI LOG==============

Timestamp                           : Sat Feb  3 06:56:45 2018
Driver Version                      : 387.34

Attached GPUs                       : 1
GPU 00000000:02:00.0
    Temperature
        GPU Current Temp            : 58 C
        GPU Shutdown Temp           : 105 C
        GPU Slowdown Temp           : 100 C
        GPU Max Operating Temp      : 96 C
        Memory Current Temp         : N/A
        Memory Max Operating Temp   : N/A
```

Referensi :

- [NVIDIA Arch Linux](https://wiki.archlinux.org/index.php/NVIDIA)
- [mkinitcpio](https://wiki.archlinux.org/index.php/Mkinitcpio#MODULES)
- [DKMS](https://wiki.archlinux.org/index.php/Dynamic_Kernel_Module_Support)