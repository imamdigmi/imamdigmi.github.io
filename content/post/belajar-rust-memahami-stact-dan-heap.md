---
date: 2017-03-17T22:41:07+07:00
lastmod: 2017-03-17T22:41:07+07:00

title: "Belajar Rust - Memahami Stact Dan Heap"
description: "Memahami Stack dan Heap di Rust"
author: "Imam Digmi"

weight: 50
draft: false
keywords: ["rust", "stack", "heap"]
tags: ["rust"]
categories: ["Rust"]
---

Jika anda adalah seorang programmer yang berkecimpung di _low-level_ atau _systems programming_ pastinya sudah tidak asing lagi dengan konsep ini dan tentunya sangatlah mengerti tentang pentingnya mengelola _resource_ atau sumber daya memori. Dalam kompilasi sebuah program diciptakan dengan sebuah struktur. Mungkin secara spesifik struktur untuk masing-masing _operating system_ berbeda tapi secara garis besar memiliki beberapa persamaan yaitu pengalamatan memori.

Pada saat program berjalan (runnig), sebuah program akan menempati lokasi tertentu pada memori. Lokasi ini kemudian dikenal dengan alamat memori. Saat program berjalan, memori akan dibagi menjadi tiga bagian atau tiga segment. Ketiga segment itu adalah:

1. text (code)
2. Stack
3. Heap

## Apa itu Segment text?
Sering juga disebut sebagai code segment yaitu tempat kode dari program tersebut (basic instruction) berada. Contohnya saat anda membuka file binari di text editor, anda akan menjumpai simbol-simbol yang terkadang terbaca atau tidak dimengerti oleh manusia. Bagian ini merupakan kode-kode bahasa mesin, representasi dari instruksi program. Bagian ini menyertakan semua simbol-simbol yang didefinisikan pengguna ketika menciptakan program (contoh: fungsi-fungsi yang didefinisikan).
Nah, kedua bagian selain code segment digunakan untuk menyimpan data yang akan dioperasikan oleh fungsi-fungsi tersebut. Lantas apa perbedaan antara keduanya (_stack_ dan _heap_) ?

## Apa itu Stack?
Stack adalah salah satu bagian yang digunakan untuk menyimpan data-data atau variabel yang pengalamatan memorinya telah dilakukan pada saat kompilasi (alamat pastinya sudah ditentukan dari awal). Bingung? Coba bayangkan seperti ini. Pada suatu waktu kita akan memasak. Dalam kegiatan memasak itu terdapat daftar barang-barang yang dibutuhkan alat-alat (misal: pisau, wajan, panci, dsb) dan bahan-bahan (misal: ayam, telur, minyak goreng, dsb). Ketika kita hendak memasak pasti kita membutuhkan tempat khusus bukan? Semisal kita memasak di dapur, maka kita harus menyiapkan tempat untuk menaruh semua alat dan bahan tersebut sebelum melakukan kegiatan masak-memasak. Dalam bahasa pemprograman, dapat diilustrasikan bahwa kegiatan memasak tersebut disebut fungsi (melakukan kerja). Di awal sebelum memasak kita telah membuat daftar apa yang harus kita lakukan. Fungsi tersebut membutuhkan variabel-variabel atau data (alat dan bahan). Untuk memasak kita butuh tempat atau bagian dari dapur untuk menaruh alat dan bahan. Tempat yang kita persiapkan itulah yang disebut _stack_, _stack_ ditentukan sejak awal-awal.

Dalam _stack_, data disimpan menggunakan metode Last In First Out (LIFO). Stack artinya tumpukan, maksudnya data yang tersimpan di memori akan di-alokasikan dan di-dealokasikan hanya di akhir memori (dinamakan puncak _stack_). Hal ini bisa kita analogikan seperti saat kita menumpuk piring, pada saat kita menumpuk piring kita akan meletakkannya dalam satu, piring yang pertama kali kita taruh pasti letaknya di bawah dan pasti akan kita ambil terakhir kali karena kita ngambilnya dari atas (ingat konsep LIFO). Alokasi semacam inilah yang diimplementasikan dalam memori komputer. Stack adalah bagian memori yang fungsinya digunakan untuk menyimpan memori yang bersifat sementara.

## Apa itu heap?
Selain konsep stack ada juga _heap_, adalah area memori yang digunakan untuk alokasi secara dinamis. Bagian-bagian memori yang dialokasikan dilakukan secara sembarang atau tanpa pola awal (no pattern). artinya kode-kode yang akan dieksekusi akan diletakkan pada lokasi penyimpanan dalam memori, namun lokasi ini tidak memiliki pattern atau tidak berpola (acak). Lokasi memori yang ditempati ini tidak akan diketahui sebelum runtime (saat dijalankan). Heap seringkali digunakan oleh program untuk berbagai keperluan, tetapi intinya adalah _heap_ dialokasikan untuk mensuplai memori tambahan yang tidak dialokasikan saat kompilasi. Alokasi ini dilakukan saat runtime, seiring berjalannya program.

Untuk memesan alamat memori, program akan terlebih dahulu melakukan pengecekan terhadap kapasitas memori sesuai besar data yang diperlukan. Jika memori tersedia namun dalam ruang terpisah maka program akan memecah informasi tersebut menjadi bagian-bagian kecil yang dapat menempati memori yang tersedia.

## Lalu apa perbedaan Stack dan Heap?
Dibandingkan dengan heap, stack terlihat lebih banyak digunakan dalam kaitannya untuk mengalokasikan memori bagi variabel-variabel yang digunakan didalam fungsi dan stack digunakan untuk menyimpan data yang sifatnya "sementara". Sementara heap diperuntukkan bagi informasi yang menetap dan dialokasikan secara khusus oleh pengguna, heap digunakan untuk menyimpan data tambahan sesuai permintaan (request data).

Karena memori yang dipakai oleh stack bersifat sementara, maka informasi yang terdapat di alamat stack akan langsung didealokasi secara otomatis ketika scope (ruang lingkup) sebuah program sudah berakhir ini disebut _lifetimes_, sedangkan memori yang dialokasikan pada heap akan tetap bertahan sampai program benar-benar mendealokasikannya.

## Mengapa stack dan heap begitu penting?
Ketika program diload ke memori, dia akan membutuhkan penanganan memori. Jika manajemen memori tidak ada di memori komputer, program akan clash karena data akan saling bertabrakan tak teratur (saling serobot tempat)