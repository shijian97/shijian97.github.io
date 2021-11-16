---
layout: 
title: about-torch-dataenhance
date: 2021-09-07 11:18:29
tags:
- torch
- dataenhance
categories:
- [学习, coding]
---

torch中使用数据增强的一些技巧和方法。
<!-- more -->

### 对tensor进行shuffle
#### 对所有element随机shuffle
```
t=torch.tensor([[1, 2, 3],[3, 4, 5]])
print(t)
idx = torch.randperm(t.nelement())
t = t.view(-1)[idx].view(t.size())
print(t)
```

#### 按照某一特定维度进行shuffle
```
t=torch.tensor([[1, 2, 3],[3, 4, 5]])
print(t)
idx = torch.randperm(t.shape[1])
t = t[:, idx].view(t.size())
print(t)
```

把原先tensor中的数据按照行优先的顺序排成一个一维的数据（这里应该是因为要求地址是连续存储的），然后按照参数组合成其他维度的tensor。比如说是不管你原先的数据是[[[1,2,3],[4,5,6]]]还是[1,2,3,4,5,6]，因为它们排成一维向量都是6个元素，所以只要view后面的参数一致，得到的结果都是一样的。