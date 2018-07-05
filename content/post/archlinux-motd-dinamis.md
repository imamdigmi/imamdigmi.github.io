---
date: 2018-02-04T15:58:32Z
lastmod: 2018-02-04T15:58:32Z

title: "Archlinux Motd Dinamis"
description: "Menampilkan MOTD secara dinamis di Arch Linux"
author: "Imam Digmi"

weight: 50
draft: true
keywords: [arch linux, linux, motd, tips]
tags: [arch linux]
categories: [Arch Linux]
---

```
$ sudo mkdir /usr/bin/scripts
$ sudo nano /usr/bin/scripts/motd.sh
$ sudo chmod +x /usr/bin/scripts/motd.sh
```

```bash
#!/bin/bash

motd="/etc/motd"

USER=`whoami`
HOSTNAME=`uname -n`
KERNEL=`uname -r`
ARCH=`uname -m`
UPTIME=`uptime -p`
DISK=`df -h / | sed 1d`
BATTERY=`upower --show-info /org/freedesktop/UPower/devices/battery_BAT1 | awk '/percentage/ {print $2}'`

W="\033[01;37m"
B="\033[01;34m"
R="\033[01;31m"
X="\033[00;37m"

clear > $motd

echo -e "$B                                      ##                                          " >> $motd
echo -e "$B                                     ####                                         " >> $motd
echo -e "$B                                    ######                                        " >> $motd
echo -e "$B                                   ########                                       " >> $motd
echo -e "$B                                  ##########                                      " >> $motd
echo -e "$B                                 ############                                     " >> $motd
echo -e "$B                                ##############                                    " >> $motd
echo -e "$B                               ################                                   " >> $motd
echo -e "$B                              ##################                                  " >> $motd
echo -e "$B                             ####################                                 " >> $motd
echo -e "$B                            ######################                                " >> $motd
echo -e "$B                           #########      #########                               " >> $motd
echo -e "$B                          ##########      ##########                              " >> $motd
echo -e "$B                         ###########      ###########                             " >> $motd
echo -e "$B                        ##########          ##########                            " >> $motd
echo -e "$B                       #######                  #######                           " >> $motd
echo -e "$B                      ####                          ####                          " >> $motd
echo -e "$B                     ###                              ###                         " >> $motd
echo -e "               $W Halo $B $USER! $W selamat datang di $B $HOSTNAME                  " >> $motd
echo -e "$R#=============================================================================#   " >> $motd
echo -e "              $R ARCH     $W= $ARCH                                                 " >> $motd
echo -e "              $R KERNEL   $W= $KERNEL                                               " >> $motd
echo -e "              $R UPTIME   $W= $UPTIME                                               " >> $motd
echo -e "              $R DISK     $W= $DISK                                                 " >> $motd
echo -e "              $R BATTERY  $W= $BATTERY                                              " >> $motd
echo -e "$R#=============================================================================#   " >> $motd
echo -e "$X" >> $motd
```

```
$ sudo echo "session    optional   pam_exec.so          stdout /usr/bin/scripts/motd.sh" >> /etc/pam.d/system-login
```

Logout lalu login kembali! enjoy :smile: