# collections

## 说明
**This module implements specialized container datatypes providing alternatives to Python'sgeneral purpose built-in containers, dict,list, set, and tuple.**
> 就是来替代python内置的容器类型的，提供更高效，更快速的容器，提供的方法
```python
__all__ = ['deque', 'defaultdict', 'namedtuple', 'UserDict', 'UserList',
            'UserString', 'Counter', 'OrderedDict', 'ChainMap']
__all__ += _collections_abc.__all__
```
>还包括_collections_abc中的所有方法，_collections_abc中的暂不考虑


### counter
**Counter是一个dict的子类，用于计数可哈希对象。它是一个集合，元素像字典键(key)一样存储，它们的计数存储为值**
```python
In [3]: from collections import Counter

In [4]: c=Counter("abcdeabcdabcaba")

In [5]: c
Out[5]: Counter({'a': 5, 'b': 4, 'c': 3, 'd': 2, 'e': 1})
```
> 有如下方法可用
#### elements
描述：返回一个迭代器，其中每个元素将重复出现计数值所指定次。 元素会按首次出现的顺序返回。 如果一个元素的计数值小于1，elements() 将会忽略它。

语法：elements( )
参数：无
```python
c = Counter(a=4, b=2, c=0, d=-2)
list(c.elements())
['a', 'a', 'a', 'a', 'b', 'b']


sorted(c.elements())
['a', 'a', 'a', 'a', 'b', 'b']


c = Counter(a=4, b=2, c=0, d=5)
list(c.elements())
['a', 'a', 'a', 'a', 'b', 'b', 'd', 'd', 'd', 'd', 'd'
```

#### most_common()
返回一个列表，其中包含n个最常见的元素及出现次数，按常见程度由高到低排序。 如果 n 被省略或为None，most_common() 将返回计数器中的所有元素，计数值相等的元素按首次出现的顺序排序，经常用来计算top词频的词语。

```python
Counter('abracadabra').most_common(3)
[('a', 5), ('b', 2), ('r', 2)]


Counter('abracadabra').most_common(5)
[('a', 5), ('b', 2), ('r', 2), ('c', 1), ('d', 1)]
```

#### subtract()
从迭代对象或映射对象减去元素。像dict.update() 但是是减去，而不是替换。输入和输出都可以是0或者负数。
```python
c = Counter(a=4, b=2, c=0, d=-2)
d = Counter(a=1, b=2, c=3, d=4)
c.subtract(d)
c
Counter({'a': 3, 'b': 0, 'c': -3, 'd': -6})

#减去一个abcd
str0 = Counter('aabbccdde')
str0
Counter({'a': 2, 'b': 2, 'c': 2, 'd': 2, 'e': 1})

str0.subtract('abcd')
str0
Counter({'a': 1, 'b': 1, 'c': 1, 'd': 1, 'e': 1}
```

#### 字典方法
通常字典方法都可用于Counter对象，除了有两个方法工作方式与字典并不相同。

fromkeys(iterable)

这个类方法没有在Counter中实现。

update([iterable-or-mapping])

从迭代对象计数元素或者从另一个映射对象 (或计数器) 添加。 像 dict.update() 但是是加上，而不是替换。另外，迭代对象应该是序列元素，而不是一个 (key, value) 对。

```python
sum(c.values())                 # total of all counts
c.clear()                       # reset all counts
list(c)                         # list unique elements
set(c)                          # convert to a set
dict(c)                         # convert to a regular dictionary
c.items()                       # convert to a list of (elem, cnt) pairs
Counter(dict(list_of_pairs))    # convert from a list of (elem, cnt) pairs
c.most_common()[:-n-1:-1]       # n least common elements
+c                              # remove zero and negative counts
```

#### 数学操作
这个功能非常强大，提供了几个数学操作，可以结合 Counter 对象，以生产 multisets (计数器中大于0的元素）。 加和减，结合计数器，通过加上或者减去元素的相应计数。交集和并集返回相应计数的最小或最大值。每种操作都可以接受带符号的计数，但是输出会忽略掉结果为零或者小于零的计数。

```python
c = Counter(a=3, b=1)
d = Counter(a=1, b=2)
c + d                       # add two counters together:  c[x] + d[x]
Counter({'a': 4, 'b': 3})
c - d                       # subtract (keeping only positive counts)
Counter({'a': 2})
c & d                       # intersection:  min(c[x], d[x]) 
Counter({'a': 1, 'b': 1})
c | d                       # union:  max(c[x], d[x])
Counter({'a': 3, 'b': 2})
```

单目加和减（一元操作符）意思是从空计数器加或者减去。
```python
c = Counter(a=2, b=-4)
+c
Counter({'a': 2})
-c
Counter({'b': 4})
```
写一个计算文本相似的算法，加权相似
```python
def str_sim(str_0,str_1,topn):
    topn = int(topn)
    collect0 = Counter(dict(Counter(str_0).most_common(topn)))
    collect1 = Counter(dict(Counter(str_1).most_common(topn)))       
    jiao = collect0 & collect1
    bing = collect0 | collect1       
    sim = float(sum(jiao.values()))/float(sum(bing.values()))        
    return(sim)         


str_0 = '定位手机定位汽车定位GPS定位人定位位置查询'         
str_1 = '导航定位手机定位汽车定位GPS定位人定位位置查询'         


str_sim(str_0,str_1,5)    
0.75  
```

## 双向队列-deque
**双端队列，可以快速的从另外一侧追加和推出对象,deque是一个双向链表，针对list连续的数据结构插入和删除进行优化。它提供了两端都可以操作的序列，这表示在序列的前后你都可以执行添加或删除操作。双向队列(deque)对象支持以下方法：**
