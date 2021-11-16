---
layout: 
title: matplotlib基础
date: 2021-08-31 21:40:52
tags:
- python
- matplotlib
categories:
- [学习, coding]
---

学习matplotlib的应用
<!-- more -->

### 简单matplotlibAPI
导入
`In [0]: import matplotlib.pyplot as plt`
简单的使用
```
In [1]: import numpy as np
In [2]: data = np.arange(10)
In [3]: plt.plot(data)
```

#### 图片与子图
matplotlib绘制的图片位于Figure对象中，使用plt.figure生成一个新图片。
使用subplot生成2X2的子图并布置在左上角的位置
```
In [4]: fig = plt.figure()
In [5]: ax1 = fig.add_subplot(2,2,1)
```

#### 画图
- matplotlib主函数接收带有x和y轴的数组以及参数。
`ax.plot(x,y,linestyle='--',color='g')`或者`ax.plot(x,y,'g--')`

- 设置刻度标签和图例
```
ticks = ax.set_xticks([0,250,500,750,1000])
labels = ax.set_xticklabels(['one','two','three','four','five'])
ax.set_title('My PLT')
ax.set_xlabel('Stage')
```

### 使用pandas
Series和DataFrame都有一个plot属性，用于绘制图形，默认为折线图，有一些简单的方法参数。
#### Series.plot()方法参数
|参数  |描述   |
|:-----|:------|
|label|图里标签|
|ax   |子图对象|
|style|样式   |
|kind|area bar barh density hist kde line pie等|
|logy|y轴对数缩放|
|xticks|x的刻度值|
|yticks|y的刻度值|
|xlim|x轴范围|
|ylim|y轴范围|

#### DataFrame.plot()参数
|参数  |描述   |
|:-----|:------|
|subplots|将df每一列绘制在独立子图中|
|sharex|subplots=True共享x轴|
|sharey|subplots=True共享y轴|
|title|标题|

#### 柱状图
plot.bar()和plot.barh()分别绘制垂直和水平的柱状图