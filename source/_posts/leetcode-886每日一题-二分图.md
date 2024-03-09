---
title: leetcode.886每日一题(二分图)
date: 2022-10-18 16:36:56
comment: true
categories:
    - 数据结构
tags:
    - 二分图
    - 并查集
    - dfs
---
今天的每日一题  
给定一组 n 人（编号为 1, 2, ..., n）， 我们想把每个人分进任意大小的两组。每个人都可能不喜欢其他人，那么他们不应该属于同一组。

给定整数 n 和数组 dislikes ，其中 dislikes[i] = [ai, bi] ，表示不允许将编号为 ai 和  bi的人归入同一组。当可以用这种方法将所有人分进两组时，返回 true；否则返回 false。

示例1：  
```
输入：n = 4, dislikes = [[1,2],[1,3],[2,4]]
输出：true
解释：group1 [1,4], group2 [2,3]
```


示例2：  
```
输入：n = 3, dislikes = [[1,2],[1,3],[2,3]]
输出：false
```

示例3：
```
输入：n = 5, dislikes = [[1,2],[2,3],[3,4],[4,5],[1,5]]
输出：false
```

起初看到这道题的时候我的第一想法是想到排列组合的方法查看是否有可以把这些互相不喜欢的人分到两组的情况，但是结果是可行的，但是超时也是没办法的。

```python
class Solution:
    def possibleBipartition(self, n: int, dislikes: List[List[int]]) -> bool:
        if len(dislikes)==0:return True
        result = [] #记录每种组合是否可以分开两组
        def get_next_child(x):# 获取当前组合情况下的下一种组合方式
            if len(x['red'])==0:
                return [dislikes[0],dislikes[0][::-1]]
            i=sorted([x['red'][-1],x['blue'][-1]])
            nextidx = dislikes.index(i)+1
            if nextidx==len(dislikes):return []
            else:
                return [dislikes[nextidx],dislikes[nextidx][::-1]]
        def dfs(x): #dfs遍历
            if len(x['red'])!=0 and ((x['red'][-1] in x['blue'][:-1] and  x['blue'][-1] in x['blue'][:-1]) or (x['red'][-1] in x['red'][:-1] and x['blue'][-1] in x['red'][:-1])):
                result.append(False)
                return
            next_chile = get_next_child(x)
            if next_chile==[]:
                if len(set(x['red'])&set(x['blue']))>0:
                    result.append(False)
                else:result.append(True)#如果有一种成功的组合则返回true
                return
            for i in next_chile:
                dfs({'red':x['red']+[i[0]],'blue':x['blue']+[i[1]]})
        dfs({'red':[],'blue':[]})
        return False if sum(result)==0 else True
```
然后超时

最后仔细搜索了一下发现这个题其实是一个叫做二分图的一个数据结构

# 二分图
其意思是一张图可以把可以把各个顶点分到两个集合,并且两个集合内的顶点互相独立没有边，就像这样
![](https://files.catbox.moe/1a8wnz.png)

结合题目可以把这些讨厌的关系画成一张二分图，互相讨厌的人分为两个组，组内的人互相喜欢，那么题目的意思就很明了了，就是判断这张图是否可以是一张二分图。

# 染色法
刚开始接触染色法这个概念，就按着自己对概念的理解做了一下，大概的意思就是初始化一个数组color，len(color)==n，所有元素都初始为0（无色）,然后遍历dislikes，给ai赋值为1（红色） bi赋值为2（蓝色），如果赋值的过程中发现ai 和bi 都为 红色或者蓝色的话就说明这个图无法形成二分图，遍历结束如果没有问题的话说明该图是一个二分图就返回True

```python
class Solution:
    def possibleBipartition(self, n: int, dislikes: List[List[int]]) -> bool:
        color = [0]*n
        g = defaultdict(list)# 创建图
        for i,j in dislikes:
            g[i-1].append(j-1)
            g[j-1].append(i-1)
        def dfs(i,c):
            #遍历每个人，给每个人分配颜色
            color[i]=c
            for j in g[i]:#遍历讨厌的人
                if color[j] == c:
                    #如果讨厌的人和自己颜色一样，则返回False
                    return False
                if color[j]==0 and not dfs(j,3-c):
                    #如果讨厌的人还没有分组，那么就给他染上一个和自己相反的颜色。
                    # 如果染色的过程中有问题也就是说讨厌的人和自己是一个颜色的，那么就返回False（因为如果遇到讨厌的人的话会dfs(j,3-c)会返回False）
                    return False
            #遍历结束如果没问题 则返回True
            return True
        return all([c or dfs(i,1)for i,c in enumerate(color)])
```
# 并查集
这里附上一个[连接](https://www.bilibili.com/video/BV1Wv4y1f7n1?share_source=copy_web&vd_source=3467e5f049bb48e9ce8af16b4576eb76)，可以非常非常生动的快速介绍并查集的原理,

这里再简要的讲一下，并查集可以做两件事，第一就是查询某个点所属的集合，第二就是可以合并两点所在的集合，变成一个集合。

而并查集是树形结构，一棵树里面的所有结点为集合内的元素，而这棵树的根就是这个集合的名字，引用[知乎](https://zhuanlan.zhihu.com/p/93647900)上的一篇很生动的比喻，就是一群丐帮里面（元素），的代表，也就是帮主（集合名）。

对于这道题而言一张图里面的元素都最终会被划分进一个集合里面，但是我们需要最终分出两个互相讨厌集合，那么具体的做法就是把这张图再复制一张，里面的元素由n+1表示，那么再划分集合的时候如果发现两个互相讨厌的点属于一个集合内则返回False，否则把a讨厌的点b的分身(b+n)划分进一个a讨厌的集合内，相反把b讨厌的点a分身(a+n)划分进b讨厌的集合内，但是可能有人会有疑问，那么如果按照这样划分的话，那a集合里面岂不是有可能存在集合b里面的分身？-------------确实有可能，下面这种情况。
```
set a
---------
1 4 6 7
其中6 7 分别是set b中的2 3的分身(2+4,3+4)
```
```
set b
---------
2 3 5 8
其中5 8 分别是set a中的1 5的分身(1+4,5+4)
```

有这种情况其实是没问题的，因为我们判断两个点是否为一个集合的时候只判断他们的本体是否是一个集合`find(a)==find(b)` 而不是判断他的本体和他讨厌的分生是否在同一个集合`find(a)==find(b+n)`。

然后下面上代码
```python
p=list(range(2*n+1))
def find(x):
    if p[x] != x:
        p[x] = find(p[x])
    return p[x]
for a,b in dislikes:
    a = find(a)
    b = find(b)
    if a==b:return False
    p[find(a+n)] = b
    p[find(b+n)] = a
return True
```
大概就是上面那么多。~~其实理解起来还是很费劲的~~