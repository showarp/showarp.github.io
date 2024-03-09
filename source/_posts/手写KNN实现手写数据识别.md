---
title: 手写KNN实现手写数据识别
date: 2022-07-17 22:29:08
categories:
    - 机器学习
tags:
    - knn
    - 手写数字识别
---
邻近算法，或者说K最邻近（KNN，K-NearestNeighbor）分类算法是数据挖掘分类技术中最简单的方法之一。所谓K最近邻，就是K个最近的邻居的意思，说的是每个样本都可以用它最接近的K个邻近值来代表。近邻算法就是将数据集合中每一个记录进行分类的方法————百度百科
## 数据集
首先我们先来看一下数据集长什么样子   
![](https://files.catbox.moe/wwgj5s.png)   
KNN目录下跟为两个文件夹testDigits顾名思义是测试及，trainingDigits顾名思义就是训练集，两个目录下均是以文本的形式存储手写数字  
![](https://files.catbox.moe/kmn5ei.png)  
"\_"前的数字代表了该手写数字对应的label"\_"，后面的则代表了该数字的序号（这个不是很重要）  
然后文件里面具体就是32*32的一个0，1矩阵用1来表示笔画。   
![](https://files.catbox.moe/my60a6.png)
## 思路
首先看到数据集第一时间就得想到把这个01图片从二维转换到一维，这样才方便表示表示数据也方便操作数据，那么都把数据变成一维的了而且一维的数据里面又全是0或者1，那么把他当成二进制也很好理解吧，这样的好处有两点  
1.节省空间，由于原来的数据是字符形式一个自负的话占用8bit也就是1字节，把它转换成2进制的话可以节省8倍的空间。  
2.方便计算距离（下面细🔒）  
把数据转换成2进制之后如何表示数据和数据之间的距离呢？我这里想到的方法是把两个数据（二进制）使用异操作（^）比如：  
010101^010100==000001  
然后再统计1的个数便可知道两个数据集之间距离相差多少（1的个数越小 两个数据集之间的距离就越小）  
然后拿样本去和数据集中得数据一一比较，得出样本呢和数据集中每一个数据的距离之后便可排序一下筛选出前k个差别最小的数据然后统计一下哪个数据最多即可得出预测结果。
## 代码实现
```python
import os
from collections import Counter
def readdDataFile(path):
    '''
    加载数据以及标签的函数 return 数据，标签
    '''
    filenames = os.listdir(path)## 读取文件夹下的目录名
    labels = [*map(lambda x:x.split('_')[0],filenames)] #把无用信息处理掉(只保留“_”前面的就好了)
    
    datas = [] #用于保存数据
    for i in filenames:
        f = open(f"{path}{i}")
        f=f.read().replace('\n','')#读取文件内容并且把数字图片转换成一行的形式（二维转一维）
        toDecimal=int(f,2) #转换成10进制
        datas.append(toDecimal)
    return datas,labels
datas,labels=readdDataFile('./trainingDigits/')
def predict(ipt,data,label,k):
    '''
    预测数据
    '''
    ipt = int(open(ipt).read().replace('\n',''),2)#读取要被预测的手写数字
    klst = [(bin(ipt^i).count('1'),x) for x,i in enumerate(data)] #和数据集种的数据比较距离
    klst.sort(key = lambda x:x[0])
    prelst = [label[klst[i][1]] for i in range(k)]
    res = Counter(prelst)
    res = dict(zip(res.values(),res.keys()))
    return res[max(res)]

def test():
    '''
    测试函数
    '''
    filenames = os.listdir('./testDigits/')
    succCount = 0
    for x,i in enumerate(filenames):
        lb = i.split('_')[0]
        res=predict(f'./testDigits/{i}',datas,labels,3)
        if res==lb :succCount+=1
    print(f"准确率:{succCount/len(filenames)*100}")
test()
```
## 准确率
最终测试得出得准确率有百分之98左右，感觉还不错。。。。。。