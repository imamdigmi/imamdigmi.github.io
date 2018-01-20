---
date: 2017-01-24T18:29:57+07:00
lastmod: 2017-01-24T18:29:57+07:00

title: "Redshift Eye Protection"
description: "Meredupkan Layar Komputer dengan Redshift (Eye Protection)"
author: "Imam Digmi"

weight: 50
draft: false
keywords: ["redshift", "eye protection"]
tags: ["tips"]
categories: ["Tips"]
---

Aplikasi RedShift ini berfungsi untuk meredamkan layar monitor agar tidak terlalu kontras, sehingga mata kita tidak cepat sakit ketika menatap layar monitor terlalu lama, oke langsung saja kita install.

## Ubuntu
```bash
sudo apt-get install redshift
```

## Archlinux
```bash
sudo pacman -S redshift
```

## Start Redshift
Untuk menjalankan RedShift secara default perintahnya `redshift -l LAT:LON -t DAY:NIGHT [OPTIONS...]` contoh :
```bash
redshift -l -7.797068:110.370529 -t 5700:3600
```

## File Konfigurasi
Kita juga bisa menambahkan configuration file untuk redshift, namun kita perlu membuat konfigurasinya secara manual di ~/.config/redshift.conf. Berikut konfigurasi yang penulis gunakan:
```
[redshift]
temp-day=5000
temp-night=4700
transition=1
gamma=0.8
location-provider=manual
adjustment-method=randr

[manual]
lat=-6.99
lon=110.42

[randr]
screen=0
```

Untuk color temperature bisa juga di-set secara permanen via terminal:
```
redshift -O 3700
```
Untuk mereset temperature menjadi netral:
```
redshift -x
```
Sekian, semoga bermanfaat
