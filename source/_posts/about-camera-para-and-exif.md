---
title: 通过EXIF计算相机内参
date: 2021-12-11 22:09:33
tags:
- 相机内参
- EXIF
categories:
- [学习, coding]
mathjax: true
---

通过图片的EXIF来进行相机内参数的计算。

<!-- more -->
### 相机内参
$$
Zp = KP{，其中p表示图像坐标系的点坐标，Z为深度，K为内参矩阵，P为空间点在相机坐标系下的坐标。}
$$
需要重点解决的就是相机内参矩阵K。K可以表示为
$$
K=\begin{bmatrix} 
  f_x & 0 & c_x \\
  0 & f_y & c_y \\
  0 & 0 & 1 \\
  \end{bmatrix}
  =\begin{bmatrix}
  f \over dx & 0 & c_x \\
  0 & f \over dy & c_y\\
  0 & 0 & 1\\
  \end{bmatrix}
$$
其中f表示焦距，单位mm，dx和dy分别表示图像x和y方向上每mm各占多少各像素。$ c_x c_y $表示图像坐标系中心的偏移距离，一般为图像长、宽的一半。

### EXIF及其与相机内参关系
EXIF可以通过Python库exifread读取。
可以读取focallength即焦距，还可以读取图像的长宽。那么f和$ c_x c_y $都可以计算出来了。
较难的是如何求dx、dy。

#### 方法1
通过exif信息读取相机型号，网上搜索对应相机的说明书，找到CCD的尺寸，若设为x、y，图像长L宽W，则$ dx = {L \over x } $ dy同理。
#### 方法2
exif信息中有一项为35mm焦距，其含义为ccd为35mm时焦距的长度，则根据其比例也可先算出ccd实际尺寸，再计算dx、dy。