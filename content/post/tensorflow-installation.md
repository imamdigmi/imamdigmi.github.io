---
date: 2018-01-24T16:30:15+07:00
lastmod: 2018-01-24T16:30:15+07:00

title: "Instalasi Tensorflow"
description: "Instalasi TensorFlow di komputer lokal"
author: "Imam Digmi"

weight: 50
draft: false
keywords: [tensorflow, deep learning, machine learning]
tags: [tensorflow]
categories: [TensorFlow]
---

# Dependencies
Instal dependency untuk Python
```
$ sudo pacman -S python-numpy python-dev python-pip python-wheel
```

Untuk instalasi TensorFlow dengan GPU anda harus menginstal NVIDIA terlebih dahulu, cara instalasinya saya sudah tuliskan [disini](https://imamdigmi.github.io/post/nvidia-setup-archlinux/)

Buat `LD_LIBRARY_PATH` variable
```
$ export LD_LIBRARY_PATH=/opt/cuda/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
```

> Note: Sesuaikan lokasi instalasi CUDA

# Instalasi Package Wheel
## CPU
```
$ pip install --upgrade tensorflow      # for Python 2.7
$ pip3 install --upgrade tensorflow     # for Python 3.n
```
## GPU
```
$ pip install --upgrade tensorflow-gpu  # for Python 2.7 and GPU
$ pip3 install --upgrade tensorflow-gpu # for Python 3.n and GPU
```

Jika anda ingin menyimpan file `.whl` untuk keperluan instalasi dikemudian hari (hemat kuota internet :smile:) anda dapat mendownload paket pip [disini](https://www.tensorflow.org/install/install_linux#the_url_of_the_tensorflow_python_package)

# Instalasi Kode Sumber
## Persipan
Clone repository TensorFlow
```
$ git clone https://github.com/tensorflow/tensorflow
```

Setelah proses cloning selesai kita perlu pindah ke branch yang mengindikasikan versi TensorFlow yang akan kita gunakan
```
$ git checkout r1.5
```

> Note: `r1.5` adalah versi tensorflow, anda dapat memilihnya sesuai keinginan (r1.0, ..., r1.5)

TensorFlow membutuhkan [Bazel](https://docs.bazel.build/versions/master/install.html) untuk kompilasi maka kita perlu menginstal bazel terlebuh dahulu
```
$ sudo pacman -S bazel
```

## Konfigurasi
Masuk ke dikertori TensorFlow yang tadi di clone 
```
$ cd tensorflow
```

Lakukan konfigurasi untuk kompilasi TensorFlow
```
$ ./configure
```

Tahap konfigurasi kurang lebih akan seperti berikut
```
Please specify the location of python. [Default is /usr/bin/python]: 

Found possible Python library paths:
  /usr/lib/python3.6/site-packages
Please input the desired Python library path to use.  Default is [/usr/lib/python3.6/site-packages]

Do you wish to build TensorFlow with jemalloc as malloc support? [Y/n]: 
jemalloc as malloc support will be enabled for TensorFlow.

Do you wish to build TensorFlow with Google Cloud Platform support? [Y/n]: 
Google Cloud Platform support will be enabled for TensorFlow.

Do you wish to build TensorFlow with Hadoop File System support? [Y/n]: 
Hadoop File System support will be enabled for TensorFlow.

Do you wish to build TensorFlow with Amazon S3 File System support? [Y/n]: 
Amazon S3 File System support will be enabled for TensorFlow.

Do you wish to build TensorFlow with XLA JIT support? [y/N]: 
No XLA JIT support will be enabled for TensorFlow.

Do you wish to build TensorFlow with GDR support? [y/N]: 
No GDR support will be enabled for TensorFlow.

Do you wish to build TensorFlow with VERBS support? [y/N]: 
No VERBS support will be enabled for TensorFlow.

Do you wish to build TensorFlow with OpenCL SYCL support? [y/N]: 
No OpenCL SYCL support will be enabled for TensorFlow.

Do you wish to build TensorFlow with CUDA support? [y/N]: Y
CUDA support will be enabled for TensorFlow.

Please specify the CUDA SDK version you want to use, e.g. 7.0. [Leave empty to default to CUDA 9.0]: 9.1 

Please specify the location where CUDA 9.1 toolkit is installed. Refer to README.md for more details. [Default is /opt/cuda]: 

Please specify the cuDNN version you want to use. [Leave empty to default to cuDNN 7.0]: 7.0.5

Please specify the location where cuDNN 7.0.5 library is installed. Refer to README.md for more details. [Default is /opt/cuda]:

Please specify a list of comma-separated Cuda compute capabilities you want to build with.
You can find the compute capability of your device at: https://developer.nvidia.com/cuda-gpus.

Please note that each additional compute capability significantly increases your build time and binary size. [Default is: 3.0]

Do you want to use clang as CUDA compiler? [y/N]: 
nvcc will be used as CUDA compiler.

Please specify which gcc should be used by nvcc as the host compiler. [Default is /usr/bin/gcc-6]: 

Do you wish to build TensorFlow with MPI support? [y/N]: 
No MPI support will be enabled for TensorFlow.

Please specify optimization flags to use during compilation when bazel option "--config=opt" is specified [Default is -march=native]: 

Add "--config=mkl" to your bazel command to build with MKL support.
Please note that MKL on MacOS or windows is still not supported.
If you would like to use a local MKL instead of downloading, please set the environment variable "TF_MKL_ROOT" every time before build.

Would you like to interactively configure ./WORKSPACE for Android builds? [y/N]: Y
Searching for NDK and SDK installations.

Please specify the home path of the Android NDK to use. [Default is /home/idiot/Android/Sdk/ndk-bundle]: /home/idiot/.Android/Sdk/ndk-bundle   

Writing android_ndk_workspace rule.
WARNING: The API level of the NDK in /home/idiot/.Android/Sdk/ndk-bundle is 16, which is not supported by Bazel (officially supported versions: [10, 11, 12, 13, 14, 15]). Please use another version. Compiling Android targets may result in confusing errors.

Please specify the home path of the Android SDK to use. [Default is /home/idiot/Android/Sdk]: /home/idiot/.Android/Sdk

Please specify the Android SDK API level to use. [Available levels: ['19', '22', '23', '25', '26', '27']] [Default is 27]: 25

Please specify an Android build tools version to use. [Available versions: ['26.0.2', '27.0.3']] [Default is 27.0.3]: 

Writing android_sdk_workspace rule.
Configuration finished
```

## Kompilasi
Kompilasi hanya untuk CPU
```
$ bazel build --config=opt //tensorflow/tools/pip_package:build_pip_package
```

Kompilasi untuk GPU
```
$ bazel build --config=opt --config=cuda //tensorflow/tools/pip_package:build_pip_package --cxxopt="-D_GLIBCXX_USE_CXX11_ABI=0"
```

> Tip: Secara default pada saat kompilasi akan memakan banyak memory RAM, jika ingin membatasi konsumsi RAM tambahkan option berikut `--local_resources 2048,.5,1.0`

> Note: Kompilasi akan memakan waktu yang cukup lama

Setelah proses kompilasi selesai kita perlu mem-_build_ paket pip `.whl` yang akan disimpan pada direktori `/tmp/tensorflow_pkg`
```
$ bazel-bin/tensorflow/tools/pip_package/build_pip_package /tmp/tensorflow_pkg
```

Pindahkan paket pip `.whl` (_misal_: di direktori `home`) yang telah kita build tadi untuk keperluan instalasi dikemudian hari agar kita tidak perlu mengkompilasinya lagi
```
$ mv /tmp/tensorflow_pkg/tensorflow-1.5.0-cp36-cp36m-linux_x86_64.whl ~/
```

## Instalasi paket PIP
Instal paket yang telah kita build tadi untuk memeriksa hasil kompilasi
```
$ pip install --user /tmp/tensorflow_pkg/tensorflow-1.5.0-cp36-cp36m-linux_x86_64.whl
```

# Periksa hasil intalasi
Jika instalasi dan kompilasi selesai kita perlu memeriksa paket tersebut

Masuk ke python _shell_ anda dapat menggunakan python atau IPython
```
$ ipython
```

Masukkan kode berikut
```python
import tensorflow as tf
hello = tf.constant('Hello, TensorFlow!')
sess = tf.Session()
print(sess.run(hello))
```

Jika berhasil maka kurang lebih hasilnya akan seperti berikut
```
Python 3.6.4 (default, Jan  5 2018, 02:35:40) 
Type 'copyright', 'credits' or 'license' for more information
IPython 6.2.1 -- An enhanced Interactive Python. Type '?' for help.

In [1]: import tensorflow as tf
   ...: hello = tf.constant('Hello, TensorFlow!')
   ...: sess = tf.Session()
   ...: print(sess.run(hello))
   ...: 
2018-02-03 08:11:13.451300: I tensorflow/stream_executor/cuda/cuda_gpu_executor.cc:895] successful NUMA node read from SysFS had negative value (-1), but there must be at least one NUMA node, so returning NUMA node zero
2018-02-03 08:11:13.451726: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1105] Found device 0 with properties: 
name: Quadro K1100M major: 3 minor: 0 memoryClockRate(GHz): 0.7055
pciBusID: 0000:02:00.0
totalMemory: 1.95GiB freeMemory: 1.58GiB
2018-02-03 08:11:13.451752: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1195] Creating TensorFlow device (/device:GPU:0) -> (device: 0, name: Quadro K1100M, pci bus id: 0000:02:00.0, compute capability: 3.0)

b'Hello, TensorFlow!'
```