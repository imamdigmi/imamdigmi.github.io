---
date: 2016-05-25T16:19:22+07:00
lastmod: 2016-05-25T16:19:22+07:00

title: "Belajar Laravel - Abstract dan Interface"
description: "Memahami abstract dan interface di PHP"
author: "Imam Digmi"

weight: 50
draft: false
keywords: ["php", "oop", "pbo", "abstract", "interface", "laravel"]
tags: ["php", "laravel"]
categories: ["PHP"]
---

Pada artikel [sebelumnya](http://imamdigmi.github.io/post/php-class-object-property-dan-method/) kita telah membahas bagaimana membuat Class, Object, Property dan Meethod. Untuk mendalami framwork laravel sangat penting sekali bagi kita untuk memahami tentang Abstract dan Interface, jika Class adalah nasi dan gorengan tempe maka abstract dan interface adalah lalapan dan sambalnya, hmmm jadi inget masakan ibu :sweat_smile:

## Abstract
Ukee, mari kita mulai dengan abstract. Sebenarnya apasih abstract itu?

**Abstract adalah sebuah class yang tidak bisa "diinstansiasi" atau tidak bisa dijadikan object secara langsung** (Wikipedia).

Abstract mempunyai peran sebagai kerangka dasar untuk Class turunannya dan untuk membuat stuktur logika penurunan, dan tentunya Abstract digunakan dalam pewarisan (Inheritance) Class untuk implementasi method-method yang sama bagi seluruh Class turunan dari abstract itu sendiri. Saya rasa cukup pembahasan teorinya, kita langsung ke TKP saja.
Skenario sederhananya, misalkan kita ingin membuat class yang terdiri dari berbagai jenis kue seperti class Kue, kita semua tau bahwa kue bisa mempunyai rasa, tapi setiap kue pasti mempunyai rasa yang berbeda tergantung jenis kuenya. Perhtikan contoh berikut :
```php
<?php
abstract class Kue {
    abstract function rasa ();
}
```
```php
<?php
include "Kue.php";
$kue = new Kue();
$kue->rasa();
```
Nah dari code diatas kita mencoba untuk membuat object (instance) dari class abstract Kue. Dan ternyata ketika dijalankan malah error seperti ini :

![Abstract Error](/images/php-abstract-dan-interface/abstract-error.png)

Screenshot diatas menunjukan bahwa class abstact tidak bisa langsung dijadikan object (instantiate). Bagaimana untuk membuat objectnya?, kita perlu membuat class baru yang meng-extends class abstract tersebut. Contohnya seperti ini :
``` php
<?php
include "Kue.php";
class KueDonat extends Kue {
    // ...
}
```
``` php
<?php
include "KueDonat.php";
$kue = new KueDonat();
$kue->rasa();
```
Mari kita jalankan...

![Abstract Error 2](/images/php-abstract-dan-interface/abstract-error-2.png)

Waduh! Kok error lagi?
Errornya karena kita belum mendeklarasikan method abstract tadi yaitu ```rasa()``` yang ada di class abstract kue. Okay, let's fixing this!
``` php
<?php
include "Kue.php";
class KueDonat extends Kue {
    public function rasa () {
        echo "Rasa Keju";
    }
}
```
Coba dijalankan?

![Abstract Succes](/images/php-abstract-dan-interface/abstract-success.png)

Tuh kan berhasil :blush:
Ini sangat penting dalam dunia per-framework-an, yang sangat berguna untuk memastikan bahwa semua method tersedia didalam class dengan bagaimanapun pendeklarasiannya. Nah, dalam contoh ini kita selalu memanggil method ```rasa()``` dengan apapun jenis Kuenya. Belum paham?
Oke tambah lagi contohnya biar paham. Skenarionanya, kita akan membuat kue bolu yang mempunyai rasa cokelat. Tapi perlu dicatat bahwa tidak ada kue bolu yang akan tersedia didepan anda, jadi gausah berharap :stuck_out_tongue_winking_eye:. Perhatikan contoh syntax ini :

``` php
<?php
include "Kue.php";
class KueBolu extends Kue {
    public function rasa () {
        echo "Rasa Cokelat";
    }
}
```

Untuk membuat Kue Bolu rasa Cokelat ini kita tetap harus menggunakan method ```rasa()``` :

``` php
<?php
include "KueBolu.php";
$kue = new KueBolu();
$kue->rasa();
```
Nah mari kita jalankan, bagaimana hasilnya?


![Abstract Success 2](/images/laravel-book/abstract-interfaces/abstract-success-2.png)

Ukee cakep!. Udah cukup yaah penjelasannya. Eh tapi pertanyaannya, _kapan kita menggunakan teknik Abstract ini?_
Biasanya, teknik ini digunakan ketika kita membutuhkan masukan yang mempunyai ketergantungan pada class kita, misalkan seorang **Mahsiswa** _harus_ menyelesaikan **Skripsi** agar dapat diwisuda.

## Interface
Setelah mehami abstract mari kita lanjutkan dengan memahami Interface. Apa itu Interface?