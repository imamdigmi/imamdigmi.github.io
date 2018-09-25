---
date: 2016-05-24T14:39:08+07:00
lastmod: 2016-05-24T14:39:08+07:00

title: "Belajar Laravel - Pengertian OOP"
description: "Pengenalan Pemrograman Berorientasi Obyek"
author: "Imam Digmi"

weight: 50
draft: false
keywords: ["php", "oop", "pbo", "laravel"]
tags: ["php", "laravel"]
categories: ["PHP"]
---

Pada artikel [sebelumnya](http://imamdigmi.github.io/post/php-pengenalan-framework/) kita telah membahas sedikit mengenai apa itu framework, namun sebelum kita mulai mendalami framework khususnya framework php yaitu laravel, kita perlu memahami tentang Pemrograman Berorientasi Obyek atau sering disebut OOP.

**Note :**
Jika anda seorang pemula dalam pemrograman saya sarankan anda belajar dulu tentang OOP secara mendalam agar anda dapat dengan mudah menggunakan atau mendalami framework yang akan kita pelajari ini.

## Apa itu OOP?
(Object-oriented programming disingkat OOP) merupakan paradigma pemrograman yang berorientasikan kepada objek. Semua data dan fungsi di dalam paradigma ini dibungkus dalam kelas-kelas atau objek-objek. Bandingkan dengan logika pemrograman terstruktur. Setiap objek dapat menerima pesan, memproses data, dan mengirim pesan ke objek lainnya,
Model data berorientasi objek dikatakan dapat memberi fleksibilitas yang lebih, kemudahan mengubah program, dan digunakan luas dalam teknik piranti lunak skala besar. Lebih jauh lagi, pendukung OOP mengklaim bahwa OOP lebih mudah dipelajari bagi pemula dibanding dengan pendekatan sebelumnya, dan pendekatan OOP lebih mudah dikembangkan dan dirawat [Wikipedia](https://id.wikipedia.org/wiki/Pemrograman_berorientasi_objek).

Nah itu penjelasan singkat mengenai OOP dari wikipedia, lebih simpelnya adalah pemrograman berorientasi obyek ini adalah suatu pendekatan untuk membuat aplikasi yang digunakan oleh laravel, kalau tidak menggunakan OOP akan sangat susah sekali untuk membangun sebuah framework. Mengapa demikian?
Karena dengan menggunakan pendekatan OOP kita bisa dengan mudah menklasifikasi kan obyek-obyek nyata kedalam sebuah class-class yang nantinya kita jadikan class tersebut sebagai cetakan untuk menghasilkan obyek.

## Konsep Dasar OOP
- Class (Kelas) itu kumpulan data atau fungsi-fungsi yang kita buat untuk membuat suatu obyek (Bayangkan: cetakan kue). Contohnya Class dari "Kue" adalah satu unit yang terdiri dari fungsi-fungsi yang bertujuan untuk menjelaskan/menggambarkan pola dari bentuk kue yang ingin kita cetak.

- Object (Obyek) nah kalau Obyek itu hasil dari class yang kita buat, Class itu cetakannya dan Obyek itu hasil dari cetakannya, jika kita membuat cetakan kue maka hasil dari cetakan kue tersebut sudah pasti adalah kue, nah "kue" inilah yang kita sebut sebagai Obyek.

- Abstraction (Abstraksi) ini gunanya untuk menyebunyikan sebuah proses, dalam kenyataannya setelah kita membuat sebuah obyek yaitu "kue" lalu kita ingin menjualnya orang yang akan membeli kue tersebut tidak akan tau bagaimana kue tersebut dibuat, bahannya apa, masaknya gimana dsb.

- Encapsulation (Enkapsulasi) sebenarnya sebuah teknik pembungkusan obyek, setelah kita mencetak "kue" tentu kita perlu membungkus kue tersebut agar tidak terkena debu, air dsb. Gunanya enkapsulasian ini agar obyek telah kita cetak tidak dapat terpengaruh oleh obyek lainnya.

- Polymorphism (Polimorfisme) _poly_ itu banyak dan _morph_ itu bentuk jadi, konsep ini digunakan ketika class-class yang kita buat memiliki prilaku yang sama, contohnya kita membuat class "kue" dan "nasi" nah untuk membuat kue dan nasi ada kemiripan dimana keduanya perlu dimasak walaupun bahan yang dimasak itu berbeda.