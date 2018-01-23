---
date: 2017-02-26T20:09:23+07:00
lastmod: 2017-02-26T20:09:23+07:00

title: "Belajar Rust - Lifetimes"
description: "Memahami lifetimes di Rust"
author: "Imam Digmi"

weight: 50
draft: false
keywords: ["rust", "lifetimes"]
tags: ["rust"]
categories: ["Rust"]
---

Sebelumnya kita membahas tentang konsep dasar kedua dari Rust yaitu [References dan Borrowing](https://imamdigmi.github.io/post/belajar-rust-references-dan-borrowing/), pada artikel ini kita masih membahas tentang konsep dasar Rust yaitu Lifetimes. Ketika kita sudah selesai berinteraksi dengan object maka proses untuk dis-alokasi memory akan dilakukan secara otomatis. Sehingga kita tidak perlu secara manual untuk membersihkan memory yang sudah digunakan.

Pada unmanaged code seperti C++ cukup sulit untuk membuat sebuah program yang benar dan terbebas dari bug, sehingga seringkali menimbulkan celah yang bisa dicompromise. Ancaman yang paling besar adalah buffer overflow attack yaitu ketika seseorang bisa mengakses informasi melebihi dari informasi yang disediakan pada alokasi memory space untuk program, Artinya seseorang bisa melakukan modifikasi di level bawah pada lokasi memori yang berada diluar jangkauan oleh program.

Oke Let's get started with Lifetimes.

## Lifetimes
Melakukan references ke resources yang dimiliki binding lain dapat menjadi rumit. Misalnya, bayangkan jika terdapat operasi :

1. Menangani beberapa jenis resource.
2. Meminjamkan references ke resource.
3. Memutuskan hubungan ke resource dan mendisalokasikan memori namun masih memiliki references.
4. Memutuskan untuk menggunakan resource.

Dan boom! Your reference is pointing to an invalid resource. Ini yang disebut "[dangling pointer](https://en.wikipedia.org/wiki/Dangling_pointer)" atau "use after free". Contoh sederhana untuk situasi diatas adalah :
```rust
let r;              // Kita buat binding : `r`.
{
    let i = 1;      // Kita buat scoped value untuk : `i`.
    r = &i;         // Assing reference dari `i` ke `r`.
}                   // `i` keluar dari scope.
println!("{}", r);  // tapi `r` masih me-refer ke `i`.
```

Untuk menghindarai kesalahan diatas, maka kita perlu memastikan bahwa pereferensian tetap dalam scope atau ruanglingkupnya.

Ketika kita membuat suatu function yang mengambil argumen dari references maka situasinya akan menjadi lebih kompleks. Perhatikan contoh berikut :

```rust
fn skip_prefix(line: &str, prefix: &str) -> &str {
  // ...
}
let line = "lang:en=Hello World!";
let lang = "en";
let v;
{
  let p = format!("lang:{}=", lang);  // -+ `p` didalam scope
  v = skip_prefix(line, p.as_str());  //  |
}                                       // -+ `p` keluar dari scope.
println!("{}", v);
```

Di sini kita memiliki fungsi `skip_prefix` yang mengambil dua referensi `&str` sebagai parameter dan mengembalikan referensi tunggal `&str`. Kita panggil dengan cara mem-passing ke dalam references pada `p`: dua variable dengan lifetimes yang berbeda.


Terimakasih, Semoga bermanfaat {%gemoji wink%}

Sumber :
[Rust Book](https://doc.rust-lang.org/stable/book)