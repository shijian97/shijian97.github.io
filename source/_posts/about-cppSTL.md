---
title: C++STL容器
date: 2021-11-22 13:22:58
tags:
- C++
- STL
categories:
- [学习, coding]
---

C++中STL常见容器的一些常用方法。
<!-- more -->

### vector
头文件 `include<vector>`
初始化 `vector<struc> vec`

| 函数名  | 作用   | 返回值   |
| :-----| :-----| :--------|
| vec.size() | 返回vector长度 | int |
| vec.empty() | 判断vector是否为空 | bool |
| vec.push_back(x) | 尾部添加元素 | 无返回值 |
| vec.pop_back() | 删除尾部元素 | 无返回值 |
| vec.begin() | 指向首位置的迭代器 | vector<struc>::iterator iter = vec.begin() |
| vec.end() | 返回最后一个元素的下一个位置的迭代器 | 同上 |
| vec[0] | 返回首位置的值 | struc |

### queue
头文件 `include<queue>`
初始化 `queue<struc> que`
| 函数名  | 作用   | 返回值   |
| :-----| :-----| :--------|
