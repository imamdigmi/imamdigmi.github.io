---
date: 2017-02-26T14:10:24+07:00
lastmod: 2017-02-26T14:10:24+07:00

title: "Belajar Rust - Ownership"
description: "Memahami ownership di Rust"
author: "Imam Digmi"

weight: 50
draft: false
keywords: ["rust", "ownership"]
tags: ["rust"]
categories: ["Rust"]
---

Sebelumnya kita membahas tentang [setup rust](https://imamdigmi.github.io/setup/belajar-rust-setup/) kali ini kita akan membahas tentang konsep dasar dari bahasa pemrograman Rust, pembahasan yang akan kita bahas meliputi :

1. [Ownership](https://imamdigmi.github.io/post/belajar-rust-ownnership/)
2. [Borrowing](https://imamdigmi.github.io/post/belajar-rust-references-dan-borrowing/)
3. [Lifetimes](https://imamdigmi.github.io/post/belajar-rust-lifetimes/)

Ketiga pembahasan diatas adalah "The most of distinct and compelling features in Rust" artinya ketiga hal tersebut adalah hal paling mendasar dan penting untuk mulai mempelajari bahasa Rust, karena ketiga pembahasan tersebut cukup panjang maka dibagi menjadi 3 artikel dan untuk artikel ini kita hanya akan membahas tentang ownership.

Sebelum kita mulai membahas lebih detail tentang Ownership, perlu diketahui bahwa Rust adalah bahasa pemrograman yang fokus pada keamanan (_safety_) dan kecepatan (_speed_) dengan fokus tersebut rust dapat memenuhi tujuan "zero-cost abstractions" artinya bahwa dengan bahasa Rust kita dapat membuat aplikasi dengan biaya yang sedikit mungkin, biaya yang dimaksudkan adalah biaya alokasi memori. Ownership adalah contoh utama dari zero-cost abstractions. Untuk mempelajari Rust memang harus lebih disiplin dan jika anda mempunyai pengalaman dalam bahasa pemrograman lainnya maka anda perlu belajar kembali tentang kedisiplinan penulisan maupun penyusunan kode program karena dalam hal ini Rust adalah bahasa yang jauh berbeda dengan bahasa lainnya.

Oke Let's learn about Ownership.

## Ownership
Jika di bahasa indonesiakan ownership adalah "kepemilikan" yang artinya Rust hanya memperbolehkan satu variable hanya memiliki satu pemilik saja atau apapun yang di ikatkan (binding) dengan variable tersebut, ini berarti ketika binding tersebut keluar dari ruang lingkupnya (scope) dengan otomatis Rust akan mengosongkan sumberdaya terkait (will free the bound of resources), contoh :
```rust
fn foo() {
    let name = "Monkey D. Luffy";
}
```
Ketika function `foo()` di panggil dan disitu terdapat variable `name` maka _string_ baru akan di buat kedalam stack, dan akan di alokasikan ruang memori kedalam heap untuk variable `name`. Tapi ketika function `foo()` sudah selesai melaksanakan tugasnya dan variable `name` selesai dieksekusi maka Rust akan membersihkan ruang yang tadinya dipakai oleh variable `name` dan semua yang berhubungan dengan alokasi ruang yang dipakai dalam function tersebut. Perlu diketahui bahwa ruang lingkup (scope) diawali dengan `{` dan di akhiri `}`.

Rust selalu memastikan bahwa hanya ada satu pemilik untuk satu binding (variable) untuk semua resource yang diberikan. Contoh :
```rust
let crew = vec!["Monkey D. Luffy", "Roronoa Zoro", "Vinsmoke Sanji"];
let crew2 = crew;
println!("The captain {}", crew[0]);
```
Kita mempunyai satu variable `crew` dan variable tersebut sudah di berikan ke variable lainnya yaitu `crew2`, jika kita panggil kembali variable `crew` maka kita akan mendapatan error seperti ini :
```
use of moved value: `crew`
let crew2 = crew;
    ----- value moved here
println!("The captain {}", crew[0]);
                           ^^^^ value used here after move
```
Itu artinya kita tidak boleh menggunakan variable yang sudah di pindahkan ke variable atau binding lainnya.

Ketika kita mempunyai kode seperti ini :
```rust
let age = 21;
```
Maka rust akan mengalokasikan memori dengan tipe data integer [i32](https://doc.rust-lang.org/stable/book/primitive-types.html#numeric-types) ke dalam stack lalu menyalin alamat memori (bit pattern) dan menghubungkan ke variable `age` tadi.

Mari kita bahas lebih dalam lagi tentang ownership. Jika kita mempunyai kode seperti ini :
```rust
let crew = vec!["Monkey D. Luffy", "Roronoa Zoro", "Vinsmoke Sanji"];
let mut crew2 = crew;
```
Pada baris pertama, Rust mengalokasikan memori untuk obyek vektor `crew` kedalam stack, tapi selain itu Rust juga mengalokasikan beberapa memori di heap untuk data aktual `(["Monkey D. Luffy", "Roronoa Zoro", "Vinsmoke Sanji"])` lalu Rust menyalin alamat memori di heap tadi kedalam pointer internal yang merupakan bagian dari obyek vektor tadi yang ditempatkan di stack (ini disebut data pointer). Kedua bagian dari vektor (satu di stack dan satu lagi di heap) panjang (length), capacity (kapassitas) dan lain-lain adalah sama nilainya.

Ketika kita memindahakan `crew` ke `crew2`, Sebenarnya Rust menyalin bitwise dari obyek vektor `crew` ke dalam alokasi stack untuk `crew2` tapi salinan ini tidak membuat kopian dari alokasi di heap yang berisi data aktual. Artinya bahwa akan ada dua pointer untuk kedua obyek vector yang mengarah ke heap yang sama.

Lalu bagaimana jika kedua obyek tersebut diakses secara bersamaan? For example, jika kita menghapus elemen ke tiga dari obyek `crew2` :
```rust
crew2.truncate(2);
```
sedangkan obyek `crew` masih diakses maka kita akan mendapatkan obyek vektor yang tidak lagi valid karena obyek `crew` tidak akan tahu bahwa salah satu elemen telah dihapuskan. Sekarang obyek `crew` pada stack tidak lagi sama dengan di heap, ini adalah kesalah yang riskan, karena menyebabkan kesalahan segmentasi atau lebih buruknya lagi memunginkan pengguna yang tidak sah untuk membaca alamat memori yang sudah tidak lagi memiliki akses. __**Inilah sebabnya mengapa Rust melarang untuk menggunakan obyek yang sebelumnya sudah dipindahkan/dipergunakan**__.

## Copy types
Rust sudah menetapkan bahwa ketika ownership sudah di transfer ke binding yang lain maka kita tidak dapat lagi menggunakan binding yang sebelumnya (asli). Namun kita bisa menggunakan fitur dari `trait` untuk menangani masalah ini yaitu `Copy`, namun untuk saat ini kita tidak akan membahas lebih dalam tentang trait, For example :
```rust
let a = 1;
let aa = a;
println!("A is: {}", v);
```
In this case, tipe `a` adalah `i32`, yang mengimplementasikan trait `Copy`. Ketika kita meng-assign `a` ke `aa` maka duplikat dari `a` akan dibuat tapi tidak dipindahkan dan kita tetap dapat mengakses `a`. Karena `i32` memiliki pointer ke data yang lain.

Semua [Primitive Types](https://doc.rust-lang.org/stable/book/primitive-types.html) adalah implementasi dari trait `Copy` dan kepemilikan nya tidak dipindahkan seperti yang diasumsikan mengikuti "aturan ownership". Untuk mengilustrasikannya, 2 potongan kode dibawah ini hanya mengkompilasi tipe data `i32` dan `boolean`.

```rust
fn main() {
    let var_x = 5;

    let _y = double(var_x);
    println!("{}", var_x);
}

fn double(x: i32) -> i32 {
    x * 2
}
```
```rust
fn main() {
    let var_x = true;

    let _y = change_truth(var_x);
    println!("{}", var_x);
}

fn change_truth(x: bool) -> bool {
    !x
}
```
Jika kita menggunakan tipe data yang tidak mengimplementasikan trait `Copy`, maka pada saat kompilasi kita akan mendapatkan pesan error karena kita mencoba untuk memindahakan nilai dari variable diatas.
```
error: use of moved value: `var_x`
println!("{}", var_x);
               ^
```

Terimakasih, Semoga bermanfaat :wink:

Sumber :
[Rust Book](https://doc.rust-lang.org/stable/book)