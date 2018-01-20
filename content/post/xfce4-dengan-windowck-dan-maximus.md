---
date: 2017-04-29T18:56:51+07:00
lastmod: 2017-04-29T18:56:51+07:00

title: "Xfce4 dengan Windowck dan Maximus"
description: "Membuat tampilan layar lebih fit dengan Windowck dan Maximus di XFCE4"
author: "Imam Digmi"

weight: 50
draft: false
keywords: ["xfce", "xfce4", "fit window", "windowck", "maximus", "linux"]
tags: ["tips"]
categories: ["Tips"]
---

Pada pembahasan ini kita akan mendekorasi tampilan XFCE4 dan memaksimalkan ruang layar komputer agar lebih fit mirip seperti Unity pada Desktop Ubuntu, untuk itu kita perlu memasang 3 plugin utama yaitu :

1. Windowck - Untuk menampilkan tombol button pada panel
2. Maximus - Untuk menghilangkan border window
3. AppMenu - Untuk menampilkan window menu pada panel

## Setup Windowck
[Windowck](http://goodies.xfce.org/projects/panel-plugins/xfce4-windowck-plugin) adalah plugin untuk XFCE yang berfungsi untuk menempatkan button window (close, maximize, minimize) pada saat window di-maximize, mirip seperti default desktop envirounment ubuntu yaitu unity. Ini berguna untuk meningkatkan ruang layar vertikal komputer kita.

Plugin ini bisa kita dapatkan di [AUR](https://aur.archlinux.org/packages/xfce4-windowck-plugin/) dan instalasinya sangatlah mudah sekali.
```
$ yaourt -S windowck
```
Setelah instalasi selesai aktifkan plugin windowck dengan cara klik kanan pada panel `Panel > Add New Items`

![Figure 1](/images/xfce4-dengan-windowck-dan-maximus/1.png)

Untuk mengubah pengaturan windowck seperti style button dan lain-lain bisa dengan cara klik kanan pada panel tepat pada bagian plugin lalu pilih `Properties`

![Figure 2](/images/xfce4-dengan-windowck-dan-maximus/2.png)

## Setup Maximus
Maximus digunakan untuk menghilangkan window border pada saat window di-maximize sehingga layar komputer kita menjadi lebih fit.
Plugin Maximus bisa kita dapatkan di [AUR](https://aur.archlinux.org/packages/maximus/) dan instalasinya mudah sekali.
```
$ yaourt -S maximus
```
Setelah instalasi selesai kita konfigurasikan maximus agar dapat bekerja, dengan cara masuk ke menu `Settings Manager > Window Manager Tweaks > Accessibility` lalu centang pada bagian `Hide title of window  when maximized`.

![Figure 3](/images/xfce4-dengan-windowck-dan-maximus/3.png)

## Setup Global Menu
GlobalMenu adalah plugin yang berfungsi untuk menampilkan menu pada window tersebut ke panel sehingga window yang sedang kita buka benar-benar fit persis seperti unity pada Ubuntu. Plugin GlobalMenu sebenarnya banyak variasi nya tapi pada pembahasan ini kita menggunakan [Vala AppMenu](https://github.com/rilian-la-te/vala-panel-appmenu) dan plugin ini bisa kita dapatkan di [AUR](https://aur.archlinux.org/packages/vala-panel-appmenu-xfce-git/) dan instalasinya pun mudah juga.
```
$ yaourt -S vala-panel-appmenu-xfce-git
```
Setelah instalasi selesai kita pun perlu menambahkan item plugin AppMenu di panel dengan cara, klik kanan pada panel `Panel > Add New Items`.

![Figure 4](/images/xfce4-dengan-windowck-dan-maximus/4.png)

> Beberapa aplikasi seperti Android Studio dengan menu window yang tertanam pada window tidak dapat dialihkan ke panel.

Jika instalasi berjalan lancar dan di konfigurasikan dengan baik maka tampilan layar akan menjadi seperi ini

![Figure 5](/images/xfce4-dengan-windowck-dan-maximus/5.png)