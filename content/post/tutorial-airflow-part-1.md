---
date: 2018-11-01T19:42:26+07:00
lastmod: 2019-01-28T19:42:26+07:00

title: "Tutorial Airflow - Pengenalan (Bagian 1)"
description: "Tutorial Airflow - Pengenalan (Bagian 1)"
author: "Imam Digmi"

weight: 50
draft: false
keywords: [airflow, apache airflow, data, pipeline, data engineer]
tags: [data-pipeline, apache-airflow]
categories: [Data Pipeline, Python]
---
Halo! Pada artikel kali ini saya akan membagikan pengalaman saya tentang membangun _data-pipeline_ menggunakan Apache Airflow, untuk itu kita akan membahasnya mulai dari konsep sampai pada tahap production, agar tutorial ini terorganisir dengan baik maka saya akan membaginya seperti berikut:

1. Konsep Dasar
2. Instalasi
3. Membuat DAG
4. Studi Kasus: Twitter Sentiment Analysis
5. Studi Kasus: Experimentation A/B Testing
6. Deployment Ke Production

Tutorial ini ditujukan buat kamu yang baru mempelajari Apache Airflow atau yang ingin terjun ke bidang Data Engineering, yang semoga saja tutorial ini dapat menjadi panduan kamu belajar.
<!--more-->

**<h4>TL;DR</h4>**

Data adalah mata uang bisnis modern. Dunia yang semakin online, terhubung oleh API yang tampaknya tidak terbatas telah menciptakan lautan data. Bisnis sekarang ini memiliki kesempatan untuk mendapatkan wawasan (_insights_) yang mendalam dari data tentang kebutuhan (_needs_), perilaku (_behaviors_), dan motif pelanggan mereka.

Seiring semakin banyaknya bisnis yang didorong oleh data (_data-driven_), ada kebutuhan yang semakin besar untuk melaksanakan "pipa" pemrosesan data (data pipeline) yang kompleks dan yang secara teratur mengekstraksi data dari berbagai sumber, mengubah data menjadi format yang memfasilitasi logika bisnis, dan menyimpan artefak yang dihasilkan dengan cara memfasilitasi peningkatan produk dan pengambilan keputusan pemangku kepentingan (_stakeholder_).

Hubungan perusahaan-perusahaan (kebanyakan adalah startup) seperti: [Tokopedia](https://tokopedia.com), [BukaLapak](https://bukalapak.com), [GO-JEK](https://www.go-jek.com) dengan data tentu tidak dapat terhindarkan. Dengan produk dan penggunanya yang bertumbuh sangat pesat, ribuan, jutaan, bahkan milyaran, dan mencatat ratusan juta peristiwa (_events_) setiap harinya tentunya perusahaan tersebut membutuhkan kerangka kerja (_framework_) untuk mengatur jalur pemrosesan data (_data pipeline_) yang pastinya sangat kompleks, atau alur kerja data seperti yang sering mereka sebut sebagai **workflow**.

**<h4>Penting</h4>**

Sebelum kita memahami apa itu Apache Airflow, kita wajib tahu dulu tentang masalah yang melatar belakangi mengapa Apache Airflow dibuat dan menjadi salah satu proyek terbesar di GitHub yang saat ini jumlah stargazers nya lebih dari 10.000, dengan begitu kita tidak tersesat dalam menggunakannya dan mudah untuk mengaplikasikannya.

Karena saya sering mendapatkan pertanyaan seperti ini: "Mam, kamu bikin aplikasi Android pake apa? Java atau Android Studio?" Aaaaakkkhhh gemesss!, dan, "Mam, bikin web pake apa yaa? XAMPP? atau Sublime?" Aaaaakkh geliii tolong jangan siksa akuuuu :disappointed_relieved:, yaaa ada benernya sih tapi kenapa kayak gitu pertanyaannya :expressionless:

# Workflow Management Systems
Singkatnya, Workflow Management Systems (WFMS) adalah suatu cara untuk mengatur tugas-tugas yang harus dikerjakan untuk mencapai tujuan/tahapan tertentu agar tugas-tugas tersebut tidak _repetitive_.

Lebih detilnya **Workflow** adalah serangkaian tugas-tugas yang harus dikerjakan entah secara berurutan maupun tidak, biasanya tugas-tugas tersebut memiliki ketergantungan satu sama lain. **Management** adalah cara mengatur tugas-tugas diatas. Sedangkan **System** adalah serangkaian komponen yang saling berkaitan demi mencapai tujuan tertenti.

**Apa tujuannya? jelas untuk menyederhanakan proses yang berulang-ulang**

# Apache Airflow
[Apache Airflow](https://airflow.apache.org/) adalah tool untuk orkestrasi pada komputasi manajemen workflow namun ada juga yang menyebutkan bahwa Airflow adalah ETL Framework, **kamu jangan bingung** ini adalah definisi yang sama saja, karena ETL singkatnya adalah serangkaian tugas-tugas atau proses dalam dunia data.

Jika kamu ditanya, "apa itu Apache Airflow?" kemudian kamu menjawab: Apache Airflow adalah Framework ETL! (ini benar), atau, Apache Airflow adalah tool Workflow Management System untuk data! (ini juga benar), asalkan jangan menjawab "Apache Airflow adalah Dashboard berbasis web untuk ETL" (ini salah).

Mengapa Apache Airflow dibuat? seperti yang dikatakan mas Maxime Beauchemin si pembuat Airflow dalam artikelnya yang berjudul [The Rise of the Data Engineer](https://medium.freecodecamp.org/the-rise-of-the-data-engineer-91be18f1e603)

<p><center style="font-size: 13pt; font-style: italic;">
    We've also observed a general shift away from drag-and-drop ETL (Extract Transform and Load) tools towards a more programmatic approach. Product know-how on platforms like Informatica, IBM Datastage, Cognos, AbInitio or Microsoft SSIS isn't common amongst modern data engineers, and being replaced by more generic software engineering skills along with understanding of programmatic or configuration driven platforms like Airflow, Oozie, Azkabhan or Luigi. It's also fairly common for engineers to develop and manage their own job orchestrator/scheduler.
</center></p>

Yang pada intinya, ETL di zaman sekarang (era Big Data) lebih kompleks ketimbang zaman dahulu yang tidak dapat diselesaikan dengan hanya drag-and-drop task, maka dari itu perlu dibuat suatu tool untuk mengakomodir ETL yang super kompleks tersebut.

Dan, yang perlu kamu ingat adalah Apache Airflow diciptakan untuk solusi Batch Processing **bukan Stream Processing atau Data Streaming**, krna itu jika kamu hendak mencari tool untuk _Stream Processing_ maka pilihlah [Apache Spark](http://spark.apache.org/streaming/), [Apache Storm](https://storm.apache.org/) dan kawan-kawannya.

# Kenapa Apache Airflow?
Jika kamu pernah menjadi seorang web developer, pasti kalian pernah menggunakan framework bukan? jika kalian menggunakan Python maka framework yang dapat digunakan adalah: Django, Flask, Falcon, dll, jika kamu menggunakan PHP? kalian bisa menggunakan: Laravel, Symfony, Yii, dst. Tapi bagaimana jika Workflow Management System? ada banyak pilihannya, kalian bisa menggunakan: Apache Airflow (AirBnB), Luigi (Spotify), Azkaban (LinkedIn), Pinball (Pinterest), dll.

Berikut ini adalah komparasi beberapa framework untuk WFMS (lihat lebih lengkapnya [disini](http://bytepawn.com/luigi-airflow-pinball.html))
![Luigi vs Airflow vs Pinball](/images/tutorial-airflow/part-1-1.png)
<p><center style="font-size: 10pt; font-style: italic;">[Sumber](http://bytepawn.com/luigi-airflow-pinball.html): Marton Trencseni's - Luigi vs Airflow vs Pinball</center></p>

Seperti yang dapat kita lihat bahwa Apache Airflow memiliki banyak fitur, dan didukung dengan integrasi tool eksternal yang banyak seperti: Hive, Pig, Google BigQuery, Amazon Redshift, Amazon S3, dst dan juga Apache Airflow memiliki keunggulan untuk urusan _scaling_. Wajar saja kita Apache Airflow menjadi pilihan yang tepat untuk membangun data pipeline saat ini.

## Konsep Utama
Sebagian orang atau bahkan diantara kamu biasanya tidak sabar dalam hal belajar, selalu buru-buru menulis kode tanpa mengetahui konsep dasarnya, itulah mengapa kebanyakan orang ketika belajar tersesat pada arah yang salah (dulu akupun begitu :grin:). Maka dari itu, kita perlu memahami konsep yang digunakan oleh Apache Airflow sehingga kita dapat lebih mudah mengimplementasikan dan mengaplikasikannya ke dalam dunia nyata dan apabila jika terdapat suatu masalah kamu dengan mudah mengatasinya.

Atau bahkan, kamu bisa saja kebingungan bagaimana cara membuat workflow, sedangkan kamu familiar dengan bahasa Python, karena itu, ini bukan hanya programming yang menjadi tumpuan melainkan sebuah metodologi dan konsep bagaimana membangun workflow, karena Apache Airflow sudah mengadopsi _best-practice_ ETL atau workflow, itulah salah satu keuntungan yang kita dapatkan ketika menggunakan Airflow.

<p><center style="font-size: 13pt; font-style: italic;">
    Karena sejatinya, teknologi adalah sebuah produk dimana kita menciptakan sebuah solusi.
</center></p>

Berikut ini adalah arsitekur Apache Airflow secara umum, yang menunjukkan bahwa Apache Airflow memiliki beberapa komponen diantaranya: Worker, Scheduler, Web UI (Dashboard), Web Server, Database, dst dalam menjalankan tugasnya dan untuk menggerakkan workflow yang kita buat.

<center><img src="/images/tutorial-airflow/part-1-2.png" alt="Airflow Architecture"></center>
<p><center style="font-size: 10pt; font-style: italic;">Arsitektur Apache Airflow</center></p>

Berikut ini adalah komponen utama dalam membangun workflow atau data-pipeline menggunakan Apache Airflow, komponen utamanya adalah DAG, karena setiap kali kita membuat sebuah workflow pastinya kita akan menuliskan bariskan kode berupa DAG.

![DAG](/images/tutorial-airflow/part-1-3.png)
<p><center style="font-size: 10pt; font-style: italic;">_Directed Acyclic Graphs_</center></p>

### DAG
DAG adalah kepanjangan dari _Directed Acyclic Graphs_ yang kita gunakan untuk membuat suatu workflow atau kita juga dapat memahami DAG sebagai sekumpulan dari Tasks. DAG inilah yang mencerminkan tentang alur dari workflow beserta relasi antar proses dan ketergantungan antar prosesnya.

<center><img src="/images/tutorial-airflow/part-1-4.png" alt="Contoh DAG"></center>
<p><center style="font-size: 10pt; font-style: italic;">Contoh DAG Sederhana 1</center></p>

Seperti contoh diatas, secara sederhana kita membuat DAG yang memiliki 3 tugas di dalamnya yaitu: `tugas_1`, `tugas_2`, `tugas_3`, tugas-tugas tersebut ada urutannya 1-2-3, bisa dikatakan bahwa `tugas_1` harus berhasil dijalankan sebelum `tugas_2` dijalankan dan begitu pun juga dengan `tugas_3` bahwa akan dijalankan hanya jika `tugas_2` berhasil dijalankan.

<center><img src="/images/tutorial-airflow/part-1-5.png" alt="Contoh DAG"></center>
<p><center style="font-size: 10pt; font-style: italic;">Contoh DAG Sederhana 2</center></p>

Contoh lainnya adalah seperti DAG pada gambar diatas yang menunjukkan bahwa, kita memiliki 4 tugas yaitu: `tugas_1`, `tugas_1_2`, `tugas_2`, dan `tugas_3`. Yang berbeda dari contoh DAG sebelumnya yakni pada Task `tugas_2` tidak memiliki ketergantungan pada Task sebelumnya, namun Task `tugas_3` akan dijalankan hanya jika `tugas_1_2` dan `tugas_2` berhasil dijalankan.

Tapi bagaimana jika ada suatu Task yang gagal dijalankan (terjadi error)? ya tentunya Task yang gagal tersebut akan dijalankan ulang (_retry_) sampai berhasil dijalankan dan Task selanjutnya akan menunggu dengan rentang waktu yang kita tentukan barulah kemudian Task selanjutnya dijalankan.

Dengan ini, DAG dapat mendefinisikan bagaimana kita ingin melakukan workflow atau data-pipeline kita, tapi kita belum menentukan apa saja yang dilakukan oleh tugas-tugas tersebut, bisa saja untuk `tugas_1` bertanggung jawab dalam mengambil data kemudian `tugas_2` bertanggung untuk membersihkan data dan yang terakhir `tugas_3` bertanggung jawab dalam menyimpan data hasil dari `tugas_2` kedalam data-store kita.

**Yang perlu kita ingat bahwa**:

1. DAG bersifat <u>"acyclic"</u> yang artinya tiap Task tidak akan berputar atau tidak kembali ke Task sebelumnya. Jika dianalogikan, ini seperti seorang ayah dari seorang anak, tentunya sang ayah kandung tidak akan menjadi seorang anak dari anak kandungnya sendiri
2. DAG tidak peduli apa yang sedang kita lakukan, karena DAG hanya bertugas dalam memastikan tugas-tugas didalamnya, dieksekusi pada waktu yang tepat, dengan urutan yang tepat, dan dengan penanganan yang benar atas masalah yang tidak terduga
3. Dan juga, di dalam membuat DAG terdapat `dag_id` yang digunakan Airflow untuk mengidentifikasi **satu** DAG saja, ini bersifat unik yang artinya kita tidak boleh menggunakan nama `dag_id` lebih dari satu kali

> Saat Airflow mencari DAG, Airflow hanya akan mengakui berkas `.py` yang memiliki string "airflow" dan "DAG" di dalamnya.

### Tasks
Tasks adalah "aktivitas" yang kamu buat kemudian dijalankan oleh Operator. Task bisa berupa Python function atau eksternal yang bisa dipanggil. Tasks ini diharapkan bersifat _idempotent_ (penjelasan tentang idempotent dapat dilihat [disini](https://en.wikipedia.org/wiki/Idempotence), [disini](https://stackoverflow.com/questions/1077412/what-is-an-idempotent-operation), dan [disini](https://stackoverflow.com/questions/40296211/what-is-the-difference-between-an-idempotent-and-a-deterministic-function)), yang intinya, jika nilai yang dimasukkan sama maka hasilnya akan tetap sama — tidak peduli berapa kali Tasks ini dijalankan.

**Yang perlu diingat bahwa**:
Dalam membuat Tasks, terdapat `task_id` sama halnya dengan `dag_id` ini bersifat unik tidak boleh digunakan berulang kali dalam satu konteks DAG itu sendiri tapi `task_id` boleh sama dengan DAG lainnya, misal: `dag_1` memiliki `task_a` dan `task_b` maka kita boleh menggunakan `task_id` yang sama pada `dag_2`.

### Operator
Operator adalah yang "menjalankan" Tasks yang kamu buat. Ketika kita membuat suatu workflow (data-pipeline) di Airflow, maka workflow tersebut didefinisikan menggunakan Operator di dalam DAG, karena setiap operator menjalankan Tasks tertentu yang ditulis sebagai Python function atau perintah shell. Airflow sudah membuatkan banyak sekali Operator yang bisa kita gunakan untuk keperluan ini, tapi buat kamu yang selalu penasaran untuk mencoba dan merasa ada yang kurang, kamu bisa membuatnya sendiri dengan meng-_extend_ class `BaseOperator` dan meng-_implement_ method `execute()`.

**Kamu jangan bingung dengan Operator dan Tasks**, Tasks itu adalah "apa yang kita jalankan" sedangkan Operator adalah "cara bagaimana kita menjalankannya", sebagai contoh, kamu ingin makan mie rebus maka mie rebus inilah disebut sebagai **Tasks** dan cara/langkah kamu memasaknya disebut sebagai **Operator**.

### Scheduler
Scheduler (penjadwalan) adalah otak di balik pengaturan workflow di Airflow atau jika dianalogikan, Scheduler adalah "petugas" yang bertanggung jawab dalam memantau **semua DAG beserta Tasks** yang ada, dan memicu (men-_trigger_) **semua** _Task Instances_ yang dependensinya telah terpenuhi, scheduler ini juga yang memastikan DAGs yang ada di dalam _DagBag_ tetap tersinkronisasi dengan Airflow, makanya setiap kali kita menambahkan satu berkas Python berisi DAG kedalam DagBag Airflow akan segera mengetahuinya dan menampilkannya di Web Dashboard (selama scheduler dijalankan).

### Executor
Executor adalah _message queuing_ yang terikat erat dengan Scheduler dan menentukan proses worker yang benar-benar mengeksekusi setiap Task yang dijadwalkan. Ada berbagai macam jenis Executor, yang masing-masing menggunakan class Executor khusus. Sebagai contoh, `LocalExecutor` mengeksekusi Task secara paralel yang berjalan pada mesin yang sama dengan Scheduler. Executor lainnya seperti `CeleryExecutor` mengeksekusi Task menggunakan worker yang ada pada "sekelompok mesin" worker yang terpisah ini biasa disebut sebagai _distributed worker_ atau dengan kata lain worker yang terdistribusi.

### Worker
Ini adalah komponen yang benar-benar menjalankan logika Task, dan worker ini ditentukan oleh Executor yang digunakan, dengan kata lain, Worker lah yang melakukan tugas yang diberikan oleh Operator (Task). Jika dianalogikan sama seperti kita memesan makanan menggunakan layanan GO-FOOD milik GO-JEK, misal kita ingin makan bakso (Task) karena malas keluar rumah kita menggunakan GO-FOOD (Executor) nah si driver yang bertugas untuk membeli bakso adalah Worker, karena layanan GO-FOOD lah yang menentukan siapa driver yang mendapatkan giliran menerima order.

<!-- ### Sensor -->

<!-- ### XCom -->

<!-- ### Backfill -->

## Istilah Lain
Selain konsep-konsep diatas ada juga istilah-istilah yang menjadi kunci utama dalam membangun workflow dan mesti kamu pahami, karena jika kamu salah memahami ini atau bahkan tidak paham maka workflow atau data-pipeline yang kamu buat tidak akan sesuai dengan yang kamu harapkan, istilah ini dapat kamu jumpai dalam menuliskan DAG, Task, maupun di web dashboard Airflow.

> Istilah ini juga sering saya temukan dan dipertanyakan di StackOverflow maupun di forum lainnya yang membuat kebanyakan orang bingung

### Start Date
`start_date` adalah tanggal dimulainya data kamu diambil. `start_date` ini biasanya dituliskan di dalam `default_args` pada saat kamu membuat DAG. Sebagai contoh, kamu memiliki 5 data seperti berikut:

| id | nama        | tanggal    |
|:--:|:------------|:-----------|
| 1  | Data Satu   | 2018-10-30 |
| 2  | Data Dua    | 2018-10-31 |
| 3  | Data Tiga   | 2018-11-01 |
| 4  | Data Empat  | 2018-11-02 |
| 5  | Data Lima   | 2018-11-03 |

Jika `start_date` nya adalah 2018-11-01 (`datetime(2018, 11, 1)`) maka data yang akan kamu dapatkan adalah data dengan id 3, 4, dan 5. Jadi data yang id nya 1 dan 2 tidak akan ikut diambil karena diluar `start_date` yang kamu tentukan.

**Penting**: `start_date` tidak boleh menggunakan tanggal dinamis seperti `datetime.now()` karena akan membingungkan dan akan menimbulkan permasalahan

> Tips: Terdapat function bantuan yang disediakan oleh Airflow yaitu `airflow.utils.dates.days_ago(n)` untuk mendapatkan tanggal yang lalu dimana `n` adalah total harinya

### Schedule Interval
`schedule_interval` adalah rentang waktu DAG dijalankan. `schedule_interval` ini diletakkan pada argument DAG yang nilainya dapat berupa `timedelta(n)` atau berupa string yang memuat [Cron Expression](https://en.wikipedia.org/wiki/Cron#CRON_expression) atau preset yang telah disediakan oleh Airflow, berikut ini adalah contoh dari ketiga pilihan tersebut untuk menambah pemahaman kamu:

| Kode                                            | Rentang waktu             | Jenis            |
|-------------------------------------------------|---------------------------|------------------|
| `DAG(..., schedule_interval=timedelta(days=1))` | Perhari                   | Python Timedelta |
| `DAG(..., schedule_interval='0 0 * * 1')`       | Tiap hari Senin jam 00:00 | Cron Expression  |
| `DAG(..., schedule_interval='@hourly')`         | Per satu jam              | Preset           |

dan berikut ini adalah daftar preset yang diambil dari [Official Documentation Ariflow](https://airflow.apache.org/scheduler.html#dag-runs)

| Preset     | Penjadwalan                                   | Cron        |
|------------|-----------------------------------------------|-------------|
| `None`     | Tidak terjadwal                               |             |
| `@once`    | Hanya sekali                                  |             |
| `@hourly`  | Tiap jam                                      | `0 * * * *` |
| `@daily`   | Tiap hari pada tengah malam                   | `0 0 * * *` |
| `@weekly`  | Tiap minggu pagi pada tengah malam            | `0 0 * * 0` |
| `@monthly` | Tiap awal bulan pada tengah malam             | `0 0 1 * *` |
| `@yearly`  | Tiap awal tahun pada tengah malam (1 Januari) | `0 0 1 1 *` |

**Penting**:

1. Jika ingin tidak terjadwal maka nilainya harus `None` tidak boleh `'None'` (string)
2. Nilai default dari `schedule_interval` adalah satu hari yaitu `datetime.timedelta(1)`
3. Jangan mendefinisikan `schedule_interval` didalam `default_args` melainkan harus sebagai argument
4. `schedule_interval` tidak boleh didefinisikan ulang (_override_) oleh Task

> Tips: kamu dapat menggunakan [crontab.guru](https://crontab.guru/) sebagai sarana untuk menentukan `schedule_interval` ala-ala cronjob

### DAG Run
Seperti namanya, DAG Run adalah **DAG yang dijalankan** oleh Airflow, sebenarnya dibalik ini Airflow membuat objek yang mewakili instantiasi DAG dalam waktu tertentu. Kita ambil contoh pada sub bagian [Start Date](/post/tutorial-airflow-part-1/#start-date), jika sekarang tanggal 2018-11-04 dan kita menetapkan `schedule_interval` nya `@daily` maka DAG Run yang kita miliki adalah 4 karena `start_date` kita adalah 2018-11-01, yang artinya Airflow sudah menjalankan DAG tersebut empat kali.

Lalu kapan DAG kita dijalankan oleh Airflow? Jawabannya adalah, ketika `start_date` + `schedule_interval` telah terlewati.

### Execution Date
Ini adalah tanggal atau waktu yang **menunjukkan kapan** DAG dijalankan. Karena tentunya kita ingin mengetahui kapan saja DAG tersebut dijalankan dalam waktu tertentu maka Execution Date muncul ketika DAG Run diinstantiasi. Sebagai contoh, mari kita ambil kasus dan data pada sub bagian [Start Date](/post/tutorial-airflow-part-1/#start-date) tadi, jika kita merujuk pada penjelasan sub bagian [Dag Run](/post/tutorial-airflow-part-1/#dag-run) maka execution date nya adalah sebagai berikut:

1. `2018-11-01 00:00:00`
2. `2018-11-02 00:00:00`
3. `2018-11-03 00:00:00`
4. `2018-11-04 00:00:00`

### Task Instances
Task Instace adalah Task di dalam DAG **yang telah di instantiasi** oleh Airflow. Semua Task terasosiasi dengan DAG Run dan Task tersebut direferensikan sebagai `TaskInstance`, dengan kata lain sebuah `TaskInstance` adalah Task yang terinstantiasi dan memiliki konteks `ExecutionDate`. Sebagai contoh kita ambil dari data sebelumnya dan kita memiliki 3 Task maka Task Instance nya adalah sebagai Berikut:

1. DAG Run `2018-11-01 00:00:00`: `task_1`, `task_2`, `task_3`
2. DAG Run `2018-11-02 00:00:00`: `task_1`, `task_2`, `task_3`
3. DAG Run `2018-11-03 00:00:00`: `task_1`, `task_2`, `task_3`
4. DAG Run `2018-11-04 00:00:00`: `task_1`, `task_2`, `task_3`

DAGRun dan TaskInstance juga konsep utama dalam Airflow, setiap DAGRun dan TaskInstance saling terhubung dengan entri dalam database metadata Airflow yang mencatat kondisi (status) mereka seperti: "queued", "running", "failed", "skipped", dan "up for retry". Membaca (_read_) dan memperbarui (_update_) status ini adalah kunci untuk penjadwalan dan proses eksekusi yang dilakukan oleh Airflow.

# Kesimpulan
Secara sederhana, berikut ini adalah alur bagaimana Airflow bekerja

1. Memuat DAG yang tersedia di DagBag
2. Scheduler menggunakan DAG yang telah dimuat untuk mengidentifikasi dan atau menginisialisasi DagRun di metadata database
3. Scheduler :
    - memeriksa kondisi (status) dari `TaskInstances` yang terhubung dengan `DagRun` yang sedang aktif
    - menyelesaikan semua ketergantungan tiap `TaskInstance`
    - mengidentifikasi `TaskInstance` yang harus dieksekusi
    - menambahkannya kedalam Worker queue
    - memperbarui status dari `TaskInstance` dari "newly-queued" ke "queued" pada database
4. Tiap worker yang tersedia, mengambil `TaskInstance` dari queue dan menjalankannya kemudian memperbarui status `TaskInstance` dari "queued" ke "running"
5. Ketika `TaskInstance` berhasil dijalankan, maka Worker yang terkait mengubah statusnya pada database menjadi: "finished", "failed", dst (tergantung dari kondisi `TaskInstance` saat dijalankan)
6. Scheduler mengubah status semua `DagRun` yang aktif ke: "running", "failed", "finished". Tergantung pada semua kondisi `TaskInstance`
7. Ulangi langkah 2 s/d 6


**Sumber**:

- [The Rise of the Data Engineer](https://medium.freecodecamp.org/the-rise-of-the-data-engineer-91be18f1e603)
- [What Is an Idempotent](https://stackoverflow.com/questions/1077412/what-is-an-idempotent-operation)
- [Airflow FAQ](https://airflow.apache.org/faq.html)
- [Airflow Concepts](https://airflow.apache.org/concepts.html)
- [Airflow Scheduler](https://airflow.apache.org/scheduler.html)
- [Airflow API](https://airflow.apache.org/code.html)

**<h4>Di bagian ini saya akan memperbaruinya secara berkala agar kamu dapat kembali ke bagian ini kapan saja untuk mendapatkan pemahaman lebih (walaupun tidak semuanya), saya tidak berjanji namun akan diusahakan.</h4>**
