---
layout: 
title: about-tensorboard
date: 2021-09-05 11:35:08
tags:
- tensorboard
- pytorch
categories:
- [学习, coding]
---

pytorch框架下的tensorboard使用
<!-- more -->

tensorboard原本是tensorflow的可视化工具，pytorch从1.2.0开始支持tensorboard。之前的版本也可以使用tensorboardX代替。

### tensorboard使用逻辑
- 将代码运行过程中的，某些你关心的数据保存在一个文件夹中。
这一步由writer完成
- 再读取这个文件夹中的数据，用浏览器显示出来。 
这一步通过tensorboard完成
### 使用流程
- 导入`from torch.utils.tensorboard import SummaryWriter`
- 实例化`writer = SummaryWriter('./path/to/log')`
- 