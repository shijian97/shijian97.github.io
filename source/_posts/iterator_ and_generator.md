---
layout:
title: 迭代器与生成器 Iterator andgenerator
date:2021-08-25
tags:
- python
- dataloader
- pytorch
categories:
- [学习, coding]
---
关于iterator和generator的区别于联系。
<!-- more -->
### 引言

简单来说生成器是一种特殊的迭代器，而可迭代对象有下面三种：
- 迭代器 --> 生成器
- 序列（字符串、列表、元组）
- 字典

### 1 生成器

&emsp;&emsp;通过列表生成式，我们可以直接创建一个列表，但是，受到内存限制，列表容量肯定是有限的，而且创建一个包含100万个元素的列表，不仅占用很大的存储空间，如果我们仅仅需要访问前面几个元素，那后面绝大多数元素占用的空间都白白浪费了。

&emsp;&emsp;所以，如果列表元素可以按照某种算法推算出来，那我们是否可以在循环的过程中不断推算出后续的元素呢？这样就不必创建完整的list，从而节省大量的空间，在Python中，这种一边循环一边计算的机制，称为生成器：generator.

&emsp;&emsp;生成器是一个特殊的程序，可以被用作控制循环的迭代行为，python中生成器是迭代器的一种，使用yield返回值函数，每次调用**yield**会暂停，而可以使用 `next()` 函数和 `send()` 函数恢复生成器。

&emsp;&emsp;生成器类似于返回值为数组的一个函数，这个函数可以接受参数，可以被调用，但是，不同于一般的函数会一次性返回包括了所有数值的数组，生成器一次只能产生一个值，这样消耗的内存数量将大大减小，而且允许调用函数可以很快的处理前几个返回值，因此生成器看起来像是一个函数，但是表现得却像是迭代器。

```
#列表生成式
lis = [x*x for x in range(10)]
print(lis)
#生成器
generator_ex = (x*x for x in range(10))
print(generator_ex)
 
结果：
[0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
<generator object <genexpr> at 0x000002A4CBF9EBA0>
```

&emsp;&emsp;那么创建list和generator_ex，的区别是什么呢？从表面看就是[  ]和（）,但是结果却不一样，一个打印出来是列表（因为是列表生成式），而第二个打印出来却是`<generator object <genexpr> at 0x000002A4CBF9EBA0>`，要一个个打印出来，可以通过next（）函数获得generator的下一个返回值。

用generator实现斐波那契数列

```
def fib(max):
    n,a,b =0,0,1
    while n < max:
        yield b
        a,b =b,a+b
        n = n+1
    return 'done'
 
a = fib(10)
print(fib(10))
# out:<generator object fib at 0x0000023A21A34FC0>

print(a.__next__())
# out:1
print(a.__next__())
# out:1
print(a.__next__())
# out:2

for i in fib(6):
    print(i)
# out:1;1;2;3;5;8
```

### 2 迭代器

- **可迭代对象**（Iterable）
集合数据类型，如list,tuple,dict,set,str等
- **迭代器**（Iterator）
Python中一个实现了_iter_方法和_next_方法的类对象，就是迭代器。

&emsp;&emsp;生成器都是Iterator对象，但list、dict、str虽然是Iterable（可迭代对象），却不是Iterator（迭代器）。把list、dict、str等Iterable变成Iterator可以使用iter()函数判断

```
>>> from collections import Iterable
>>> isinstance([], Iterable)
True
>>> isinstance({}, Iterable)
True
>>> isinstance('abc', Iterable)
True
>>> isinstance((x for x in range(10)), Iterable)
True
>>> isinstance(100, Iterable)
False
```

```
>>> from collections import Iterator
>>> isinstance((x for x in range(10)), Iterator)
True
>>> isinstance([], Iterator)
False
>>> isinstance({}, Iterator)
False
>>> isinstance('abc', Iterator)
False
```

