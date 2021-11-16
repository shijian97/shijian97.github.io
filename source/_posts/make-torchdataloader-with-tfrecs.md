---
layout: 
title: 使用tfrecords制作pytorch的dataloader 双框架混用
date: 2021-08-25
tags:
- tensorflow
- tfrecords
- pytorch
- dataloader
- iterator
categories:
- [学习, coding]
---

在kaggle比赛的时候，有时候会需要读取tfrecords文件，而我使用的是torch的框架，此时需要通过tfrecords制作dataset和dataloader。解决这个问题第一是用了tfrecord库，第二是通过kaggle的一篇discussion学习到重写dataloader的方法。
<!-- more -->

### 1 tfrecords文件读取

#### 1.1 tfrecords文件的结构

TFRecords是一种tensorflow的内定标准文件格式，其实质是二进制文件，遵循protocol buffer协议（谷歌的一种数据交换格式），是Google专门为TensorFlow设计的一种数据格式。
tfrecord内部包含了多个 `tf.train.Example`， 而 `Example` 是protocol buffer(protobuf) 数据标准的实现，在一个 `Example` 消息体中包含了一系列的 `tf.train.feature` 属性，而 每一个 `feature` 是一个 `key-value` 的键值对，其中，key 是string类型，而value 的取值有三种：
-` bytes_list`: 可以存储string 和byte两种数据类型。
-` float_list`: 可以存储float(float32)与double(float64) 两种数据类型 。
- `int64_list`: 可以存储：bool, enum, int32, uint32, int64, uint64 。

#### 1.2 数据写入

此部分简单 实例，通过代码了解即可

```
writer = tf.python_io.TFRecordWriter(out_file_name)  # 1. 定义 writer对象

for data in dataes:
    context = dataes[0]
    question = dataes[1]
    answer = dataes[2]

    """ 2. 定义features """
   example = tf.train.Example (
    features=tf.train.Features(
        feature={
            'name' : tf.train.Feature(bytes_list=tf.train.BytesList (value=[splits[-1].encode('utf-8')])),
            'label': tf.train.Feature(int64_list=tf.train.Int64List (value=[int(label)])),
            'shape': tf.train.Feature(int64_list=tf.train.Int64List (value=[img.shape[0], img.shape[1], img.shape[2]])),
            'data' : tf.train.Feature(bytes_list=tf.train.BytesList (value=[bytes(img.numpy())]))
        }
    )
)

    
    """ 3. 序列化,写入"""
    serialized = example.SerializeToString()
    writer.write(serialized)
```

#### 1.3 数据读取

通过写入的example指定参数解析tfrecord

```
reader = tf.data.TFRecordDataset(file_name)

feature_description = {
    'name' : tf.io.FixedLenFeature([], tf.string, default_value='Nan'),
    'label': tf.io.FixedLenFeature([] , tf.int64, default_value=-1),
    'shape': tf.io.FixedLenFeature([3], tf.int64),
    'data' : tf.io.FixedLenFeature([], tf.string)
}
def _parse_function (exam_proto):
    return tf.io.parse_single_example (exam_proto, feature_description)

reader = reader.repeat (1) # 读取数据的重复次数为：1次，这个相当于epoch
reader = reader.shuffle (buffer_size = 2000) # 在缓冲区中随机打乱数据
reader = reader.map (_parse_function) # 解析数据
batch  = reader.batch (batch_size = 10) # 每10条数据为一个batch，生成一个新的Dataset

shape = []
batch_data_x, batch_data_y = np.array([]), np.array([])
for item in batch.take(1): # 测试，只取1个batch
    shape = item['shape'][0].numpy()
    for data in item['data']: # 一个item就是一个batch
        img_data = np.frombuffer(data.numpy(), dtype=np.uint8)
        batch_data_x = np.append (batch_data_x, img_data)
    for label in item ['label']:
        batch_data_y = np.append (batch_data_y, label.numpy())

```

### 2 读取tfrecord制作torch dataloader

上代码

```
def get_dataset(files, batch_size=16, repeat=False, cache=False, shuffle=False, labeled=True, return_image_ids=True):
    ds = tf.data.TFRecordDataset(files, num_parallel_reads=AUTO)
    if cache:
        ds = ds.cache()

    if repeat:
        ds = ds.repeat()

    if shuffle:
        ds = ds.shuffle(1024 * 2)
        opt = tf.data.Options()
        opt.experimental_deterministic = False
        ds = ds.with_options(opt)


    ds = ds.map(read_labeled_tfrecord, num_parallel_calls=AUTO)
    ds = ds.batch(batch_size)
    ds = ds.prefetch(AUTO)
    return tfds.as_numpy(ds)
```

这一部分相当于建了一个tf.data.TFRecordDataset，并且包含了shuffle、repeat、等操作，其核心部分**map**中的 `read_labeled_tfrecord` 就相当于上面的 `_parse_function`

```
tfrec_format = {
    "label": tf.io.FixedLenFeature([], tf.int64),
    "data": tf.io.FixedLenFeature([], tf.string),
    "id": tf.io.FixedLenFeature([], tf.string)
}
def read_labeled_tfrecord(example):
    example = tf.io.parse_single_example(example, tfrec_format)
    example['data'] = decode_wave(example['data'])
    return example
```

解析时，在本例中需要用到解码，因为在制作tfrecords时使用了 `raw = data.astype(np.float32).tobytes()` ，将三段4096长度的音频encode了，因此，解码函数为

```
def decode_wave(wave):
    wave = tf.reshape(tf.io.decode_raw(wave, tf.float32), (3, 4096))
    normalized_waves = []
    for i in range(3):
        normalized_waves.append(wave[i])
    wave = tf.stack(normalized_waves, axis=0)
    wave = tf.cast(wave, tf.float32)
    return wave
```

再利用dataset制作dataloader，先上代码
```
class TFRecordDataLoader:
    def __init__(self, files, batch_size=16, cache=False, train=True, repeat=False, shuffle=False, labeled=True, return_image_ids=True):
        self.ds = get_dataset(
            files, 
            batch_size=batch_size,
            cache=cache,
            repeat=repeat,
            shuffle=shuffle,
            labeled=labeled,
            return_image_ids=return_image_ids)
        
        if train:
            self.num_examples = count_data_items(files)
        else:
            self.num_examples = count_data_items_test(files)

        self.batch_size = batch_size
        self.labeled = labeled
        self.return_image_ids = return_image_ids
        self._iterator = None
    
    def __iter__(self):
        if self._iterator is None:
            self._iterator = iter(self.ds)
        else:
            self._reset()
        return self._iterator

    def _reset(self):
        self._iterator = iter(self.ds)

    def __next__(self):
        batch = next(self._iterator)
        return batch

    def __len__(self):
        n_batches = self.num_examples // self.batch_size
        if self.num_examples % self.batch_size == 0:
            return n_batches
        else:
            return n_batches + 1
```

其中最重要的几个方法涉及到了**iterator**，因为dataloader本身就是一个iterator，下面以此介绍

- `__iter__(self)`:  `self._iterator = iter(self.ds)` 将可迭代对象加载为成迭代器。
- `__next__(self)`: 获取下一个对象 `batch = next(self._iterator)`.
- `__len__(self)`: 返回迭代器的长度。