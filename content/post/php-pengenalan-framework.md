---
date: 2016-05-24T00:43:50+07:00
lastmod: 2016-05-24T00:43:50+07:00

title: "Belajar Laravel - Pengenalan Framework"
description: "Berkenalan dengan teknologi framework di PHP"
author: "Imam Digmi"

weight: 50
draft: false
keywords: ["php", "framework", "laravel"]
tags: ["php", "laravel"]
categories: ["PHP"]
---

## Apakah buku ini sudah selesai?
**Belum**, Doakan saja semoga penulis sehat selalu agar buku ini dapat segera terselesaikan :wink:
Okay, mari kita bahas dulu apa itu framework dan bagaiamana cara kerjanya.

## Apa itu framework?
Adalah suatu struktur konseptual dasar yang digunakan untuk memecahkan atau menangani suatu masalah kompleks. Istilah ini sering digunakan antara lain dalam bidang perangkat lunak untuk menggambarkan suatu desain sistem perangkat lunak yang dapat digunakan kembali, serta dalam bidang manajemen untuk menggambarkan suatu konsep yang memungkinkan penanganan berbagai jenis atau entitas bisnis secara homogen.
[_Wikipedia_](https://id.wikipedia.org/wiki/Kerangka_kerja).

## Mengapa menggunakan framwork?
Framwork dibuat tentunya dengan adanya alasan dan manfaat yang terkandung didalamnya, apa saja manfaatnya? Framework daat mempercepat dan mempermudah pembangunan sebuah aplikasi, tanpa harus membuat fungsi atau class dari awal.

## Daftar isi
### 1. Konsep Dasar OOP
#### [Pengertian OOP](https://imamdigmi.github.io/post/php-pengertian-oop/)
#### [Class, Object, Property dan Method](https://imamdigmi.github.io/post/php-class-object-property-dan-method/)
#### [Abstract dan Interfaces](https://imamdigmi.github.io/post/php-abstract-dan-interface/)
### 2. Konsep Dasar
#### [JSON](https://imamdigmi.github.io/post/php-json/)
#### [Anonymous Function/Lambda & Closure](https://imamdigmi.github.io/post/php-anonymous-function-lambda-dan-closure/)
#### [Autoloader](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Traits](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Namespace](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Magic Method](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Reflection](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Composer](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
### 3. Konfigurasi, untuk Project Skala Besar
#### [Kebutuhan Sistem](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Instalasi](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Homestead](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Artisan CLI](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Environment](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Arsitektur Laravel](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Struktur Aplikasi](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Service Providers](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Service Container](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Facades](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Contracts](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Alur Request](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
### 4. Routing, Kendalikan Alur Aplikasi
#### [Kenapa menggunakan routing?](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Konfigurasi Routing](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Memahami HTTP Verb](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Menggunakan Beberapa Verb untuk Satu Route](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Memahami Parameter dalam Routing](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Regex Parameter](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Optional Parameter](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Memahami Named Routes](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Memahami Route Groups](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Subdomain Routing](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Whats next?](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
### 5. Mengakses Database
#### [Konfigurasi](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Database Migration & Schema Builder](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Basic CRUD Query](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Database Seeding](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Query Builder](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Transaksi](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Logging](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Database Caching](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Menggunakan Multiple Database](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Konfigurasi Read/Write](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
### 6. Model dari MVC
#### [Memahami ORM](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Eloquent, ORM Powerfull dari Laravel](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Konfigurasi](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Mass Assignment](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [CRUD Dasar](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Output Model ke Array / JSON](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Query dalam Eloquent](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Soft Delete](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Query Scope](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Global Scope](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Menggunakan Accessors dan Mutators](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Date Mutator & Carbon](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Attribute Casting](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Model Event & Observer](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Route Model Binding](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
### 7. Relationship dalam Eloquent
#### [Memahami Jenis Relationship](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [One To One](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [One To Many](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Many To Many](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Has Many Through](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Polymorphic Relations](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Self Referencing Relationship](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Many To Many Polymorphic Relations](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Custom Key untuk Relationship](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Eager Loading](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
### 8. Collection dengan Eloquent
#### [Class Collection](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Membuat Collection](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Method pada Collection](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
### 9. View dari MVC
#### [Penggunaan View](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Konfigurasi](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Passing Variable](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [View Composer](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Blade, Templating Engine dari Laravel](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Blade Layout](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Blade Control Structure](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Elixir](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Form](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Form Model Binding](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
### 10. Controller dari MVC
#### [Penggunaan Controller](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Konfigurasi](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Implicit Controller](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [RESTful Resource Controller](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Dependency Injection](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Route Caching](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Whats Next?](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
### 11. Request & Response
#### [Memahami Request di Laravel](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Menerima Input](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Old Input](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Menerima Upload File](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Bekerja dengan Cookie](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Mengambil info dari Request](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Memahami Response di Laravel](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Membuat Response Redirect](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Membuat Response JSON](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Membuat Response Download File](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Menggunakan Response Macro](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
### 11. Middleware
#### [Membuat Middleware](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [After Middleware](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Register](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Terminable](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Middleware Parameters](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Route Middleware](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Controller Middleware](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Middleware Group (update Laravel 5.2)](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
### 12. Authentikasi & Authorisasi, dari Login hingga Hak Akses
#### [Memahami Alur Authentikasi Laravel](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Proteksi route dengan Auth Middleware](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Menambah field baru ke table user](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Menggunakan Username untuk login](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Kustomisasi Login](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Password Reminder & Reset](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Menggunakan Throttles Login](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Auto Generate Authentikasi (Update Laravel 5.2)](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Menambahkan fitur Suspend](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Mengakses User yang telah Login](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Social Authentication dengan Socialite](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Membangun Hak Akses User (RBAC + ACL)](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Menggunakan Ability & Policy](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [JWT (JSON Web Token) Authentication](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
### 13. Validasi Data
#### [Memahami Validasi di Laravel](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Menampilkan pesan error](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Memodifikasi pesan error](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Validasi Kondisional](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Validasi Array (update Laravel 5.2)](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Validasi untuk Ajax / API](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Menggunakan berbagai rule validasi di Laravel](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Membuat Custom Rule](https://imamdigmi.github.io/post/php-pengenalan-framework/#)
#### [Validasi dengan Form Request](https://imamdigmi.github.io/post/php-pengenalan-framework/#)