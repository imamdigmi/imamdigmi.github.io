---
date: 2016-06-03T14:06:22+07:00
lastmod: 2016-06-03T14:06:22+07:00

title: "Graph Database Dengan Neo4j"
description: "Berkenalan dengan Graph Database (GDBMS) menggunakanan Neo4J"
author: "Imam Digmi"

weight: 50
draft: false
keywords: ["Tutorial", "NoSQL", "Graph", "Database", "Neo4J", "GDBMS"]
tags: ["database"]
categories: ["NoSQL"]

comment: true
toc: true
autoCollapseToc: false
contentCopyright: <a rel="license" href="http://creativecommons.org/licenses/by-nc-nd/4.0/">CC BY-NC-ND 4.0</a>
reward: false
mathjax: false
mathjaxEnableSingleDollar: false
---

Pada kesempatan kali ini kita akan membahas tentang _Graph Database_ atau sering disebut dengan (GDBMS).
### Apa itu Graph Database?
Graph yang dimaksud pada graph database adalah definisi graph pada graph theory yang merupakan bagian dari ilmu matematika diskrit. Paper pertama yang membahas graph theory ditulis oleh Leonhard Eulear pada tahun 1736 yang membahas tentang permasalahan Konigsberg Bridge.
Sebagai contoh, database relasional akan kewalahan melakukan query yang dibutuhkan untuk keperluan jejaring sosial seperti Facebook dan Twitter. Database relasional memang memiliki JOIN untuk menggabungkan tabel, tapi JOIN yang mereka miliki tidak memiliki arah dan label. Salah satu jenis database yang diciptakan untuk mengembalikan makna ‘relasional’ tersebut adalah graph database.

### Apa itu Neo4J?
Pada kesempatan ini, saya akan mencoba menggunakan sebuah graph database populer, Neo4j. Saya harus mulai dari mana? Pada database relasional, saya bisa mulai dengan merancang tabel dengan merancang skema dengan menggunakan ERD. Berbeda dari database relasional, sebuah graph database tidak memiliki skema seperti tabel dan kolom yang berbeda dengan isi (record). Segala sesuatunya adalah data! Oleh sebab itu, pada saat merancang untuk graph database, saya bisa langsung memakai contoh data.
Neo4j adalah salah satu Graph Database populer dengan Cypher Query Language (CQL). Neo4j ditulis dalam Bahasa Java dan Neo4j adalah tool open source pertama untuk Grafik Database. Cukup itu saja dulu teorinya.

### Install Neo4J
Okay, setelah kita sedikit membahas Neo4J sekarang saatnya kita install Neo4J agar bisa merasakan sensasi Graph Databas secara nyata {%gemoji smile%}.

__Karena Neo4J adalah based on Java Language__
Java 8 tidak ada dalam pakeu Ubuntu 14.04 LTS, atau Debian 8 (jessie) dan anda perlu menginstallnya secara manual jadi komputer kita harus terinstall Java Runtime yaa, saya asumsikan komputer anda sudah ada Java Runtime nya.
Untku pengguna Debian bisa menggunakan [OpenJDK 8 backports](https://packages.debian.org/jessie-backports/openjdk-8-jdk).

#### Menggunakana Reposiory Debian
Untuk menginstall Neo4J kita gunakan Repository Debian yang juga bisa digunakan oleh Ubuntu, untuk menggunakan repository ini kita perlu melakukan langkah berikut :
``` bash
wget -O - https://debian.neo4j.org/neotechnology.gpg.key | sudo apt-key add -
echo 'deb http://debian.neo4j.org/repo stable/' >/tmp/neo4j.list
sudo mv /tmp/neo4j.list /etc/apt/sources.list.d
sudo apt-get update
```
#### Install Neo4J
Setelah repository sudah kita daftarkan dalam list repo komputer kita, baru kita bisa menginstall Neo4Jnya, silahkan pilih Edition mana yang akan anda install dengan cara :
Community Edition:
``` bash
sudo apt-get install -y neo4j
```
Enterprise Edition:
``` bash
sudo apt-get install -y neo4j-enterprise
```

### Menggunakanan Neo4J
Untuk menggunakan Neo4J kita harus mengaktifkannya dulu dengan cara lakukan perintah ini via Terminal anda :
``` bash
sudo neo4j start
```
Setelah itu buka browser anda dan arahkan ke [http://localhost:7474/](http://localhost:7474/) maka seperti inilah tamppilannya :

![Neo4j Homepage](/graph-database-dengan-neo4j/1.png)

Setelah terbuka, biasanya anda akan ada notifikasi bahwa database belum terkoneksi dan anda diminta untuk mengaktifkan server
database nya lalu anda diminta lagi untuk memasukkan password untuk user __neo4j__.
Jika sudah melalui proses tersebut maka kita siap untuk bermain-main dengan graph database.

Di Neo4J sudah di sediakan contoh data yang dapat kita gunakan dan tentu dengan perintah-perintah CQLnya juga baik banget kan {%gemoji smile%}
Untuk menggunakannya pilih menu dengan simbol ``Bintang`` lalu pilih ``Example Graphs`` lalu silahkan anda pilih kedua sample tersebut, saya memilih ``Movie Graph`` :

![Figure 1](/graph-database-dengan-neo4j/2.png)

Setelah itu klik tombol berbentuk simbol ``play`` disebelah kanan atas. Lalu akan muncul Contoh data Movie Graphnya seperti ini
Untuk mulai menggunakan contoh ini klik tombol ``>`` berwarna hitam dibagian kanan seperti ini :

![Figure 2](/graph-database-dengan-neo4j/3.png)

Lalu akan muncul contoh CQL ``Create`` berikut dengan datanya. Klik pada kotak yang terdapat CQL tersebut sepertin ini:

![Figure 3](/graph-database-dengan-neo4j/4.png)

Lalu perintah yang tadi di klik akan muncul di box untuk menjalankan perintah, setelah perintah nya muncul klik simbol ``play`` untuk menjalankannya :

![Figure 4](/graph-database-dengan-neo4j/5.png)

Setelah itu CQL akan memproses data yang dimasukkan, tunggu beberapa detik lalu akan muncul Graph Movie seperti ini :

![Figure 6](/graph-database-dengan-neo4j/6.png)

Keren kan {%gemoji smile%} hehee. Oke sampai disini dulu, jika ada pertanyaan silahkan berkomentar di kotak disqus dibawah ini.
terimakasih semoga bermanfaat.

__Note :__ Karena artikel ini belum sepenuhnya selesai maka tunggu updatenya, stay tune!.