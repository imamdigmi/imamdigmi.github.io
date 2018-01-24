---
date: 2016-05-26T22:44:46+07:00
lastmod: 2016-05-26T22:44:46+07:00

title: "Belajar Laravel - Anonymous Function/Lambda dan Closure"
description: "Memahami anonymous function atau lambda dan closure di php"
author: "Imam Digmi"

weight: 50
draft: false
keywords: ["php", "oop", "pbo", "lambda", "anonymous function", "closure", "laravel"]
tags: ["php", "laravel"]
categories: ["PHP"]
---

Pada artikel [sebelumnya](http://imamdigmi.github.io/post/php-json/) kita telah membahas tentang apa itu JSON.
Nah kali ini kita akan membahas tentang Anonymous Function/Lambda dan Closure,
Ukee mari kita bahas satu persatu dan kita mulai dari Anonymous Function atau Lambda.

## Apa itu Anonymous Function/Lamda?
Apakah anda pernah menonton film Hacker? mereka sering menyebutkan dirinya sebagai anonymous kan?
tidak mau menyebutkan nama asli mereka kan?
Nah, Anonymous Function/Lambda pun sama, mereka berdua tidak mau menyebutkan namanya tetapi
tindakan dan perlakuannya sangat terasa oleh kita.
Pada dasarnya, ini adalah sebuah fungsi (function) biasa tapi mempunyai keistimewaan yaitu tidak mempunyai nama.
Ukee perhatikan contoh syntax ini :
``` php
function nama () {
    return "Imam";
}

function sekolah ($nama) {
    return $nama . " rajin sekolah! :D";
}

echo sekolah(nama());
```
Pembaca yang baik pasti akan tahu bahwa itu adalah function biasa :P. Ukee mari kita jalankan bagaimana hasilnya?

![Function Basic](/images/php-anonymous-function-lambda-dan-closure/function-basic.png)

### Menulis anonymous function/lambda?
Anonymous function :
``` php
function () {
    return "Imam";
}
```
Tapi tunggu dulu, jika kita ingin menggunakan function ini kita perlu mempassing function tersebut ke sebuah variable. Contohnya seperti ini :
``` php
$nama = function () {
    return "Imam";
};

function sekolah ($nama) {
    return $nama . " rajin sekolah! :D";
}

echo sekolah($nama());
```
Perhatikan di baris ke __1__, disitu kita mempassing anonymous function ke sebuah variable yaitu (nama) dan tentunya di akhir
function yaitu ( } ) pada baris ke __3__ kita perlu menutup block dengan ( ; )
perhatikan juga di baris __9__ kita perlu menambahkan tanda ( $ ) untuk memanggilnya sebagaimana pemanggilan variable seperti biasanya.

Coba jalankan, seperti apa hasilnya? seharunya hasilnya akan tetap sama!

![Function Anonymous](/images/php-anonymous-function-lambda-dan-closure/function-anonymous.png)

Sudah paham? Jika sudah paham, mari kita lanjutkan ke Closure.

## Apa itu Closure?
Sama dengan Anonymous Function/Lambda, Closure juga berbentuk fungsi (function) tapi perbedaannya ada di kemampuannya yaitu
_closure dapat mengakses parameter dan variable dari luar function itu sendiri_ dan biasanya closure ini digunakan pada function yang menggunakan callback sebagai parameternya. Perhatikan, contohnya seperti :
``` php
$nama_depan = "Imam";
$nama_belakang = "Digm";

function sekolah ($namaLengkap) {
    return $namaLengkap . " rajin sekolah! :D";
}

$namaLengkap = function () use ($nama_depan, $nama_belakang) {
    return $nama_depan . " " . $nama_belakang;
}

echo sekolah($namaLengkap());
```
Perhatikan di baris __8__, terlihat bahwa jika kita ingin mengakses variable yaitu ($nama_depan, $nama_belakang) dari luar closure kita harus menggunakan keyword __use__.
Dan jika dijalankan hasilnya akan tetap sama.

![Closure](/images/php-anonymous-function-lambda-dan-closure/closure.png)

Itu masih closure biasa seperti anonymous, tapi bagaimana jika closure ini di gunakan sebagai parameter dari sebuah function? Contohnya seperti apa?
Pernah menggunakan function array_walk() bawaan dari PHP yang return valuenya boolean? Jika sudah maka parameter kedua itu dinamakan _callback_.

### Apa itu callback?
Callback adalah sebuah parameter atau argumen pada sebuah function yang berupa function,
bisa juga teknik Closure ini dinamakan _inner-function_ jadi ada function di dalam function. Ukee saya kasih contoh :

``` php
$min = 65;
$nilai = [
    ['nama' => 'Imam', 'nilai' => 70],
    ['nama' => 'Ramita', 'nilai' => 55],
    ['nama' => 'Rosmini', 'nilai' => 85],
];
array_walk($nilai, function ($siswa) use ($min) {
    echo $siswa['nama'] . " = " . $siswa['nilai'] . " -> ";
    $status = ($siswa['nilai'] >= $min) ? "Lulus" : "Tidak Lulus";
    echo $status;
    echo "\n";
});
```
Bagaimana jika code diatas kita dijalankan?

![Closure as Callback](/images/php-anonymous-function-lambda-dan-closure/closure-as-callback.png)

Sudah paham kan? :smile:

Semoga paham dan jika belum paham silahkan kasih komentar dikotak disqus yang ada di bawah ini atau anda dibaca ulang lagi lebih teliti ya,
karena teknik-teknik ini akan sering kita gunakan didalam pengembangan aplikasi yang menggunakan Laravel.