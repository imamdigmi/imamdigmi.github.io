---
date: 2018-03-05T16:17:26+07:00
lastmod: 2018-03-05T16:17:26+07:00

title: "Machine Learning Dengan Tensorflow Part 1"
description: "Belajar Machine Learning dengan TensorFlow Part 1 Eksplorasi dan Transformasi Data"
author: "Imam Digmi"

weight: 50
draft: true
keywords: [tensorflow, machine learning, data]
tags: [tensorflow, machine learning]
categories: [Machine Learning]
---

# Eksplorasi dan Transformasi Data
Bab pertama ini kita akan membahas mengenai _Eksplorasi_ dan _Transformasi Data_ yang bertujuan agar pembaca dapat memahami konsep utama dari TensorFlow.

## Tensors
## Properti
### Rank
```python
import tensorflow as tf
sess = tf.Session()
tens = tf.constant([[[1,2],[2,3]],[[3,4],[5,6]]])
print(sess.run(tens)[1,1,0])

# Output
5
```
| Rank | Entitas (Math) | Penjelasan Contoh Kode                                            |
|:----:|:---------------|:------------------------------------------------------------------|
| 0    | Scalar         | `scalar = 1000`                                                   |
| 1    | Vector         | `vector = [2, 8, 3]`                                              |
| 2    | Matrix         | `matrix = [[4, 2, 1], [5, 3, 2], [5, 5, 6]]`                      |
| 3    | 3-tensor       | `tensor = [[[4], [3], [2]], [[6], [100], [4]], [[5], [1], [4]]]`  |
| n    | n-tensor       | ...                                                               |

### Shapes
| Rank | Shape                | Nomor Dimensi | Contoh                                         |
|:----:|:---------------------|:-------------:|:-----------------------------------------------|
| 0    | `[]`                 | 0             | `4`                                            |
| 1    | `[D0]`               | 1             | `[2]`                                          |
| 2    | `[D0, D1]`           | 2             | `[6, 2]`                                       |
| 3    | `[D0, D1, D2]`       | 3             | `[7, 3, 2]`                                    |
| n    | `[D0, D1, ... Dn-1]` | n-D           | `Sebuah tensor dengan shape [D0, D1, … Dn-1]`  |

```python
import tensorflow as tf
tens = tf.constant([[[1, 2], [2, 3]], [[3, 4], [5, 6]]])
tens

# Output
<tf.Tensor 'Const_1:0' shape=(2, 2, 2) dtype=int32>
```

### Types

| Tipe Data    | Tipe Data (Dalam Python) | Keterangan                                                             |
|:-------------|:-------------------------|:-----------------------------------------------------------------------|
| `DT_FLOAT`   | `tf.float32`             | 32 bits floating point.                                                |
| `DT_DOUBLE`  | `tf.float64`             | 64 bits floating point.                                                |
| `DT_INT8`    | `tf.int8`                | 8 bits signed integer.                                                 |
| `DT_INT16`   | `tf.int16`               | 16 bits signed integer.                                                |
| `DT_INT32`   | `tf.int32`               | 32 bits signed integer.                                                |
| `DT_INT64`   | `tf.int64`               | 64 bits signed integer.                                                |
| `DT_UINT8`   | `tf.uint8`               | 8 bits unsigned integer.                                               |
| `DT_STRING`  | `tf.string`              | Variable length byte arrays. Each element of a tensor is a byte array. |
| `DT_BOOL`    | `tf.bool`                | Boolean.                                                               |

### Membuat Tensor Baru

```python
import tensorflow as tf
import numpy as np
x = tf.constant(np.random.rand(32).astype(np.float32))
y = tf.constant ([1,2,3])
x
y

# Output
<tf.Tensor 'Const:0' shape=(32,) dtype=float32>
<tf.Tensor 'Const_1:0' shape=(3,) dtype=int32>
```

### Numpy to Tensors
```python
import tensorflow as tf
import numpy as np
sess = tf.Session() # start a new Session Object
x_data = np.array([[1., 2., 3.],[3., 2., 6.]]) # 2x3 matrix
x = tf.convert_to_tensor(x_data, dtype=tf.float32)
print(x)

# Output
Tensor("Const_2:0", shape=(2, 3), dtype=float32)
```