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

### 顺序容器
顺序容器有以下三种：可变长动态数组 vector、双端队列 deque、双向链表 list。
它们之所以被称为顺序容器，是因为元素在容器中的位置同元素的值无关，即容器不是排序的。将元素插入容器时，指定在什么位置（尾部、头部或中间某处）插入，元素就会位于什么位置。

#### vector
头文件 `include<vector>`
初始化 `vector<struc> vec;`

| 函数名  | 作用   | 返回值   |
| :-----| :-----| :--------|
| vec.size() | 返回vector长度 | int |
| vec.empty() | 判断vector是否为空 | bool |
| vec.push_back(x) | 尾部添加元素 | 无返回值 |
| vec.pop_back() | 删除尾部元素 | 无返回值 |
| vec.begin() | 指向首位置的迭代器 | vector<struc>::iterator iter = vec.begin() |
| vec.end() | 返回最后一个元素的下一个位置的迭代器 | 同上 |
| vec.front() | 返回第一个元素的引用 | struc |
| vec.back() | 返回最后一个元素的引用 | struc |
| vec[0] | 返回首位置的值 | struc |

#### deque（双端队列）
头文件 `include<deque>`
初始化 `deque<struc> deq;`

| 函数名  | 作用   | 返回值   |
| :-----| :-----| :--------|
| deq.size() | 返回deque长度 | int |
| deq.empty() | 判断deque是否为空 | bool |
| deq.push_back(x) | 尾部添加元素 | 无返回值 |
| deq.push_front(x) | 头部添加元素 | 无返回值 |
| deq.pop_back() | 删除尾部元素 | 无返回值 |
| deq.pop_front() | 删除头部元素 | 无返回值 |
| deq.begin() | 指向首位置的迭代器 | deque<struc>::iterator iter = deq.begin() |
| deq.end() | 返回最后一个元素的下一个位置的迭代器 | 同上 |
| deq.front() | 返回第一个元素的引用 | struc |
| deq.back() | 返回最后一个元素的引用 | struc |

#### list
头文件 `include<list>`
初始化 `list<struc> li;`

| 函数名  | 作用   | 返回值   |
| :-----| :-----| :--------|
| li.size() | 返回li长度 | int |
| li.empty() | 判断li是否为空 | bool |
| li.push_back(x) | 尾部添加元素 | 无返回值 |
| li.push_front(x) | 头部添加元素 | 无返回值 |
| li.pop_back() | 删除尾部元素 | 无返回值 |
| li.pop_front() | 删除头部元素 | 无返回值 |
| li.begin() | 指向首位置的双向迭代器 | list<struc>::iterator iter = li.begin() |
| li.end() | 返回最后一个元素的下一个位置的双向迭代器 | 同上 |
| li.front() | 返回第一个元素的引用 | struc |
| li.back() | 返回最后一个元素的引用 | struc |

顺序容器总结有如下常用成员函数
front()：返回容器中第一个元素的引用。
back()：返回容器中最后一个元素的引用。
push_back()：在容器末尾增加新元素。
pop_back()：删除容器末尾的元素。
insert(...)：插入一个或多个元素。该函数参数较复杂，此处省略。

### 容器适配器
栈 stack、队列 queue、优先级队列 priority_queue。

#### queue
特点：先进先出

头文件 `include<queue>`
初始化 `queue<struc> que`

| 函数名  | 作用   | 返回值   |
| :-----| :-----| :--------|
| que.size() | 返回que长度 | int |
