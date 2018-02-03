---
date: 2018-01-25T15:51:05+07:00
lastmod: 2018-01-25T15:51:05+07:00

title: "TensorFlow - Object Detection API"
description: "Membuat pendeteksi objek menggunakan API TensorFlow"
author: "Imam Digmi"

weight: 50
draft: false
keywords: [tensorflow, object detection, api, python, deep learning, machine learning]
tags: [tensorflow, python, deep learning, machine learning]
categories: [Deep Learning]
---

# Setup TensorFlow Model
Untuk menggunakan TensorFlow Object Detection API harus sudah terinstal package TensorFlow, jika belum baca artikel saya tentang [Instalasi TensorFlow](https://imamdigmi.github.io/post/tensorflow-installation/).

Pertama-tama kita perlu menginstal dependencies yang dibutuhkan, salah satunya adalah [ProtoBuf](https://github.com/google/protobuf) dan dependency lainnya
```
$ sudo sudo pacman -S protobuf
$ pip install --user pillow lxml jupyter matplotlib
```

Kemudian kita perlu mendownload TensorFlow Models [disini](https://codeload.github.com/tensorflow/models/zip/master) jika download sudah selesai ekstrak file `.zip` lalu masuk ke direktori `models/research`
```
$ cd models/research
```

API Deteksi Objek Tensorflow menggunakan ProtoBuf untuk mengkonfigurasi parameter model dan training. Sebelum masuk proses training, library ProtoBuf harus dikompilasi terlebih dahulu. Jalankan perintah berikut pada direktori `models/research`
```
$ protoc object_detection/protos/*.proto --python_out=.
```

Jalankan perintah berikut di dalam direktori `models/research` untuk mambahkan `PYTHONPATH` variable
```
export PYTHONPATH=$PYTHONPATH:`pwd`:`pwd`/slim
```

> Note: Perintah diatas harus dijalankan setiap kali kita ingin menggunakan API

Lebih lengkapnya dapat dilihat [disini](https://github.com/tensorflow/models/blob/master/research/object_detection/g3doc/installation.md).

# Deteksi Objek pada Gambar
Untuk memulai deteksi objek kita akan menggunakan Jupyter Notebook, jalankan perintah berikut 
```
$ jupyter notebook --notebook-dir=object_detection
```

1. Klik file `object_detection_tutorial.ipynb`
2. Klik menu `Cell`
3. Pilih menu `Run All`

Maka hasilnya akan seperti berikut

![Result Image 1](/images/tensorflow-object-detection-overview/result-image-1.png)
![Result Image 1](/images/tensorflow-object-detection-overview/result-image-2.png)

# Deteksi Objek via Webcam
Kita juga dapat menggunakan Webcam untuk mendeteksi objek secara _real-time_

1. Klik menu `File`
2. Pilih `Download as`
3. Pilih `Python (.py)`

Kemudian pindahkan file tersebut (`object_detection_tutorial.py`) kedalam direktori `models/research/object_detection`

Instal opencv
```
$ sudo pacman -S opencv
```

Instal library opencv-python
```
$ pip install --user opencv-python
```

Tambhakan kode berikut
```python
import cv2
cap = cv2.VideoCapture(0)
```

Hapus baris `get_ipython().run_line_magic('matplotlib', 'inline')`

Kemudian ubah kode berikut
```python
for image_path in TEST_IMAGE_PATHS:
    image = Image.open(image_path)
    # the array based representation of the image will be used later in order to prepare the
    # result image with boxes and labels on it.
    image_np = load_image_into_numpy_array(image)
```

Menjadi
```python
while True:
    ret, image_np = cap.read()
```

Kemudian ubah kode berikut
```python
plt.figure(figsize=IMAGE_SIZE)
plt.imshow(image_np)
```

Menjadi
```python
cv2.imshow('object detection', cv2.resize(image_np, (800,600)))
if cv2.waitKey(25) & 0xFF == ord('q'):
    cv2.destroyAllWindows()
    break
```

Full code
```python
import numpy as np
import os
import six.moves.urllib as urllib
import sys
import tarfile
import tensorflow as tf
import zipfile
from collections import defaultdict
from io import StringIO
from matplotlib import pyplot as plt
from PIL import Image

if tf.__version__ < '1.4.0':
    raise ImportError('Please upgrade your tensorflow installation to v1.4.* or later!')

import cv2
cap = cv2.VideoCapture(0)

sys.path.append("..")

from utils import label_map_util
from utils import visualization_utils as vis_util
MODEL_NAME = 'ssd_mobilenet_v1_coco_2017_11_17'
MODEL_FILE = MODEL_NAME + '.tar.gz'
DOWNLOAD_BASE = 'http://download.tensorflow.org/models/object_detection/'
PATH_TO_CKPT = MODEL_NAME + '/frozen_inference_graph.pb'
PATH_TO_LABELS = os.path.join('data', 'mscoco_label_map.pbtxt')
NUM_CLASSES = 90

opener = urllib.request.URLopener()
opener.retrieve(DOWNLOAD_BASE + MODEL_FILE, MODEL_FILE)
tar_file = tarfile.open(MODEL_FILE)
for file in tar_file.getmembers():
    file_name = os.path.basename(file.name)
    if 'frozen_inference_graph.pb' in file_name:
        tar_file.extract(file, os.getcwd())

detection_graph = tf.Graph()
with detection_graph.as_default():
    od_graph_def = tf.GraphDef()
    with tf.gfile.GFile(PATH_TO_CKPT, 'rb') as fid:
        serialized_graph = fid.read()
        od_graph_def.ParseFromString(serialized_graph)
        tf.import_graph_def(od_graph_def, name='')

label_map = label_map_util.load_labelmap(PATH_TO_LABELS)
categories = label_map_util.convert_label_map_to_categories(label_map, max_num_classes=NUM_CLASSES, use_display_name=True)
category_index = label_map_util.create_category_index(categories)

def load_image_into_numpy_array(image):
    (im_width, im_height) = image.size
    return np.array(image.getdata()).reshape(
        (im_height, im_width, 3)).astype(np.uint8)

PATH_TO_TEST_IMAGES_DIR = 'test_images'
TEST_IMAGE_PATHS = [ os.path.join(PATH_TO_TEST_IMAGES_DIR, 'image{}.jpg'.format(i)) for i in range(1, 3) ]
IMAGE_SIZE = (12, 8)

with detection_graph.as_default():
    with tf.Session(graph=detection_graph) as sess:
        image_tensor = detection_graph.get_tensor_by_name('image_tensor:0')
        detection_boxes = detection_graph.get_tensor_by_name('detection_boxes:0')
        detection_scores = detection_graph.get_tensor_by_name('detection_scores:0')
        detection_classes = detection_graph.get_tensor_by_name('detection_classes:0')
        num_detections = detection_graph.get_tensor_by_name('num_detections:0')
        while True:
            ret, image_np = cap.read()
            image_np_expanded = np.expand_dims(image_np, axis=0)
            (boxes, scores, classes, num) = sess.run(
                [detection_boxes, detection_scores, detection_classes, num_detections],
                feed_dict={image_tensor: image_np_expanded})
            vis_util.visualize_boxes_and_labels_on_image_array(
                image_np,
                np.squeeze(boxes),
                np.squeeze(classes).astype(np.int32),
                np.squeeze(scores),
                category_index,
                use_normalized_coordinates=True,
                line_thickness=8)
            cv2.imshow('object detection', cv2.resize(image_np, (800,600)))
            if cv2.waitKey(25) & 0xFF == ord('q'):
                cv2.destroyAllWindows()
                break
```

Jalankan dengan perintah berikut di dalam direktori `models/research/object_detection`
```
$ python object_detection_tutorial.py
```

> __Tip__: Tekan tombol `Q` atau `Esc` untuk menutup window webcame

Maka hasilnya akan seperti berikut
![Result Webcam](/images/tensorflow-object-detection-overview/result-webcam.png)
