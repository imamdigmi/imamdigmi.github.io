---
date: 2017-01-21T18:25:27+07:00
lastmod: 2017-01-21T18:25:27+07:00

title: "Websocket Dengan Golang"
description: "Berkenalan dengan Websocket di Golang"
author: "Imam Digmi"

weight: 50
draft: false
keywords: ["go", "golang", "websocket"]
tags: ["golang"]
categories: ["GoLang"]
---

WebSocket merupakan sebuah protokol komunikasi dua arah yang dapat digunakan oleh browser. Jika pada AJAX kita hanya dapat melakukan komunikasi satu arah dengan mengirimkan request kepada server dan menunggu balasannya, maka menggunakan WebSocket kita tidak hanya dapat mengirimkan request kepada server, tetapi juga menerima data dari server tanpa harus mengirimkan request terlebih dahulu. Hal ini berarti ketika menggunakan WebSocket pengguna harus terus menerus terkoneksi dengan server, dan kita memerlukan sebuah server khusus untuk dapat menjalankan aplikasi WebSocket dengan benar.<!--more-->

kita dapat melihat bagaimana dalam kasus AJAX, setiap request dari pengguna maupun respon dari server dilakukan secara terpisah. Jika ingin mengirimkan request ke server, kita perlu membuka koneksi baru yang kemudian akan dibalas oleh server seperti ia membalas permintaan halaman HTML biasa. Server juga tidak dapat langsung melakukan pengiriman data ke klien tanpa ada permintaan terlebih dahulu. Hal ini berbeda dengan pola komunikasi WebSocket, di mana koneksi berjalan tanpa terputus dan server dapat mengirimkan data atau perintah tanpa harus ada permintaan dari pengguna terlebih dahulu. Dengan begitu, WebSocket bahkan juga memungkinkan server mengirimkan data atau perintah ke semua klien yang terkoneksi, misalkan untuk memberikan notifikasi global.

Untuk dapat melihat langsung bagaimana cara kerja WebSocket, kita akan langsung melakukan eksperimen. Objek yang diperlukan untuk menggunakan WebSocket dari browser adalah WebSocket. Tetapi sebelumnya, kita memerlukan sedikit persiapan terlebih dahulu, yaitu komponen server dari WebSocket.

### Websocket dengan golang (Aplikasi Chatting)
Pertama, pastikan golang sudah terinstall jika belum bisa baca [disini](http://imamdigmi.github.io/blog/2017/01/15/install-golang-di-linux/) and make sure to go get:
```bash
go get github.com/gorilla/websocket
```
### Bagaimana Skenarionya?
1. Klien menghubungkan koneksi ke server dengan browser.
2. Server memberikan respon tampilan web kepada klien.
3. Klien terkoneksi dengan server melalui websocket dengan javascript.
4. Server menerimanya dengan standar `http handler`.
5. Membuat koneksi websocker dari `http`.
6. Berkomunikasi dengan klien.
7. Memutuskan koneksi.

### Persiapan
Sebelum membuat code go, kita akan membuat tampilannya terlebih dahulu, sebut saja index.html buat file dengan nama index.html lalu isi file tersebut dengan :
```html
<!DOCTYPE HTML>
<html>
<head>
  <script type="text/javascript">
  function myWebsocketStart() {
      var ws = new WebSocket("ws://localhost:3000/websocket");
      ws.onmessage = function (evt) {
          var myTextArea = document.getElementById("msg");
          myTextArea.value = myTextArea.value + "\n" + evt.data
      };

      ws.onclose = function() {
          var myTextArea = document.getElementById("textarea1");
          myTextArea.value = myTextArea.value + "\n" + "Connection closed";
      };
  }
  </script>
</head>
<body>
<button onclick="javascript:myWebsocketStart()">Subscribe</button>
<textarea id="msg">My message ...</textarea>
</body>
</html>
```

Terimakasih, Semoga bermanfaat

Sumber : jacobmartins.com, bertzzie.com