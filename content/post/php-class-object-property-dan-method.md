---
date: 2016-05-24T15:41:07+07:00
lastmod: 2016-05-24T15:41:07+07:00

title: "Belajar Laravel - Class, Object, Property dan Method"
description: "Memahami Class, Object, Property dan Method di PHP"
author: "Imam Digmi"

weight: 50
draft: false
keywords: ["php", "class", "object", "property", "method", "oop", "laravel"]
tags: ["php", "laravel"]
categories: [PHP]
---

Pada artikel [sebelumnya](http://imamdigmi.github.io/post/php-pengertian-oop/) kita telah membahas pengetian OOP dan bagaimana konsep dasar dari pendekatan OOP itu sendiri, nah sekarang mari kita praktekan bagaimana membuat Class, Object, Property dan Method.

## Class
Kita akan mulai dengan membuat Class terlebih dulu, Untuk membuat Class, penulisannya harus diawali dengan keyword **class** kemudian diikuti dengan nama Class, namun ada aturan penamaan dalam penulisan nama Class yaitu diawali dengan huruf _kapital_ dan jika nama class tersebut lebih dari dua atau lebih maka kata selanjutnya disambung dan diawali dengan huruf _kapital_ juga contohnya "**KueDonat**" penamaan seperti ini disebut dengan (Camel Case).

berikut contoh kode sederhana pemembuatan Class :
``` php
<?php
class Kue {
    // isi dengan perintah...
}
```

## Property
Property atau juga disebut dengan atribut adalah data yang ada di dalam Class itu sendiri, sebenarnya property sama dengan variable tapi didalam OOP disebut property. Aturan penulisan nya pun sama dengan variable bisa langsung di definisikan bisa juga diawali dengan keyword **var** dan bisa juga langsung di berikan nilai.

Berikut contoh pembuatan property lanjutan dari Class Kue :
``` php
<?php
class Kue {
    var $bahan = ['Tepung', 'Gula', 'Telur'];
    var $bentuk;
}
```

## Method
Method pada dasarnya adalah fungsi (function) didalam sebuah Class yang berguna untuk melakukan suatu perintah-perintah, function juga bisa menerima parameter atau argumen dan mengembalikan sebuah nilai dengan keyword **return**. Berbeda dengan Class, aturan penamaan sebuah method sebaiknya diawali dengan huruf kecil tapi jika ada lebih dari 1 kata maka bisa disambign dengan _underscore_ (Snake Case) seperti **memasak_kue** atau dengan disambung huruf kapital di awal kata (Camel Case) seperti **memasakKue**.

Berikut contoh sederhana pembuatan method :
``` php
<?php
class Kue {
    var $bahan = ['Tepung', 'Gula', 'Telur'];
    var $bentuk;

    function memasak_kue ([parameter]) {
        // isi dengan perintah...
    }

    function membungkusKue ([parameter]) {
        // isi dengan perintah...
    }
}
```

## Object
Object (obyek) adalah hasil/cetakan atau hasil "_konkret_" dari sebuah Class, jika diatas kita membuatan Class **Kue** maka object bisa *kue_bolu*, *kue_donat*, *kue_gethuk* dan lain-lain. Dan tentunya object-object tersebut akan memiliki ciri-ciri dari Class pembuatnya yaitu sebuah Kue, yang dihasilkan dari property-property dan method-methodnya.
Untuk membuat sebuah object harus kita awali dengan keyword **new** lalu diikuti dengan nama Classnya.

Berikut contoh pembuatan object dari Class Kue tadi :
``` php
<?php
class Kue {
    var $bahan = ['Tepung', 'Gula', 'Telur'];
    var $bentuk;

    function memasak_kue ([parameter]) {
        // isi dengan perintah...
    }

    function membungkusKue ([parameter]) {
        // isi dengan perintah...
    }
}

$kue_bolu = new Kue();
$kue_donat = new Kue();
$kue_gethuk = new Kue();
```