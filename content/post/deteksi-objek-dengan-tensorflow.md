---
date: 2018-01-24T16:30:15+07:00
lastmod: 2018-01-24T16:30:15+07:00

title: "Deteksi Objek dengan TensorFlow"
description: "Membuat pendeteksi obyek menggunakan API TensorFlow"
author: "Imam Digmi"

weight: 50
draft: false
keywords: [tensorflow, object detection, api, python, deep learning]
tags: [tensorflow, python]
categories: [TensorFlow]
---

# Mempersiapkan Dataset

## Membuat Anotation
Untuk memuat _anotation_ atau memberikan label pada gambar kita akan menggunakan aplikasi [LabelImg](https://github.com/tzutalin/labelImg) yang nantinya akan disimpan kedalam file `XML` dengan format [PASCAL VOC](http://www.image-net.org/)
Cara membuat anotasinya :

1. Buka aplikasi `labelImg`
2. Klik tombol "OpenDir"
3. Pilih direktori dataset gambar
4. Klik tombol "Create RectBox" untuk membuat kotak area objek yang akan dikenali
5. Arahkan kursos dan tarik area kotak disekitar objek
6. Lalu akan muncul kotak dialog untuk memberikan nama label dari objek yang akan kita kenali
7. Simpan hasil pelabelan dengan menekan tombol `CTRL+S`
8. Pilih direktori penyimpanan file hasil pelabelan `*.xml`

>Note: Nama label objek yang dikenali harus sama `Case Sensitive`.

![Pelabelan Gambar](/images/deteksi-objek-dengan-tensorflow/pelabelan-gambar.png)

## Convert XML ke CSV
```python
import os
import glob
import pandas as pd
import xml.etree.ElementTree as ET

def xml_to_csv(path):
    xml_list = []
    for xml_file in glob.glob(path + '/*.xml'):
        tree = ET.parse(xml_file)
        root = tree.getroot()
        for member in root.findall('object'):
            value = (root.find('filename').text,
                     int(root.find('size')[0].text),
                     int(root.find('size')[1].text),
                     member[0].text,
                     int(member[4][0].text),
                     int(member[4][1].text),
                     int(member[4][2].text),
                     int(member[4][3].text)
                     )
            xml_list.append(value)
    column_name = ['filename', 'width', 'height', 'class', 'xmin', 'ymin', 'xmax', 'ymax']
    xml_df = pd.DataFrame(xml_list, columns=column_name)
    return xml_df

def main():
    for directory in ['train', 'test']:
        image_path = os.path.join(os.getcwd(), 'annotations/{}'.format(directory))
        xml_df = xml_to_csv(image_path)
        xml_df.to_csv('data/{}_labels.csv'.format(directory), index=None)
        print('Successfully converted xml to csv.')

main()
```
Simpan kode program berikut dengan nama `xml_to_csv.py`. Lalu jalankan perintah dibawah ini untuk mengkonversi file XML ke CSV.

```bash
$ python3 xml_to_csv.py
```

## Convert TFRecord
```python
from __future__ import division
from __future__ import print_function
from __future__ import absolute_import
import os
import io
import pandas as pd
import tensorflow as tf
from PIL import Image
from object_detection.utils import dataset_util
from collections import namedtuple, OrderedDict

flags = tf.app.flags
flags.DEFINE_string('type', '', 'Type of CSV input (train/test)')
flags.DEFINE_string('csv_input', '', 'Path to the CSV input')
flags.DEFINE_string('output_path', '', 'Path to output TFRecord')
FLAGS = flags.FLAGS

def class_text_to_int(row_label):
    if row_label == 'plate':
        return 1
    else:
        None

def split(df, group):
    data = namedtuple('data', ['filename', 'object'])
    gb = df.groupby(group)
    return [data(filename, gb.get_group(x)) for filename, x in zip(gb.groups.keys(), gb.groups)]

def create_tf_example(group, path):
    with tf.gfile.GFile(os.path.join(path, '{}'.format(group.filename)), 'rb') as fid:
        encoded_jpg = fid.read()
    encoded_jpg_io = io.BytesIO(encoded_jpg)
    image = Image.open(encoded_jpg_io)
    width, height = image.size

    filename = group.filename.encode('utf8')
    image_format = b'jpg'
    xmins = []
    xmaxs = []
    ymins = []
    ymaxs = []
    classes_text = []
    classes = []

    for index, row in group.object.iterrows():
        xmins.append(row['xmin'] / width)
        xmaxs.append(row['xmax'] / width)
        ymins.append(row['ymin'] / height)
        ymaxs.append(row['ymax'] / height)
        classes_text.append(row['class'].encode('utf8'))
        classes.append(class_text_to_int(row['class']))

    tf_example = tf.train.Example(features=tf.train.Features(feature={
        'image/height': dataset_util.int64_feature(height),
        'image/width': dataset_util.int64_feature(width),
        'image/filename': dataset_util.bytes_feature(filename),
        'image/source_id': dataset_util.bytes_feature(filename),
        'image/encoded': dataset_util.bytes_feature(encoded_jpg),
        'image/format': dataset_util.bytes_feature(image_format),
        'image/object/bbox/xmin': dataset_util.float_list_feature(xmins),
        'image/object/bbox/xmax': dataset_util.float_list_feature(xmaxs),
        'image/object/bbox/ymin': dataset_util.float_list_feature(ymins),
        'image/object/bbox/ymax': dataset_util.float_list_feature(ymaxs),
        'image/object/class/text': dataset_util.bytes_list_feature(classes_text),
        'image/object/class/label': dataset_util.int64_list_feature(classes),
    }))
    return tf_example

def main(_):
    writer = tf.python_io.TFRecordWriter(FLAGS.output_path)
    path = os.path.join(os.getcwd(), 'images/{}'.format(FLAGS.type))
    examples = pd.read_csv(FLAGS.csv_input)
    grouped = split(examples, 'filename')
    for group in grouped:
        tf_example = create_tf_example(group, path)
        writer.write(tf_example.SerializeToString())

    writer.close()
    output_path = os.path.join(os.getcwd(), FLAGS.output_path)
    print('Successfully created the TFRecords: {}'.format(output_path))

if __name__ == '__main__':
    tf.app.run()
```
Simpan kode program berikut dengan nama `generate_tfrecord.py`. Lalu jalankan perintah dibawah ini untuk men-generate TFRecord file.

```bash
$ python3 generate_tfrecord.py --type=train --csv_input=data/train_labels.csv  --output_path=data/train.record
// dan
$ python3 generate_tfrecord.py --type=test --csv_input=data/test_labels.csv  --output_path=data/test.record
```

## Membuat Label Map
```json
item {
    id: 1
    name: 'plate'
}
```
Simpan konfigurasi label map tersebut dengan nama `object-detection.pbtxt` kedalam folder `data`.

## Konfiguras Pipeline
Karena tensorflow menggunakan ProtoBuf maka kita perlu membuat konfigurasi pipeline dan kita akan menggunakan konfigurasi dari [SSD with Mobilenet v1](https://github.com/tensorflow/models/blob/master/research/object_detection/samples/configs/ssd_mobilenet_v1_pets.config) yang digunakan oleh Oxford-IIIT untuk Dataset hewan peliharaan. Contoh konfigurasi dapat dilihat [disini](https://github.com/tensorflow/models/blob/master/research/object_detection/samples/configs).

```json
model {
  ssd {
    num_classes: 1
    box_coder {
      faster_rcnn_box_coder {
        y_scale: 10.0
        x_scale: 10.0
        height_scale: 5.0
        width_scale: 5.0
      }
    }
    matcher {
      argmax_matcher {
        matched_threshold: 0.5
        unmatched_threshold: 0.5
        ignore_thresholds: false
        negatives_lower_than_unmatched: true
        force_match_for_each_row: true
      }
    }
    similarity_calculator {
      iou_similarity {
      }
    }
    anchor_generator {
      ssd_anchor_generator {
        num_layers: 6
        min_scale: 0.2
        max_scale: 0.95
        aspect_ratios: 1.0
        aspect_ratios: 2.0
        aspect_ratios: 0.5
        aspect_ratios: 3.0
        aspect_ratios: 0.3333
      }
    }
    image_resizer {
      fixed_shape_resizer {
        height: 300
        width: 300
      }
    }
    box_predictor {
      convolutional_box_predictor {
        min_depth: 0
        max_depth: 0
        num_layers_before_predictor: 0
        use_dropout: false
        dropout_keep_probability: 0.8
        kernel_size: 1
        box_code_size: 4
        apply_sigmoid_to_scores: false
        conv_hyperparams {
          activation: RELU_6,
          regularizer {
            l2_regularizer {
              weight: 0.00004
            }
          }
          initializer {
            truncated_normal_initializer {
              stddev: 0.03
              mean: 0.0
            }
          }
          batch_norm {
            train: true,
            scale: true,
            center: true,
            decay: 0.9997,
            epsilon: 0.001,
          }
        }
      }
    }
    feature_extractor {
      type: 'ssd_mobilenet_v1'
      min_depth: 16
      depth_multiplier: 1.0
      conv_hyperparams {
        activation: RELU_6,
        regularizer {
          l2_regularizer {
            weight: 0.00004
          }
        }
        initializer {
          truncated_normal_initializer {
            stddev: 0.03
            mean: 0.0
          }
        }
        batch_norm {
          train: true,
          scale: true,
          center: true,
          decay: 0.9997,
          epsilon: 0.001,
        }
      }
    }
    loss {
      classification_loss {
        weighted_sigmoid {
          anchorwise_output: true
        }
      }
      localization_loss {
        weighted_smooth_l1 {
          anchorwise_output: true
        }
      }
      hard_example_miner {
        num_hard_examples: 3000
        iou_threshold: 0.99
        loss_type: CLASSIFICATION
        max_negatives_per_positive: 3
        min_negatives_per_image: 0
      }
      classification_weight: 1.0
      localization_weight: 1.0
    }
    normalize_loss_by_num_matches: true
    post_processing {
      batch_non_max_suppression {
        score_threshold: 1e-8
        iou_threshold: 0.6
        max_detections_per_class: 100
        max_total_detections: 100
      }
      score_converter: SIGMOID
    }
  }
}

train_config: {
  batch_size: 8
  optimizer {
    rms_prop_optimizer: {
      learning_rate: {
        exponential_decay_learning_rate {
          initial_learning_rate: 0.004
          decay_steps: 800720
          decay_factor: 0.95
        }
      }
      momentum_optimizer_value: 0.9
      decay: 0.9
      epsilon: 1.0
    }
  }
  fine_tune_checkpoint: "ssd_mobilenet_v1_coco_2017_11_17/model.ckpt"
  from_detection_checkpoint: true
  num_steps: 25000
  data_augmentation_options {
    random_horizontal_flip {
    }
  }
  data_augmentation_options {
    ssd_random_crop {
    }
  }
}

train_input_reader: {
  tf_record_input_reader {
    input_path: "data/train.record"
  }
  label_map_path: "data/object-detection.pbtxt"
}

eval_config: {
  num_examples: 2000
  max_evals: 10
}

eval_input_reader: {
  tf_record_input_reader {
    input_path: "data/test.record"
  }
  label_map_path: "data/object-detection.pbtxt"
  shuffle: false
  num_readers: 1
}
```
Simpan file konfigurasi diatas dengan nama `plate_number_v1.config` kedalam folder `training`. Sampai ditahap ini kita sudah selesai menyiapkan semua file yang dibutuhkan pada saat training model object detection kita.

> Note: Pastikan struktur direktori dan file dengan benar.

Berikut sususan akhir direktori yang penulis gunakan :
```
.
├── annotations
│   ├── test
│   │   ├── IMG-20180108-WA0115.xml
│   │   ├── ...
│   │   ├── IMG-20180108-WA0120.xml
│   │   └── IMG-20180108-WA0121.xml
│   └── train
│       ├── 100.E 6467 QW-09-20.xml
│       ├── ...
│       ├── 255.E 6527 OD-02-20.xml
│       └── 256.E 4572 SS-01-16.xml
├── data
│   ├── object-detection.pbtxt
│   ├── test.record
│   ├── test_labels.csv
│   ├── train.record
│   └── train_labels.csv
├── images
│   ├── test
│   │   ├── IMG-20180108-WA0115.jpeg
│   │   ├── ...
│   │   ├── IMG-20180108-WA0120.jpg
│   │   └── IMG-20180108-WA0121.jpg
│   └── train
│       ├── 100.E 6467 QW-09-20.jpeg
│       ├── ...
│       ├── 255.E 6527 OD-02-20.jpg
│       └── 256.E 4572 SS-01-16.jpg
├── training
│   └── plate_number_v1.config
├── generate_tfrecord.py
└── xml_to_csv.py

8 directories, 1016 files
```

# Training Model
Setelah semua file yang dibutuhkan selesai dipersiapkan maka kita akan memasuki tahap training Neural Network untuk mengenali pola Tanda Nomor Kendaraan Bermotor (TNKB). Download [TensorFlow Model](https://github.com/tensorflow/models/) lalu ekstrak folder models.

Jalankan perintah dibawah ini untuk membuat `PYTHONPATH` variable

```bash
$ cd models/research
$ export PYTHONPATH=$PYTHONPATH:`pwd`:`pwd`/slim
```

Jalankan perintah dibawah ini untuk mengkompilasi ProtoBuf yang akan kita butuhkan pada saat proses training.

```bash
$ protoc object_detection/protos/*.proto --python_out=.
$ cd object_detection
```

Setelah kita menyiapkan TensorFlow Model kita perlu menyalin berkas-berkas yang kita buat sebelumnya untuk kebutuhan proses trainig.

1. Salin direktori `data`, `images`, dan `training` yang kita buat sebelumnya ke dalam folder `models/research/object_detection`.
2. Setelah semua berkas sudah disalin kita akan menggunakan pre-trained model yang bernama `SSD Mobilnet v1` dari [COCO](http://cocodataset.org/) model tersebut bisa diunduh [disini](http://download.tensorflow.org/models/object_detection/ssd_mobilenet_v1_coco_2017_11_17.tar.gz), jika sudah berhasil diunduh lalu ekstrak `ssd_mobilenet_v1_coco_2017_11_17.tar.gz` kedalam direktori `models/research/object_detection`.

Jika kita melihat ke daftar [COCO-trained Model](https://github.com/tensorflow/models/blob/master/research/object_detection/g3doc/detection_model_zoo.md#coco-trained-models-coco-models) maka akan ada banyak model yang tersedia dan silahkan pilih sendiri mana yang akan kita gunakan untuk training Neural Network kita, tapi penulis akan memilih yang tercepat yaitu `ssd_mobilenet_v1_coco` yang mana kecepatannya sampai 30/milisecond.

Baik! setelah itu kita mulai training Neural Network, jalankan perintah dibawah ini untuk memulai proses training.

```bash
python3 train.py --logtostderr --train_dir=training/ --pipeline_config_path=training/plate_number_v1.config
```

![Training Process](/images/deteksi-objek-dengan-tensorflow/training.png)

## Grafik TensorBoard
Untuk melihat grafik selama proses training kita akan menggunakan TensorBoard, jalankan perintah dibawah ini untuk menjalankan TensorBoard.

```bash
tensorboard --logdir=training
```

Kurang lebih tampilan TensorBoard akan seperti dibawah ini

![TensorBoard Stats](/images/deteksi-objek-dengan-tensorflow/tensorboard.png)

![TensorBoard Graph](/images/deteksi-objek-dengan-tensorflow/tensorboard-graph.png)

## Export Graph
Proses training diatas belum sepenuhnya selesai karena tujuan utama dari proses training Neural Network adalah sebuah model yang akan kita gunakan untuk mendeteksi objek-objek yang telah kita masukkan sebelumnya, maka dari itu kita perlu mengekspor modelnya, jalankan perintah berikut untuk mengekspor model.

```bash
python3 export_inference_graph.py \
    --input_type image_tensor \
    --pipeline_config_path training/ssd_mobilenet_v1_pets.config \
    --trained_checkpoint_prefix training/model.ckpt-25000 \
    --output_directory plate_model_exported
```

# Testing
Ini adalah tahap terakhir yang kita lalui dan kita akan mencoba menggunakan model yang telah kita latih untuk mengenali Tanda Nomor Kendaraan Bermotor (TNKB) sebelumnya, berikut kode untuk menguji model 
```python

```

Dan hasilnya akan seperti dibawah ini

![Results Detection](/images/deteksi-objek-dengan-tensorflow/result-1.png)
![Results Detection](/images/deteksi-objek-dengan-tensorflow/result-2.png)