---
date: 2019-01-02T10:50:01+07:00
lastmod: 2019-01-02T10:50:01+07:00

title: "Tutorial Airflow - Deploy ke Production (Bagian 6)"
description: "Tutorial Airflow - Deploy ke Production (Bagian 6)"
author: "Imam Digmi"

weight: 50
draft: true
keywords: [airflow, apache airflow, data, pipeline, data engineer]
tags: [data-pipeline, apache-airflow]
categories: [Data Pipeline, Python]
---

<!--more-->

### Integrasi dengan PostgreSQL

#### Instalasi PostgreSQL
```sh
sudo pacman -S postgresql
```

```sh
sudo -iu postgres
```

Inisialisasi database
```sh
initdb -D /var/lib/postgres/data
```

> Lihat lebih lanjut di [ArchWiki](https://wiki.archlinux.org/index.php/PostgreSQL)

##### Distribusi Lain
Distribusi linux Debian/Ubuntu
```sh
sudo apt update && sudo apt install postgresql postgresql-contrib
```

Distribusi linux CentOS
```sh
sudo yum install postgresql-server postgresql-contrib;
sudo postgresql-setup initdb;
```

##### Menjalankan PostgreSQL

```sh
sudo systemctl start postgresql;
sudo systemctl status postgresql;
sudo systemctl enable postgresql;
```

##### Setup User/Database

```sql
CREATE DATABASE airflow;
CREATE USER airflow WITH ENCRYPTED PASSWORD 'airflow';
GRANT ALL PRIVILEGES ON DATABASE airflow TO airflow;
ALTER ROLE airflow SET search_path = airflow;
```

##### Setup Airflow
```sh
pip install apache-airflow[postgres,async,crypto,password,redis]
```

ubah beberapa nilai berikut pada konfigurasi `airflow.cfg`

| Section       | Variable            | Ubah Menjadi                                                   |
|:--------------|:--------------------|:---------------------------------------------------------------|
| `[core]`      | `default_timezone`  | `Asia/Jakarta`                                                 |
| `[core]`      | `sql_alchemy_conn`  | `postgresql+psycopg2://airflow:airflow@localhost:5432/airflow` |
| `[core]`      | `executor`          | `LocalExecutor`                                                |
| `[webserver]` | `authenticate`      | `True`                                                         |

dan tambahkan nilai berikut pada konfigurasi `airflow.cfg`
```cfg
auth_backend = airflow.contrib.auth.backends.password_auth
```

kemudian start kembali Airflow Webserver, maka akan muncul halaman login

![Airflow Login](/images/9.png)
---
date: 2019-02-03T15:05:44+07:00
lastmod: 2019-02-03T15:05:44+07:00

title: "Tutorial Airflow Part 5"
description: "Tutorial Airflow Part 5"
author: "Imam Digmi"

weight: 50
draft: true
keywords: []
tags: []
categories: []
---

<!--more-->
