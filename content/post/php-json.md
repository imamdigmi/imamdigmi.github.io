---
date: 2016-05-26 01:05:06+07:00
lastmod: 2016-05-26 01:05:06+07:00

title: "Belajar Laravel - Json (JavaScript Object Notation)"
description: "Memahami JSON di PHP"
author: "Imam Digmi"

weight: 50
draft: false
keywords: ["php", "json", "laravel"]
tags: ["php", "laravel"]
categories: ["PHP"]
---

Pada artikel [sebelumnya](http://imamdigmi.github.io/post/php-abstract-dan-interface/) kita telah membahas tentang konsep dasar OOP. Ukee Broo or Sist! saya yakin anda sudah tidak sabar untuk membahas tentang laravel,
karena mungkin anda sudah tau mengapa anda memilih framework laravel untuk dipelajari yaitu framework yang sekarang banyak digunakan dan lagi terkenal.
Tapi santai dulu, saya pengen agar anda bisa paham banget di laravel, makadari itu sengaja saya sisipkan konsep dasarnya dulu yaah agar anda tidak tersesat dalam proses pembelajaran ini.

*Note :
Pada bagian ini, saya sarankan anda bukanlah orang yang baru kenal dengan PHP, dan kalo udah ngerasa mual-mual, puyeng, kepala sakit berlanjut setelah membaca artikel ini silahkan hubungi paman google dan pelajari dasar-dasar PHP prosedural atau OOP secara mendalam.*

## Apa itu JSON?
JSON (dibaca: "jeyson") adalah singkatan dari _Javascript Object Notation_ adalah format ringkasan pertukaran data dalam komputer. Tapi secara teknis, biasanya JSON ini digunakan untuk pertukaran
data dalam satu aplikasi ke aplikasi lainnya, bisa dari web ke mobile ataupun sebaliknya atau juga bisa dari satu bahasa pemrograman ke bahasa lainnya. Pada awalnya JSON hanya digunakan di javascript saja
tapi siringnya waktu, JSON ini digunakan oleh bahasa-bahasa lain.
Konsepnya hampir mirip dengan _array_ bisa terdapat lebih dari satu dimensi didalamnya dan juga key-value. Hanya saja, jika di array hanya berupa element sedangakn JSON berupa object.

## Bagaimana menulis format JSON?
Nah cara menulisnya sangat mudah, yaitu data yang tersimpan harus didalam kurung-kurawal ```{...}``` lalu diikuti dengan nama indexnya penamaanya sama dengan variable dan valuenya pun sama dengan mendefinisikan variable, seperi ini format penulisannya :
``` json
{
    key1: value1,
    key2: {
        key3: value3,
        key4: value4,
        key5: value5
    },
    key6: value6
}
```

JSON juga mendukung berbagai format data, diataranya :

- Number    : bilangan bulat atau desimal.
- String    : teks yang diapit tanda petik (_single/double quotes_).
- Boolean   : Isian benar (_true_) atau salah (_false_).
- Array     : Data terurut yang diapit ```[...]``` dan dibatasi dengan koma. Array juga bisa berisi gabungan tipe data yang lain.
- Object    : Data tidak terurut yang diapit ```{...}``` dan dibatasi koma. Setiap elemen object berisi key-value yang dibatasi dengan ```:``` (_titik dua_).
- NULL      : nilai kosong, diisi dengan keyword null.

Jika format data tersebut di gabungkan maka penulisannya seperti ini :
``` json
{
    "nim": 135610103,
    "nama": "Imam Digmi",
    "umur": 20,
    "lulus": false,
    "ipk": 1.80,
    "telpon": null,
    "hobi": [
        'membaca',
        'menulis',
        'main gitar'
    ],
    "mata_kuliah": {
        "Distributed Database",
        "Cloud Computing",
        "Enterprise Information Systems",
        "System Information Functional"
    },
    "asal_sekolah": {
        {"tingkat": "SD", "nama_lembaga": "SDN 1 Cikedung, Indramayu"},
        {"tingkat": "SMP", "nama_lembaga": "SMP Terisila, Indramayu"},
        {"tingkat": "SMK", "nama_lembaga": "SMKN 1 Cikedung, Indramayu"},
        {"tingkat": "S1", "nama_lembaga": "STMIK AKAKOM Yogyakarta"}
    }
}
```
Karena kita akan menggunakan laravel, maka kita akan sering berjumpa dengan JSON. Maka dari itu, anda pastikan paham dengan ini.
