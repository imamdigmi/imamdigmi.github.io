---
date: 2017-01-15T17:47:17+07:00
lastmod: 2017-01-15T17:47:17+07:00

title: "Instalasi Golang"
description: "Instalasi Bahasa Pemrograman Go"
author: "Imam Digmi"

weight: 50
draft: false
keywords: ["go", "golang", "install", "setup"]
tags: ["golang"]
categories: ["GoLang"]

comment: true
toc: true
autoCollapseToc: true
contentCopyright: <a rel="license" href="http://creativecommons.org/licenses/by-nc-nd/4.0/">CC BY-NC-ND 4.0</a>
reward: false
mathjax: false
mathjaxEnableSingleDollar: false
---

Pada artikel kali ini kita akan membahas cara meng-install golang (Google Go Language) di Linux, disini Linux yang penulis gunakan adalah :<!--more-->
```bash
No LSB modules are available.
Distributor ID: Ubuntu
Description:    Ubuntu 16.10
Release:        16.10
Codename:       yakkety
```
## Download Golang

Sebelum kita meng-install golang kita harus mendownload aplikasi golang terlebih dahulu [disini](https://golang.org/dl/) lalu klik pada bagian Linux dan jangan lupa pilih jenis bit komputer anda.
> NOTE: Pada artikel ini ditulis versi stabil dari golang yaitu 1.7.4 dan kita akan menggunakan versi ini.

![Download](/instalasi-golang/download.png)

Walaupun sebenarnya ada banyak versi yang di sediakan oleh golang termasuk unstable version, tapi lebih baik kita download yang stable version saja agar tidak ada masalah nantinya ketika kita bermain dengan golang.
```bash
ls -la
total 447412
drwxrwxr-x  3 idiot idiot      4096 Jan 15 00:18 .
drwxr-xr-x 63 idiot idiot      4096 Jan 15 00:31 ..
-rw-r--r--  1 idiot idiot  84021919 Jan  7 16:26 go1.7.4.linux-amd64.tar.gz
-rw-r--r--  1 idiot idiot  76419072 Jan 14 23:52 go1.7.4.windows-amd64.msi
```
## Ekstrak file

Buka terminal lalu extract file hasil download tadi dan pindahkan hasil extract ke direktori `/usr/local` :
```bash
sudo tar zxvf go1.7.4.linux-amd64.tar.gz -C /usr/local
```

## Export path go

untuk meng-eksport path lokasi bisa langsung dengan perintah `export PATH=$PATH:/usr/local/go/bin` di terminal atau bisa juga menaruh perintah tersebut di dalam file `$HOME/.profile` dan penulis sendiri menggunakan cara yang kedua :
```bash
echo "export PATH=\$PATH:/usr/local/go/bin" >> ~/.profile
```

## Selesai

Jika langkah diatas dilakukan dengan benar maka go sudah terinstall di komputer kita, selanjutnya lihat versi go yang barusan kita install :
```bash
go version

// Output:
go version go1.7.4 linux/amd64
```

## Workspace

Setelah Golang berhasil di-instal, ada hal yang perlu disiapkan sebelum bisa masuk ke sesi pembuatan aplikasi, yaitu setup workspace untuk proyek-proyek yang akan dibuat. Kita diharuskan mempunyai 3 folder didalam direktori workspace kita yaitu :
```bash
Workspace
├── bin
├── pkg
└── src
```
Struktur diatas adalah struktur standar workspace Golang jadi pastikan penamaannya harus sama, lokasi folder yang dijadikan workspace boleh dimana saja dan lokasi tersebut harus di daftarkan kedalam `PATH Variable` dengan nama `GOPATH`
- `src` lokasi dimana proyek golang disimpan
- `pkg` berisi file hasil kompilasi
- `bin` berisi file executable hasil build

Daftarkan `GOPATH` kedalam `PATH Variable` :
> NOTE: Ganti < **workspace** > sesuai dengan direktori workspace golang di komputer anda.

```bash
echo "export GOPATH=\"\$HOME/< workspace >\"" >> ~/.profile
```
Setelah itu cek apakah GOPATH sudah terdaftar atau belum
```bash

// Output:
/home/idiot/Workspace/golang
```
> TIP: Jika $GOPATH tidak dikenali cobalah untuk menutup lalu membuka kembali terminal anda

```bash
tree -L 2 $GOPATH

// Output:
/home/idiot/Workspace/golang
├── bin
│   ├── gocode
│   ├── godep
│   ├── gogetdoc
│   ├── goimports
│   ├── golint
│   ├── gometalinter
│   ├── gotype
├── pkg
│   └── linux_amd64
└── src
    ├── github.com
    ├── golang.org
    └── gopkg.in

7 directories, 28 files
```

## Best Practice

Golang memang sudah terinstall tapi _envirounment_ nya masih belum friendly, berdasarkan rekomendasi dari [sini](https://golang.org/doc/code.html) dan best practice nya [disini](https://peter.bourgon.org/go-best-practices-2016/) maka kita harus membuat PATH lagi untuk workspace kita :
```bash
echo "export GOROOT=\"/usr/local/go\"" >> ~/.profile
echo "export PATH=\"\$GOPATH/bin:\$(go env GOROOT)/bin:\$PATH\"" >> ~/.profile
```
> TIP: Langkah diatas kita lalukan agar easily accessible ketika kita hendak mengakses file binari yang kita install dengan perintah `go install` pada proyek kita.

In the end, kumpulan `PATH` yang kita taruh pada file .profile adalah sebagai berikut :
![Profile File](/instalasi-golang/profile-file.png)

Setelah itu mari kita lihat go envirounment yang sudah kita buat :
```bash
go env

// Output:
GOARCH="amd64"
GOBIN="/usr/local/go/bin"
GOEXE=""
GOHOSTARCH="amd64"
GOHOSTOS="linux"
GOOS="linux"
GOPATH="/home/idiot/Workspace/golang"
GORACE=""
GOROOT="/usr/local/go"
GOTOOLDIR="/usr/local/go/pkg/tool/linux_amd64"
CC="gcc"
GOGCCFLAGS="-fPIC -m64 -pthread -fmessage-length=0 -fdebug-prefix-map=/tmp/go-build688917244=/tmp/go-build -gno-record-gcc-switches"
CXX="g++"
CGO_ENABLED="1"go env
```

## Menguji Instalasi Go

Setelah semua nya selesai mari kita uji instalasi go dengan membuat program sederhana :
```bash
mkdir -p $GOPATH/src/github.com/imamdigmi/hello
touch $GOPATH/src/github.com/imamdigmi/hello/main.go
```
Isikan file `main.go` dengan :
```go
package main
import "fmt"
func main() {
    fmt.Println("Halo Golang <3!")
}
```
Jalankan program yang kita buat tadi :
```bash
go run main.go
```
![Test Installation](/instalasi-golang/test-installation.png)

Oke sampai disini dulu, semoga bermanfaat {%gemoji smile%}
