---
date: 2018-01-20T11:13:39+07:00
lastmod: 2018-01-20T11:13:39+07:00

title: "Pemutar musik dengan MPD dan NCMPCPP"
description: "Memutar musik dengan server MPD dan NCMPCPP sebagai interface"
author: "Imam Digmi"

weight: 50
draft: true
keywords: ["mpd", "music", "ncmpcpp", "arch", "linux"]
tags: ["tips"]
categories: ["Tips"]

comment: true
toc: true
autoCollapseToc: true
contentCopyright: <a rel="license" href="http://creativecommons.org/licenses/by-nc-nd/4.0/">CC BY-NC-ND 4.0</a>
reward: false
mathjax: false
mathjaxEnableSingleDollar: false
---

MPD atau Music Player Daemon ini adalah sebuah server daemon yang gunanya untuk memutar music dan NCMPCPP adalah client atau sebuah aplikasi yang digunakan untuk manajemen file musik dan sebagainya, dan kelebihan dari tool ini dapat mengkonfigurasi nya sesuai dengan yang kita inginkan seperti tampilan menu, daftar lagu, visualizer dan sebagainya.<!--more-->

![Figure 1](/images/pemutar-musik-dengan-ncmpcpp/1.png)

Langkah instalasinya akan kita bagi menjadi dua yaitu :
1. Setup MPD (Music Player Daemon)
2. Setup NCMPCPP (Client untuk MPD)
3. Setup Cava (Music visualizer external)

## Install MPD
```
// Arch Linux
$ sudo pacman -S mpd

// Ubuntu / Debian
$ sudo apt-get install mpd
```

Agar MPD dapat memutar ulang audio, [ALSA](https://wiki.archlinux.org/index.php/ALSA) atau [OSS](https://wiki.archlinux.org/index.php/OSS) (opsional dengan [PulseAudio](https://wiki.archlinux.org/index.php/PulseAudio)) perlu disiapkan konfigurasinya.
MPD dikonfigurasi di dalam sebuah file yaitu `mpd.conf`. Lokasi file ini tergantung bagaimana anda ingin menjalankan MPD (lihat bagian di bawah). Ini adalah opsi konfigurasi yang umum digunakan :

- `pid_file` - File dimana MPD menyimpan ID prosesnya
- `db_file` - Database musik
- `state_file` - Kondisi saat ini untuk MPD tercatat pada file ini
- `playlist_directory` - Folder untuk menaruh playlist lagu kita
- `music_directory` - Folder yang digunakan MPD untuk me
- `sticker_file` - Stiker database

### Konfigurasi
MPD dapat dikonfigurasi perpengguna dan juga secara _system-wide (global)_ pada bahasan ini kita hanya akan menerapkan untuk konfigurasi perpengguna karena lebih baik dan menjalankan MPD sebagai pengguna biasa (bukan root) memiliki keunggulan yaitu :

- Direktori tunggal `~/.config/mpd/` (atau direktori lainnya di bawah `$HOME`) yang akan berisi semua file konfigurasi MPD.
- Lebih mudah menghindari kesalahan izin read/write yang tak terduga.

Sebaiknya kita membuat satu direktori untuk file dan daftar putar yang dibutuhkan misal `~/.config/mpd/` atau `~/.mpd/`. MPD mencari file konfigurasi di `$XDG_CONFIG_HOME/mpd/mpd.conf` dan kemudian `~/.mpdconf`. Ini memungkinkan kita untuk melewati jalur lain sebagai argumen baris perintah tanpa harus menyebutkan file konfigurasi yang dimaksudkan pada saat kita hendak menjalankan MPD.
```
$ mkdir -p ~/.config/mpd
$ ~/.config/mpd/mpd.conf
```
Berikut konfigurasi MPD yang penulis gunakan :

```
bind_to_address     "localhost"
port                "6600"
log_level           "default"

input {
  plugin            "curl"
}

audio_output {
  type              "pulse"
  name              "My Pulse Output"
  options           "dev=dmixer"
}

audio_output {
  type              "fifo"
  name              "My fifo output"
  path              "/tmp/mpd.fifo"
  format            "44100:16:2"
}

db_file             "~/.config/mpd/database"
log_file            "~/.config/mpd/log"

music_directory     "~/Music"
playlist_directory  "~/.config/mpd/playlists"
pid_file            "~/.config/mpd/pid"
state_file          "~/.config/mpd/state"
sticker_file        "~/.config/mpd/sticker.sql"
```
Setelah itu kita buat direktori playlists nya :
```
$ mkdir ~/.config/mpd/playlists
```
Lalu kita coba jalankan MPD nya :
```
$ mpd
```
Coba kita lihat juga apakah MPD sudah listening :
```
$ netstat -tulpen
```
Jika terdapat MPD yang sedang LISTEN seperti dibawah ini brarti MPD sudah berhasil kita pasang
```
(Not all processes could be identified, non-owned process info will not be shown, you would have to be root to see it all.)
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       User       Inode      PID/Program name    
tcp        0      0 127.0.0.1:6600          0.0.0.0:*               LISTEN      1000       497923     7624/mpd            
tcp6       0      0 ::1:6600                :::*                    LISTEN      1000       497922     7624/mpd            
tcp6       0      0 :::80                   :::*                    LISTEN      0          13954      -                   
....
```

## Install NCMPCPP
Setelah instalasi MPD berhasil dan dikonfigurasikan dengan baik maka kita lanjutkan dengan langkah instalasi NCMPCPP sebagai client untuk pemutar musik kita.
```
// Arch Linux
$ sudo pacman -S ncmpcpp

// Ubuntu / Debian
$ sudo apt-get install ncmpcpp
```
Setelah instalasi selesai file konfigurasi ncmpcpp tidak otomatis dibuat maka kita perlu menambahkannya sendiri yaitu `~/.ncmpcpp/config`. Konfigurasi untuk NCMPCPP sangatlah mudah dan dapat kita kreasikan sesuai selera, agar lebih mudah cukup salin dan ubah sesuai kebutuhan saja dari konfigurasi yang penulis buat, berikut konfigurasi yang penulis gunakan :

{% gist 81aa7f0cb06275034492ed11421cf06e ncmpcpp.conf %}

Lalu kita jalankan ncmpcpp nya :
```
$ ncmpcpp
```
> sebelum kita menjalankan ncmpcpp pastikan mpd sudah running

## Shortcut untuk tampilan
- `1` - Menampilkan playlist saat ini
- `2` - Filesystem browser
- `3` - Pencarian dalam databases
- `4` - Pustaka musik
- `5` - Playlist editor
- `6` - Tag editor
- `7` - Output selector
- `8` - Music visualizer
- `=` - Jam dinding
- `F1` - Help

## UI Shortcut lainnya
- `q` - Keluar
- `f` - Seek forward
- `b` - Seek backward
- `\` - Mengganti tampilan klasik/alternatif
- `#` - Menampilkan bitrate pada lagu
- `i` - Menampilkan info lagu
- `I` - Menampilkan info artis (tersimpan pada file `~/.ncmpcpp/artists/ARTIST.txt`)
- `l` - Menampilkan lirik untuk lagu yang sedang diputar
- `L` - Mencari lirik lagi yang terdapat di database
- `>` - Lagu selanjutnya
- `<` - Lalu sebelumnya
- `p` - Play/Pause
- `+` - Menambahkan volume
- `-` - Mengurangi volume

## Instal Cava
Cava adalah visualizer berbasis konsol untuk ALSA audio namun dapat digunakan juga untuk audio lainnya seperti PulseAudio dan lain-lai, cava dapat kita gunakan ketika kita menggunakan MPD, cava bisa didapatkan di [AUR](https://aur.archlinux.org/packages/cava/) dan untuk instalasi cukup mudah hanya.
```
$ yaourt -S cava
```
Lalu kita jalankan cava
```
$ cava
```

If everything have done, berhasil terpasang dan terkonfigurasi dengan baik maka kurang lebih tampilannya akan seperti diatas.