---
date: 2017-02-26T12:16:03+07:00
lastmod: 2017-02-26T12:16:03+07:00

title: "Belajar Rust - Setup"
description: "Instalasi Rust"
author: "Imam Digmi"

weight: 50
draft: false
keywords: ["install", "setup", "rust"]
tags: ["rust"]
categories: ["Rust"]
---

Rust adalah bahasa pemrograman yang terbilang baru yang pembuatannya dimulai pada tahun 2006 oleh developer Mozilla. Rust adalah bahasa pemrograman sistem (system programming) yang difokuskan pada tiga tujuan: keamanan, kecepatan, dan concurrency. Ia memelihara tujuan ini tanpa _garbage collector_ yang membuatnya menjadi bahasa yang berguna untuk sejumlah kasus penggunaan bahasa lain. Tidak hanya itu rust juga bahasa dengan general purpose language, bahasa low level yang bisa membantu kita untuk mengeksplorasi potensi sisi system, embedded system, dan hal-hal kritis terkait performance. Salah satu aplikasi menarik yang dibuat dengan rust programming language adalah pembuatan servo sebuah parallel browser engine [https://servo.org/](https://servo.org/)

## Pembahasan
1. Install rust
2. Komponen tambahan
3. Pustaka tambahan

## Install rust
```bash
curl https://sh.rustup.rs -sSf | sh
```
Output
```bash
info: downloading installer

Welcome to Rust!

This will download and install the official compiler for the Rust programming
language, and its package manager, Cargo.

It will add the cargo, rustc, rustup and other commands to Cargo's bin

directory, located at:

  /home/idiot/.cargo/bin

This path will then be added to your PATH environment variable by modifying the
profile file located at:

  /home/idiot/.profile

You can uninstall at any time with rustup self uninstall and these changes will
be reverted.

Current installation options:

   default host triple: x86_64-unknown-linux-gnu
     default toolchain: stable
  modify PATH variable: yes

1) Proceed with installation (default)
2) Customize installation
3) Cancel installation
```

Ketik **1** lalu enter, maka outputnya :
```bash
info: syncing channel updates for 'stable-x86_64-unknown-linux-gnu'
info: downloading component 'rustc'
 35.9 MiB /  35.9 MiB (100 %)   1.2 MiB/s ETA:   0 s

info: downloading component 'rust-std'
 49.2 MiB /  49.2 MiB (100 %)   1.2 MiB/s ETA:   0 s
info: downloading component 'cargo'
  4.4 MiB /   4.4 MiB (100 %)   1.2 MiB/s ETA:   0 s
info: installing component 'rustc'
info: installing component 'rust-std'
info: installing component 'cargo'
info: default toolchain set to 'stable'

  stable installed - rustc 1.15.1 (021bd294c 2017-02-08)


Rust is installed now. Great!

To get started you need Cargo's bin directory in your PATH environment
variable. Next time you log in this will be done automatically.

To configure your current shell run source $HOME/.cargo/env
```
Instalasi rust sudah berhasil, periksa instalasi :

```bash
rustc --version

// Output
rustc 1.15.1 (021bd294c 2017-02-08)
```

Setelah itu kita tambahkan **PATH** environment variable untuk Cargo :
```bash
echo "export PATH=$HOME/.cargo/bin:$PATH" >> ~/.bashrc
```

### Versi Nightly
By default, ketika kita memasang Rust maka yang terpasang adalah rust versi stabil (stable) namun dalam kasus lain misalkan kita ingin menggunakan [Rocket Web Framework](http://rocket.rs) maka kita diharuskan untuk menggunakan versi Nightly (baca: [nightly build vs stable](http://superuser.com/a/330447)) untuk mengganti versi dari stabil ke nighly caranya :
```bash
rustup default nightly
```
Atau bisa juga hanya dalam satu proyek saja :
```bash
cd /path/to/project/
rustup override set nightly
```

## Komponen Tambahan
```bash
rustup component add rust-src

// Output
nfo: downloading component 'rust-src'
 27.1 MiB /  27.1 MiB (100 %)   1.2 MiB/s ETA:   0 s
info: installing component 'rust-src'
```

## Pustaka Tambahan
Racer
```bash
cargo install racer
```
Periksa instalasi Racer :
```bash
racer complete std::io::B
// Output
MATCH BufRead,1208,10,/home/idiot/.rustup/toolchains/stable-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/src/libstd/io/mod.rs,Trait,pub trait BufRead: Read
MATCH Bytes,1605,11,/home/idiot/.rustup/toolchains/stable-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/src/libstd/io/mod.rs,Struct,pub struct Bytes<R>
MATCH BufReader,50,11,/home/idiot/.rustup/toolchains/stable-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/src/libstd/io/buffered.rs,Struct,pub struct BufReader<R>
MATCH BufWriter,309,11,/home/idiot/.rustup/toolchains/stable-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/src/libstd/io/buffered.rs,Struct,pub struct BufWriter<W: Write>
```
Rust Formater
```bash
cargo install rustfmt
```
Rust Symbol
```bash
cargo install rustsym
```

And then?
You can playing with rust directly in [Rust Playground](https://play.rust-lang.org/) or read [Official Book](https://doc.rust-lang.org/stable/book/) biar lebih Rock N Roll!

Terimakasih, semoga bermanfaat :smile:.