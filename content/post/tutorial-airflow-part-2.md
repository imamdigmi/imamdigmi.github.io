---
date: 2018-11-17T13:41:36+07:00
lastmod: 2018-11-17T13:41:36+07:00

title: "Tutorial Airflow - Instalasi (Bagian 2)"
description: "Tutorial Airflow - Instalasi (Bagian 2)"
author: "Imam Digmi"

weight: 50
draft: false
keywords: [airflow, apache airflow, data, pipeline, data engineer]
tags: [data-pipeline, apache-airflow]
categories: [Data Pipeline, Python]
---

Seperti yang sudah kita bahas pada [Pengenalan Airflow - Bagian 1](/post/tutorial-airflow-part-1) bahwa Apache Airflow adalah pilihan yang tepat saat ini, maka pada Bagian 2 ini kita akan mencoba menginstal Apache Airflow di komputer masing-masing dan mencoba menjalankan contoh DAGs yang sudah disediakan oleh Airflow untuk kita memulai belajar

<!--more-->

# Setup
Sebelum kita menginstal Apache Airflow ada beberapa hal yang harus dipersiapkan yaitu mengatur _PATH Variable_ `AIRFLOW_HOME`, sejatinya, ini hanyalah sebuah path directory, path ini digunakan oleh Airflow untuk meletakkan beberapa berkas seperti: konfigurasi, logs, dags, operators, dsb. Secara default Airflow akan meletakkan direktori ini pada `/home/<user>/airflow` namun kita dapat mengubahnya sesuai kebutuhan, dan ada juga path variable `SLUGIFY_USES_TEXT_UNIDECODE` yang digunakan pada saat instalasi maupun upgrade Airflow untuk mencegah konflik _GPL dependency_ yaitu penggunaan `unidecode`. Berikut ini adalah pengaturan yang bisa kamu gunakan

```sh
export AIRFLOW_HOME=$HOME/airflow;
export SLUGIFY_USES_TEXT_UNIDECODE=yes;
```

# Instalasi Apache Airflow
```sh
pip install apache-airflow
```

**Tahukah kamu?** Bahwa jika kamu adalah seorang web developer atau pernah menjadi seorang web developer berlatar belakang Python dan menggunakan Flask sebagai _micro-framework_ (seperti saya :smile:) tentunya kamu tidak asing dengan logs saat instalasi Airflow ini. Ya! Airflow menggunakan Flask untuk membangung web nya yang memudahkan kita untuk memantau workflow (DAGs) yang kita buat, Airflow juga menggunakan pustaka-pustaka yang sering digunakan oleh web developer Flask seperti, `Flask-WTF` untuk urusan form, `Flask-SQLAlchemy` untuk urusan komunikasi database, `Flask-Admin`, `Flask-Migrate`, `Flask-Babel`, dst. Tentunya ini menguntungkan bagi kita yang familiar

Setelah kita berhasil menginstal Apache Airflow kita perlu menginisiasi database backend karena Airflow membutuhkan database untuk menyimpan _metadata_ nya, Database inilah yang digunakan Airflow untuk menyimpan data-data seperti: Variable, Connection, Pool, DAG State, Logs Task Instance, dll

```sh
airflow initdb
```

setelah diinisiasi Airflow membuatkan satu direktori `$AIRFLOW_HOME`, isi dari direktori tersebut kurang lebih akan seperti berikut

```
➜ tree ~/.airflow -L 2
/home/idiot/airflow
├── airflow.cfg
├── airflow.db
├── logs
│   └── scheduler
└── unittests.cfg

2 directories, 3 files
```

Seperti yang dapat kita lihat diatas bahwa Airflow membuatkan satu berkas yaitu `airflow.db` karena Airflow menggunakan SQLite sebagai database backend defaultnya, namun kita bisa saja mengubah database backend Airflow menggunakan database lain seperti PostgreSQL atau MySQL, namun untuk saat ini kita menggunakan database default saja dulu untuk percobaan, kita akan mencoba mengubah database backend Airflow di bagian selanjutnya

Yang tidak kalah penting adalah berkas `airflow.cfg`, berkas tersebut dapat kita digunakan untuk mengatur konfigurasi Airflow, dan lagi, kita bisa mengubahnya sesuai kebutuhan namun untuk saat ini kita biarkan saja seperti itu

> Buat kamu yang keburu penasaran, bisa menjalankan `airflow version` untuk melihat versi Airflow yang terinstal

Setelah semua nya berhasil di-_setup_ mari kita jalankan webserver nya untuk melihat tampilan utama web Airflow

```sh
airflow webserver
```

> Jika ingin menggunakan port lain `airflow webserver -p 8888`

Nah, berikut ini adalah tampilan awal web Airflow

![Airflow Webserver](/images/tutorial-airflow/part-2-1.png)

Seperti yang dapat kita lihat bahwa Airflow menyediakan contoh-contoh DAGs yang dapat kita gunakan untuk mulai belajar, kita akan lebih mengeksplor fitur-fitur Airflow di bagian selanjutnya

Sekarang mari kita jalankan Airflow _scheduler_, masih ingat apa itu Airflow scheduler? kalo kamu belum baca atau udah lupa kamu bisa membacanya di [Pengenalan Airflow - Bagian 1](/post/tutorial-airflow-part-1)

```sh
airflow scheduler
```

Setelah Airflow Scheduler kita jalankan, cobalah untuk menjalankan (un-pause) salah satu contoh DAGs yang disediakan kemudian memicunya (trigger) contohnya seperti yang ditunjukkan pada gambar berikut ini

![Airflow Webserver](/images/tutorial-airflow/part-2-2.png)

Kemudian kamu bisa coba klik di kolom DAG Runs dan di bagian bulat berwarna hijau, maka akan muncul tampilan sebuah daftar kapan saja DAG tersebut dijalankan

![Airflow Webserver](/images/tutorial-airflow/part-2-3.png)

Sekarang, cobalah untuk kembali ke halaman utama dan klik pada nama DAG yang sedang berjalan, maka kamu akan diberikan tampilan Tree View seperti berikut

![Airflow Webserver](/images/tutorial-airflow/part-2-4.png)

Dan kamu juga bisa mencoba meng-klik di menu Graph View yang menampilkan grafik DAGs beserta Tasks nya, seperti berikut

![Airflow Webserver](/images/tutorial-airflow/part-2-5.png)

Penasaran apa saja fungsi dari tampilan-tampilan yang disediakan oleh Airflow? Nah, pada bagian selanjutnya kita akan mencoba mengeksplornya lebih jauh lagi.

# Penanganan Kendala
Terkadang kita menemui beberapa kendala bahkan bisa jadi sering, entah karena kelalaian atau kesalahan lainnya, berikut ini adalah daftar kendala dan cara menanganinya

**Pada sub bagian ini, mungkin akan diperbarui secara berkala buat kamu yang sering mengalami kendala, atau saya membuatkan artikel khusus membahas kendala-kendala dan penanganannya dilain kesempatan**

## Error Webserver
Mungkin kita akan menemukan pesan error seperti "Port already exists", itu artinya port untuk webserver yang akan kita jalankan sudah digunakan, maka kita dapat melihat daftar penggunaan port dengan cara

```sh
lsof -t -i:8080
```

> `8080` adalah port target

maka kurang lebih akan muncul seperti berikut ini

![Airflow Webserver](/images/tutorial-airflow/part-2-6.png)


kita dapat menghentikan webservernya terlebih dahulu dengan cara `kill $(lsof -t -i:8080)`, kemudian kita dapat menggunakannya kembali
