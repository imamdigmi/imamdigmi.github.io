---
date: 2017-02-26T17:23:01+07:00
lastmod: 2017-02-26T17:23:01+07:00

title: "Belajar Rust - References Dan Borrowing"
description: "Memahami References dan Borrowing di Rust"
author: "Imam Digmi"

weight: 50
draft: false
keywords: ["rust", "references", "borrowing"]
tags: ["rust"]
categories: ["Rust"]
---

Sebelumnya kita membahas tentang konsep dasar dari Rust yaitu [Ownership](https://imamdigmi.github.io/post/belajar-rust-ownership/), kita masih dalam konsep dasar dan kali ini kita akan membahas tentang konsep dasar selanjutnya yaitu References dan Borrowing.

Rust adalah bahasa dengan safety code dimana object diatur oleh bahasa pemograman tersebut dari awal hingga akhir. Developer tidak perlu lagi melakukan pointer arithmatic dan manajemen memory seperti yang kita lakukan dalam bahasa C dan C++.

Oke Let's learn about Borrowing.

## Borrowing
Kode program dibawah ini pada dasarnya untuk menjumlahkan dua buah vector :
```rust
fn main() {
    fn sum_vec(v: &Vec<i32>) -> i32 {
        return v.iter().fold(0, |a, &b| a + b);
    }
    fn foo(v1: &Vec<i32>, v2: &Vec<i32>) -> i32 {
        let s1 = sum_vec(v1);
        let s2 = sum_vec(v2);
        s1 + s2
    }
    let v1 = vec![1, 2, 3];
    let v2 = vec![4, 5, 6];
    let answer = foo(&v1, &v2);
    println!("{}", answer);
}
```
Alih-alih kita menggunakan `Vec<i32>` sebagai argument pada function, kita menggunakan references: `&Vec<i32>` untuk melakukan pereferensian. Dan kita tidak menggunakan `v1` dan `v2` secara langsung melainkan kita menggunakn `&v1` dan `&v2`. Kita gunakan "references" yaitu `&T` sebagai cara untuk membawa obyek tapi tidak sekaligus dengan pemiliknya. Binding yang di-_borrow_ tidak akan dihapuskan alokasi memorinya sehingga ketika kita memanggil binding tersebut diluar ruanglingkupnya kita masih bisa menggunakan binding yang aslinya.

Referensi tetap bersifat "immutable" seperti binding, yang artinya jika terdapat binding didalam function `foo()` maka tidak akan bisa diubah atau dibawa keluar dari "scope" :
```rust
fn main() {
  fn foo(v: &Vec<i32>) {
    v.push(5);
  }
  let v = vec![];
  foo(&v);
}
```
Jika program diatas kita running, maka akan muncul error seperti ini :
```
error: cannot borrow immutable borrowed content `*v` as mutable
fn foo(v: &Vec<i32>) {
          --------- use `&mut Vec<i32>` here to make mutable
v.push(5);
^
```
karena kita mencoba untuk memasukkan secara paksa sebuah nilai kedalam vector, dan itu tidak akan bisa kita lalukan.

## References
Jenis kedua dari references yaitu : `&mut T`. Adalah sebuah "mutable references" yang mana kita diperbolehkan untuk merubah resource dari yang kita borrow. For example :
```rust
fn main() {
  let mut x = 5;
  {
    let y = &mut x;
    *y += 1;
  }
  println!("{}", x);
}
```

Program diatas akan menghasilkan angka `6`. Kita buat mutable reference untuk `x` (`&mut x`) lalu kita tambahkan nilai **1** ke `y`. Karena sebelumnya kita memanggil `x` dengan mutable references maka kita bisa mengolah `x` tadi, jika tidak seperti itu maka kita tidak akan bisa mengolahnya.

Ketika kita ingin mengakes pereferensian dari `&mut` maka kita membutuhkan asterisk (`*`) sebelum `y` karena `y` adalah `&mut` dari pe-referensian. Selain itu kita tambahkan pula `{` dan `}` yang menandakan bahwa pengolahan tersebut dalam ruanglingkup tersendiri, coba kita ubah kode diatas menjadi seperti ini :
```rust
fn main() {
  let mut x = 5;
  let y = &mut x;
  *y += 1;
  println!("{}", x);
}
```
maka kita akan mendapatkan error seperti ini :

```bash
error: cannot borrow `x` as immutable because it is also borrowed as mutable
let y = &mut x;
             - mutable borrow occurs here
*y += 1;
println!("{}", x);
               ^ immutable borrow occurs here
- mutable borrow ends here
```

## Borrowing Rules
- Semua borrow harus dalam ruanglingkup (scope) yang tidak lebih dari binding aslinya.
- Boleh memakai satu atau dua dari kedua jenis borrow, tapi tidak memakai kedua jenis borrow secara bersamaan :
  - satu atau lebih references (`&T`) ke resource.
  - tepat satu mutable reference (`&mut T`).

Dengan references, kemungkinan kita akan memiliki banyak references yang akan kita buat. Namun kita hanya dapat memiliki `&mut` dalam satu waktu, dengan demikian kita tidak akan bisa memiliki data race jika kita menggunakan `&mut` secara bersamaan. Inilah mengapa Rust mencegah data races pada saat kompilasi dan kita akan mendapatkan error ketika kita melanggar aturan yang ada.

Oke, let's consider our example again.
```rust
fn main() {
  let mut x = 5;
  let y = &mut x;
  *y += 1;
  println!("{}", x);
}
```
Jika kode diatas kita jalankan, maka kita akan mendapatkan error seperti ini :
```bash
error: cannot borrow `x` as immutable because it is also borrowed as mutable
let y = &mut x;
             - mutable borrow occurs here
*y += 1;
println!("{}", x);
               ^ immutable borrow occurs here
- mutable borrow ends here
```
Error tersebut karena kita telah melanggar aturan : `&mut T` yang di pointing ke `x`, dan kita tidak diperbolehkan untuk membuat `&T`.
```rust
fn main() {
  let mut x = 5;
  let y = &mut x;    // -+ &mut borrow `x` dimulai disini.
  *y += 1;           //  |
  println!("{}", x); // -+ - mencoba mem-borrow kembali `x` disini.
}                    // -+ &mut borrow dari `x` berkahir disini.
```
Scope nya akan konflik karena kita tidak bisa membuat `&x` selama `y` masih dalam scope yang sama dengan binding yang aslinya.
Dan ketika kita membuat scope untuk borrowing diatas kita tandai dengan `{` dan `}` :
```rust
let mut x = 5;
{
    let y = &mut x; // -+ &mut borrow dimulai disini.
    *y += 1;        //  |
}                   // -+ ... borrow berakhir disini.
println!("{}", x);  // <- `x` di-borrow kembali disini.
```
Kode diatas tidak akan mendapatkan masalah karena kita mem-borrow dalam scope yang berbeda.

Baik, sekian dulu belajar kali ini.
Terimakasih, Semoga bermanfaat :wink:

Sumber :
[Rust Book](https://doc.rust-lang.org/stable/book)