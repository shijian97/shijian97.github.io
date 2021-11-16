---
layout: 
title: 数组杂谈
date: 2021-08-31 09:45:41
tags:
- python
- tuple
- list
- ndarray
- numpy
categories:
- [学习, coding]
---

python以及pytorch中许多都会涉及到数组或者列表，需加以区分。
<!-- more -->
### 总述
常用数据结构包括**元组** **列表** **字典** **集合**等，这些属于内建函数，而numpy的数组**ndarray**也是在机器学习中常用的，下面会记录较为重要的**元组** **列表** **数组**

### 元组
元组是一种固定长度，不可变的对象序列。
- `tuple()`将任意序列或者迭代器转换为元组
```
In []: tuple([4, 0, 2])
Out[]: (4, 0, 2)
```

- 用`+`号或者`*`号来链接元组
```
In []: (4, None, 'foo') + (6, 0)
Out[]: (4, None, 'foo', 6, 2)

In []: (4, 0, 2) * 2
Out[]: (4, 0, 2, 4, 0, 2)
```

- 由于元组的内容与长度无法改变，因此实例方法较少。常见的有`count`方法，作用是计算某个数值在元组中出现的次数
```
In []: a = (4, 2, 0, 2, 2)
In []: a.count(2)
Out[]: 3
```

- 使用[i]索引获取元组指定位置i的对象

### 列表
与元组不同，列表长度可变，内容可修改。

- 使用中括号[]或者list类型来定义列表
```
In []:a_list = [2, 3, None]
In []:tup = 2, 3, None
In []:b_list = list(tup)
```

- 函数用法和tuple相似
- 可以将迭代器或者生成器变为列表
```
In []: gen = range(10)
In []: list(gen)
Out[]: [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
```

- 元素的增添或移除
	-- `.append()`将元素添加到列表尾部
	-- `.insert(loc, obj)`在loc位置添加obj
	-- `.remove()`移除第一个符合要求的值
	-- `.pop()`将特定元素移除并返回
```
In []: a = [1, 2, 3, 4]
In []: a.append(5)
Out[]: [1, 2, 3, 4, 5]
In []: a.insert(1, 999)
Out[]: [1, 999, 2, 3, 4, 5]
```

- 列表的链接和移除
	-- 与元组类似，两个列表之间使用+号连接
	-- 若有已经定义的数据集，可以用extend向该列表添加元素
```
In []: a = [1, 2, 3, 4]
In []: a.extend(['foo', None, (1,2)])
In []: a
Out[]: [1, 2, 3, 4, 'foo', None, (1,2)]
```

- 排序 sort
```
In []: a = [2, 3, 1, 4]
In []: a.sort()
In []: a
Out[]: [1, 2, 3, 4]
```
sort函数会有二级排序key
```
In []: a = ['a', 'asd', 'asdf', 'as']
In []: a.sort(key=len)
In []: a
Out[]: ['a', 'as', 'asd', 'asdf']
```

- 切片start包含，stop不包含
```
In []: a = [2, 3, 1, 4]
In []: a[-1:0]
Out[]: [4]
In []: a[::2] #步长
Out[]: [2,1]
In []: a[::-1] #相当于翻转
Out[]: [4, 1, 2, 3]
```

### ndarray
import numpy as np
numpy方法比python内建方法快速、占用内存小，用C语言写成。
较为重要的通用属性：shape属性表征*每一维度的数量*，ndim属性描述*数组的维数*，dtype属性描述*数组的数据类型*。
- 生成ndarray
	-- 使用array函数
```
In []: data_a = [1, 2, 3, 4]
In []: ary_a = np.array(data_a)
In []: a
Out[]: array([[1, 2, 3, 4]])
```
	-- 使用`np.zeros(num)`可以生成num数量够的全0数组，`np.ones(num)`同理。
	-- `np.arange()`为内建函数的range数组版
	默认类型为float64.
- ndarray的数据类型
使用astype方法显式转换数组
```
In []: arr = np.array([1, 2, 3, 4])
In []: arr.dtype
Out[]: dtype('int64')
In []: float_arr = arr.astype(np.float64)
In []: float_arr.dtype
Out[]: dtype('float64')
```
- 基础切片与索引与列表同
- 数组转置和换轴
	-- **transpose**方法
	-- **T**属性（转置）
- 一元与二元通用函数：对数组每个元素进行计算