---
title: 关于python数组比较
date: 2023-01-24 21:29:31
categories:
    - 语言特点
tags:
    - python
---
# 故事背景
今天在用python自带的库heapq做堆排序的时候遇到了一个问题，在heapq中初始化堆列表`heapq.heapify(arr)`的时候由于`arr`是个二维数组`[[1,3],[5,7],[9,6]]`我想让这个函数根据每个元素里面的第二个元素为key来做排序,但是`heapq.heapify(arr)`又没有key参数。
# 深究细节
在我们平常对两个数组的做判断的时候用得最多的时候就是`==`和`!=`但是大家可能忽略了个问题，其实数组也是可以用`>`和`<`做比较的,比如......
```python
>>> [1,2,3]>[3,2,1]
False
>>> [1,2,3]<[3,2,1]
True
>>>
```
```python
>>> [[[3,5],2],1]>[[[1],[5],2],3]
True
>>> [[[3,5],2],1]<[[[1],[5],2],3]
False
```
相信大家看到这里已经看出来数组的比较规则了，就是第一个元素深度优先找到最底层非容器元素然后进行比较，但是这以上的前提是这两个数组的维度`(ndim)`是相同的。如果不相同则会报错。
```python
>>> [[[1]]]<[[[[[[5]]]]]]
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: '<' not supported between instances of 'list' and 'int'
>>>
```
所以在使用`heapify(arr)`的时候把要比较的元素放在第一位就好了`[[3,1],[7,5],[6,9]]`



祝大家新年快乐🐰🐰🐰