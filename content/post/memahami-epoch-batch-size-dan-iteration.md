---
date: 2018-01-25T22:53:14+07:00
lastmod: 2018-01-25T22:53:14+07:00

title: "Memahami Epoch Batch Size Dan Iteration"
description: "Memahami cara kerja dan fungsi Epoch, Batch Size, dan Iterations pada Machine Learning"
author: "Imam Digmi"

weight: 50
draft: false
keywords: [machine learning, deep learning, neural network, epoch, batch size, iterations]
tags: [machine learning, deep learning]
categories: [Machine Learning]
---

Jika anda adalah seseorang yang sedang menggeluti bidang _Machine Learning_ atau _Deep Learning_ pasti sangat familiar dengan ketiga hal ini, namun mungkin juga anda sering bingung sampai menggaruk-garuk kepala anda dan bertanya-tanya "Mengapa saya mengetik ketiga istilah ini dalam kode saya dan apa bedanya? fungsinya apa?" karena semuanya terlihat sangat mirip.

Baiklah mari kita bahas, tapi untuk mengetahui perbedaan antara ketiga hal ini kita perlu mengetahui beberapa istilah pada _Machine Learning_ seperti _Gradient Descent_ agar kita lebih memahami fungsi dari Epoch, Batch Size, dan Iterations.

## Gradient Descent
_Gradient Descent_ adalah algoritma untuk mengoptimalkan iteratif yang digunakan pada Machine Learning untuk menemukan hasil yang terbaik (minima kurva). Algoritma iteratif berarti bahwa kita perlu mendapatkan hasilnya berkali-kali untuk mendapatkan hasil yang paling optimal atau bisa dikatakan hampir sempurna. Kualitas iteratif dari _Gradient Descent_ membantu grafik yang tidak dilengkapi untuk membuat grafik sesuai dengan optimal pada data.

![Gradient Descent](/images/memahami-epoch-batch-size-iteration/gradient-descent.gif)

_Gradient Descent_ mempunyai sebuah parameter yang bernama **_learning rate_**, seperti yang anda lihat di atas (sebelah kiri). Pada awalnya, langkahnya lebih besar yang berarti _learning rate_ tersebut tinggi dan dipertengahan hasilnya menurun, _learning rate_ menjadi lebih kecil dengan ukuran langkah yang lebih pendek. Begitupun juga dengan _Cost Function_ yang menurun atau _Cost_nya menurun. Kadang, mungkin anda pernah mendengar orang mengatakan bahwa _Loss Function_nya menurun atau _Cost_nya menurun, dan perlu diketahui juga bahwa "Cost" dan "Loss" merupakan hal yang sama. Dan jika _Cost_ atau _Loss_nya menurun itu bagus karena pada prinsipnya semakin kecil kerugian yang kita dapatkan akan semakin bagus untuk kita, kan?.

Mungkin juga anda bertanya-tanya mengapa ada istilah _Epoch_, _Batch Size_, dan _Iterations_ di _Machine Learning_. Jawabannya adalah, karena ketiga hal ini menjadi sebuah solusi untuk menangani data yang jumlahnya besar yang mana komputer kita tidak memungkinkan untuk men-training data begitu banyaknya dalam satu kali training. Jadi, untuk mengatasi masalah ini kita perlu membagi data menjadi ukuran yang lebih kecil dan memberikannya ke komputer kita satu per satu dan memperbarui weight (bobot) pada _Neural Network_ di akhir setiap langkah agar sesuai dengan data yang diberikan.

## Epoch
_Epoch_ adalah ketika seluruh dataset sudah melalui proses training pada Neural Netwok sampai dikembalikan ke awal untuk sekali putaran, karena satu _Epoch_ terlalu besar untuk dimasukkan (feeding) kedalam komputer maka dari itu kita perlu membaginya kedalam satuan kecil (batches).

Tapi, mengapa kita perlu lebih dari satu Epoch?
Kita tahu itu tidak masuk akal di awal bahwa melewati seluruh dataset melalui jaringan saraf tidak cukup dan kita perlu melewati dataset penuh beberapa kali ke jaringan saraf yang sama. Namun perlu diingat bahwa kita menggunakan dataset yang terbatas dan untuk mengoptimalkan pembelajaran dan grafik yang kita gunakan adalah _Gradient Descent_ yang merupakan proses iteratif. Jadi, mengupdate weight (bobot) dengan satu epoch saja tidak cukup.

Satu epoch mengarah pada underfitting pada grafik (di bawah).

![Epoch](/images/memahami-epoch-batch-size-iteration/epoch.png)

Seiring bertambahnya jumlah epoch, semakin banyak pula weight (bobot) yang berubah dalam Neural Network dan kurvanya melengkung dari kurva yang kurang sesuai hingga selaras dengan kurva yang overfitting.

Lalu berapakah jumlah epoch yang harus kita tentukan?
Sayangnya, tidak ada jawaban yang benar untuk pertanyaan ini. Jawabannya berbeda untuk dataset yang berbeda tapi anda bisa mengatakan bahwa jumlah epoch terkait dengan beragamnya data anda, jadi jumlah epoch tergantung dataset yang anda miliki.

## Batch Size
_Batch Size_ adalah jumlah sampel data yang disebarkan ke Neural Network. Contoh: jika kita mempunyai 100 dataset dan batch size kita adalah 5 maka algoritma ini akan menggunakan 5 sempel data pertama dari 100 data yang kita miliki (<sup>ke</sup>1, <sup>ke</sup>2, <sup>ke</sup>3, <sup>ke</sup>4, dan <sup>ke</sup>5) lalu disebarkankan atau ditraining oleh Neural Network sampai selesai kemudian mengambil kembali 5 sampel data kedua dari 100 data (<sup>ke</sup>6, <sup>ke</sup>7, <sup>ke</sup>8, <sup>ke</sup>9, dan <sup>ke</sup>10), dan begitu seterusnya sampai 5 sampel data ke 20 (100/5=20).

Tapi, apa itu Batch?
Seperti yang penulis katakan sebelumnya, kita tidak bisa melewati seluruh dataset ke dalam jaring saraf sekaligus. Jadi, kita membagi dataset menjadi sejumlah atau satu set atau bagian. Sama seperti kita membagi sebuah naskah skripsi yang panjang menjadi beberapa bagian seperti BAB 1, BAB 2, dan BAB 3 yang memudahkan kita untuk pembacanya dan memahami keseluruhan naskah tersebut. :smile:

## Iterations
_Iterations_ adalah jumlah batch yang diperlukan untuk menyelesaikan satu epoch. Tapi, untuk memahami iterasi sebenarnya kita hanya perlu mengetahui tabel perkalian atau memiliki kalkulator :stuck_out_tongue_winking_eye:

> Note: Jumlah batch sama dengan jumlah iterasi untuk satu epoch.

Katakanlah kita memiliki 2000 contoh training yang akan kita gunakan, maka kita dapat membagi dataset dari 2000 contoh tersebut menjadi batch dari 500 maka akan dibutuhkan 4 iterasi untuk menyelesaikan 1 epoch :wink:

Referensi :
[Machine Learning 101](https://medium.com/onfido-tech/machine-learning-101-be2e0a86c96a)