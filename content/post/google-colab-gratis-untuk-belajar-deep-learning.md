---
date: 2018-02-21T05:39:09Z
lastmod: 2018-02-21T05:39:09Z

title: "Google Colab Gratis Untuk Belajar Deep Learning"
description: "Google Colab Gratis Untuk Belajar Deep Learning"
author: "Imam Digmi"

weight: 50
draft: false
keywords: [deep learning, machine learning, google, colab]
tags: [deep learning]
categories: [Deep Learning]
---

Google Colaboratory atau disebut juga _Colab_ adalah tools baru yang dikeluarkan oleh [Google Internal Research](https://research.google.com) yang dibuat untuk membantu para Researcher dalam mengolah data untuk keperluan belajar maupun bereksperimen pada pengolahan data khususnya bidang _Machine Learning_, tools ini secara penggunaan mirip seperti [Jupyter Notebook](http://jupyter.org/) dan dibuat diatas _envirounment_ Jupyter yang tidak memerlukan pengaturan atau setup terlebih dahulu sebelum digunakan dan berjalan sepenuhnya pada Cloud dengan memanfaatkan media penyimpanan Google Drive.

Yang menjadi menarik perhatian adalah tools Colab ini menyediakan layanan GPU gratis kepada penggunanya sebagai backend komputasi dan dapat digunakan selama 12 jam pada suatu waktu, menarik bukan? kita tidak perlu mengeluarkan sejumlah uang atau membeli perangkat komputer dengan tambahan GPU jika kita ingin belajar Machine Learning, karena pada praktiknya proses pelatihan (_training_) pada bidang Machine Learning ini cukup membuat pusing terlebih lagi jika perangkat yang kita miliki kurang memadai dan mungkin akan patah, apalagi jika kita hanya sekedar mencoba. 

Dengan Google Colab anda dapat membangun aplikasi berbasis Deep Learning menggunakan pustaka poluler seperti [Keras](https://keras.io/), [TensorFlow](https://www.tensorflow.org/), [PyTorch](http://pytorch.org/) dan [OpenCV](https://opencv.org/).

Pada atikel ini akan membahas penggunaan dasar dari Google Colaboratory, dan jika anda pernah menggunakan Jupyter Notebook pastinya anda sudah tidak asing lagi dengan ini karena pada prinsipnya tools ini sama seperti Jupyter Notebook, hanya perbedaannya Colab berjalan diatas cloud milik Google dan menyimpan berkas kita kedalam Google Drive, perbedaan mendasar lainnya adalah jika pada Jupyter Notebook kita hanya dapat menjalankan _syntax_ Python dan Markdown saja maka di Google Colab ini kita dapat menjalankan _command line_ langsung pada cell notebook dengan diawali tanda `!`.

# Setup
## Install Colab
Untuk dapat menggunakan Google Colab kita perlu menambahkan ekstensi baru ke Google Drive kita dengan cara klik tombol **New > More > Connect more apps** lalu tuliskan "colab" pada kolom search kemudian klik tombol **connect**.
![Install Colab](/images/google-colab-gratis-untuk-belajar-deep-learning/connect-app.png)

## Membuat Notebook
Jika Colab sudah terintegrasi dengan Drive kita maka kita siap untuk menggunakannya, pertama kita buat direktori baru terlebih dahulu dengan cara klik tombol **New** kemudian pilih **Folder** lalu berikan nama direktori tersebut (misal: `Colab`) kemudian buat file Notebook baru dengan cara klik kanan pada area kosong didalam direktori yang baru saja kita buat **Klik kanan > More > Colaboratory**.

![Create New Notebook](/images/google-colab-gratis-untuk-belajar-deep-learning/create-new-notebook.png)

Maka tampilan awal dari Notebook kita adalah seperti berikut

![Create New Notebook](/images/google-colab-gratis-untuk-belajar-deep-learning/startpage.png)

Ada baiknya kita berikan nama notebook kita dengan cara double klik pada area `Untitled0.ipnyb` lalu tuliskan nama notebook seperti berikut 

![Rename Notebook](/images/google-colab-gratis-untuk-belajar-deep-learning/rename-notebook.png)

## Setup GPU
Karena Colab menyediakan GPU gratis untuk penggunanya kita pun dapat memberikan pengaturan pada notebook untuk menggunakan layanan GPU gratis tersebut caranya klik menu **Edit > Notebook settings** kemudian ubah "Hardware accelerator" menjadi GPU dan kita juga dapat mengubah runtime versi Python pada notebook sedang aktif.

![Setup GPU](/images/google-colab-gratis-untuk-belajar-deep-learning/setup-gpu.png)

## Menjalankan Kode Pertama
Proses setup sudah selesai dan mari kita mencoba beberapa operasi tipe data dasar, berikut potongan kodenya

```python
x = 3
print(type(x)) # Prints "<class 'int'>"
print(x)       # Prints "3"
print(x + 1)   # Addition; prints "4"
print(x - 1)   # Subtraction; prints "2"
print(x * 2)   # Multiplication; prints "6"
print(x ** 2)  # Exponentiation; prints "9"
x += 1
print(x)  # Prints "4"
x *= 2
print(x)  # Prints "8"
y = 2.5
print(type(y)) # Prints "<class 'float'>"
print(y, y + 1, y * 2, y ** 2) # Prints "2.5 3.5 5.0 6.25"
```

Tekan tombol :arrow_forward: dibagian kiri cell untuk menjalankan perintah tersebut atau dapat juga dengan shortcut **Ctrl+Enter**

> **Tip!**: Tekan **Shift+Enter** jika ingin menjalankan perintah sekaligus menambah cell dibawahnya

Berikut ini adalah tampilan hasil dari menjalankan perintah diatas

![Basic Data Type Numpy](/images/google-colab-gratis-untuk-belajar-deep-learning/basic-data-types-numpy.png)

## Import File Python
Fitur colab tidak hanya dapat menjalankan perintah pada notebook saja tapi bisa juga menjalankan berkas yang berisikan kode Python (`*.py`) yang sudah kita buat sebelumnya, untuk memanfaatkan fitur ini kita perlu menginstal `OCaml Fuse` yang digunakan untuk import berkas Python, caranya jalankan perintah dibawah ini pada cell.

```
!apt-get install -y -qq software-properties-common python-software-properties module-init-tools
!add-apt-repository -y ppa:alessandro-strada/ppa 2>&1 > /dev/null
!apt-get update -qq 2>&1 > /dev/null
!apt-get -y install -qq google-drive-ocamlfuse fuse
from google.colab import auth
auth.authenticate_user()
from oauth2client.client import GoogleCredentials
creds = GoogleCredentials.get_application_default()
import getpass
!google-drive-ocamlfuse -headless -id={creds.client_id} -secret={creds.client_secret} < /dev/null 2>&1 | grep URL
vcode = getpass.getpass()
!echo {vcode} | google-drive-ocamlfuse -headless -id={creds.client_id} -secret={creds.client_secret}
```

Berikut ini tampilan hasil dari menjalankan perintah diatas.

![Google Verification](/images/google-colab-gratis-untuk-belajar-deep-learning/google-verification-result.png)

Kita perlu memasukkan kode verifikasi akun dengan cara klik link yang muncul pada output kemudian akan dialihkan ke tab baru lalu klik tombol **Allow**, maka akan muncul kode seperti berikut, salin kodenya lalu masukkan ke kolom **Enter verification code** pada output cell tadi dan **Enter**.

![Google Verification Code](/images/google-colab-gratis-untuk-belajar-deep-learning/google-verification-code.png)

Jika muncul peringatan dan link lagi maka lakukan seperti langkah diatas, **Klik link > Allow > Salin kode > Masukkkan Kode > Enter**, jika proses verifikasi berhasil maka output cell akan seperti berikut

![Google Verification Result](/images/google-colab-gratis-untuk-belajar-deep-learning/google-correctly-result.png)

Setelah proses verifikasi berhasil kita perlu melakukan _mounting_ Google Drive ke Notebook agar berkas yang ada bisa diakses langsung, caranya jalankan perintah berikut

```
!mkdir -p drive
!google-drive-ocamlfuse drive
```

Instal Keras

```
!pip install -q keras
```

Upload kode `mnist_cnn.py` kedalam direktori Colab di Google Drive, file tersebut dapat diunduh [disini](https://github.com/keras-team/keras/blob/master/examples/mnist_cnn.py). Kemudian jalankan file `mnist_cnn.py` tersebut dengan cara :

```
!python3 drive/Colab/mnist_cnn.py
```

Maka proses training mnist menggunakan pustaka Keras akan seperti berikut.

![Training MNIST Code](/images/google-colab-gratis-untuk-belajar-deep-learning/mnist-training.png)

# Dataset Titanic (.csv)
Lalu bagaimana jika ingin menggunakan dataset yang kita miliki dan mengoperasikannya? Baiklah mari kita coba menggunakan dataset [Titanic](https://data.world/nrippner/titanic-disaster-dataset) dan melakukan beberapa perintah di Notebook. Pertama-tama download dulu datasetnya dengan menjalankan perintah berikut pada cell maka file `Titanic.csv` akan otomatis tersimpan pada direktori Colab kita di Google Drive.

```
!wget https://raw.githubusercontent.com/vincentarelbundock/Rdatasets/master/csv/datasets/Titanic.csv -P drive/Colab
```
![Download Titanic Dataset](/images/google-colab-gratis-untuk-belajar-deep-learning/download-titanic-dataset.png)

Lalu kita coba tampilkan dataset tersebut menggunakan pustaka Pandas

```python
import pandas as pd
titanic = pd.read_csv("drive/app/Titanic.csv")
titanic.head(5)
```

Kode diatas akan menampilkan 5 data pertama pada berkas Titanic.csv dan outputnya akan seperti berikut

![Download Titanic Dataset](/images/google-colab-gratis-untuk-belajar-deep-learning/read-titanic-dataset.png)


# Cloning Repo Git
Mungkin ada satu kasus dimana kita ingin menggunakan kode dari repository seperti GitHub atau Bitbucket dan kita ingin mencobanya didalam Notebook, maka kita perlu mengkloning kode sumber tersebut kedalam Drive dan mengaksesnya melalui Notebook, ini dapat dilakukan dengan mudah seperti mengkloning repository seperti biasanya, caranya seperti berikut

```
!git clone https://github.com/imamdigmi/keras-mnist-tutorial.git drive/Colab/keras-mnist-tutorial
```

![Cloning Repo](/images/google-colab-gratis-untuk-belajar-deep-learning/clone-repo.png)

Dan direktori kode sumber repository tersebut sudah tersimpan di Google Drive dan berkas notebook dapat dijalankan langsung dari Google Drive

![Directory Repo](/images/google-colab-gratis-untuk-belajar-deep-learning/repo-cloned.png)

Jalankan notebook dari direktori repository dengan cara klik kanan pada berkas notebook __Open with > Colaboratory__.

![Open Notebook](/images/google-colab-gratis-untuk-belajar-deep-learning/open-notebook-from-repo.png)

Jalankan kode pada notebook dengan cara klik menu __Runtime > Run all__ atau dengan shortcut __Ctrl+F9__ beberapa outpunya akan seperti berikut

![Running MNIST](/images/google-colab-gratis-untuk-belajar-deep-learning/run-notebook.png)

# Tips!
Beberapa tips berikut ini mungkin akan sangat berguna jika anda ingin bermain dan berkolaborasi menggunakan Colab

## Restart Colab
Untuk merestart atau memuat ulang notebook bisa melalui menu __Runtime > Restart runtime__ atau dengan shortcut __Ctrl+M__ atau bisa juga dengan perintah berikut
```
!kill -9 -1
```

## Menggunakan TensorBoard 
Jika ingin menjalankan dan menggunakan TensorBoard melalui Colab penulis sangat merekomendasikan untuk menggunakan [Colab Utils](https://github.com/mixuala/colab_utils).

## Instal pustaka tambahan
Perintah-perintah berikut ini berguna untuk menginstal pustaka-pustaka pada Colab melalui notebook

### Keras
```
!pip install -q keras
import keras
```

### PyTorch
```
!pip install -q http://download.pytorch.org/whl/cu75/torch-0.2.0.post3-cp27-cp27mu-manylinux1_x86_64.whl torchvision
import torch
```

### MxNet
```
!apt install libnvrtc8.0
!pip install mxnet-cu80
import mxnet as mx
```

### OpenCV
```
!apt-get -qq install -y libsm6 libxext6 && pip install -q -U opencv-python
import cv2
```

### XGBoost
```
!pip install -q xgboost==0.4a30
import xgboost
```

### GraphViz
```
!apt-get -qq install -y graphviz && pip install -q pydot
import pydot
```

### 7zip Reader
```
!apt-get -qq install -y libarchive-dev && pip install -q -U libarchive
import libarchive
```

### Pustaka Lain
Untuk menginstal pustaka lain dapat menggunakan `pip` 
```
!pip install <package_name> 
```

dan juga dapat menggunakan _package manager_ seperti berikut
```
!apt-get install <package_name>
```

## Bermain dengan GPU
Untuk melihat apakah sedang menggunakan GPU atau tidak dapat menggunakan perintah berikut pada cell
```python
import tensorflow as tf
tf.test.gpu_device_name()
```

![GPU usage](/images/google-colab-gratis-untuk-belajar-deep-learning/tips-cpu-gpu.png)

Melihat spesifikasi GPU yang digunakan Google Colab
```python
from tensorflow.python.client import device_lib
device_lib.list_local_devices()
```

![GPU spec](/images/google-colab-gratis-untuk-belajar-deep-learning/gpu-spec.png)

Melihat spesifikasi Memory RAM
```
!cat /proc/meminfo
```

![RAM spec](/images/google-colab-gratis-untuk-belajar-deep-learning/ram-spec.png)

Melihat spesifikasi CPU
```
!cat /proc/cpuinfo
```

![CPU spec](/images/google-colab-gratis-untuk-belajar-deep-learning/cpu-spec.png)

## Mengubah direktori kerja
Pada padasarnya ketika menjalankan perintah `!ls` kita akan melihat direktori __datalab__ dan __drive__ yang sebenarnya tidak ada di direktori Google Drive kita, maka dari itu kita harus menambahkan __`drive/<nama_direktori>`__ sebelum mendefinisikan tiap nama berkas.

Untuk mengatasi masalah ini dapat dengan mudah untuk melakukan ubah direktori kerja (contoh pada artikel ini adalah direktori __Colab__) dengan menambahkan kode berikut :

```
import os
os.chdir("drive/Colab")
```

Kemudian jalankan perintah berikut untuk melihat daftar berkas dan direktori pada direktori kerja kita saat ini

```
!ls
```

## Troubleshooting
Terkadang, dalam menggunakan Colab kita seringkali menemui masalah yang umum seperti berikut ini

### "No backend with GPU available"
Jika anda menemukan error seperti berikut

```
Failed to assign a backend
No backend with GPU available. Would you like to use a runtime with no accelerator?
```

Tunggu beberapa saat lalu cobalah kembali, karena masalah tersebut pada dasarnya terjadi karena banyak orang sedang menggunakan GPU dan error diatas mengindikasikan bahwa semua layanan GPU sedang digunakan.

### "apt-key output should not be parsed (stdout is not a terminal)"
Jika anda menemukan error seperti berikut ini

```
Warning: apt-key output should not be parsed (stdout is not a terminal)
```

Artinya bahwa otentikasi sudah dilakukan sebelumnya, anda hanya perlu melakukan _mounting_ Google Drive dengan cara menjalankan perintah berikut

```
!mkdir -p drive
!google-drive-ocamlfuse drive
```

Referensi :

- [Kaggle](https://www.kaggle.com/getting-started/47096#post271139)
- [CS231n](http://cs231n.github.io/python-numpy-tutorial/)
- [Colab](https://colab.research.google.com/notebooks/welcome.ipynb)