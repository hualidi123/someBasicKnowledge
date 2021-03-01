# python3.7之后 散列冲突说明
> 问题起源《流畅的python》，查了很多资料文档，蹭着下午茶整理下

1. python 3.6之前，字典是无序的
> 当声明一个字典时，会默认生成一个8X3的多维数组，插入数据时，先hash（key）的值，再用算法处理hash值后对8（数组长度）取模，得到的模就是插入数据的数组下标，插入 [hash(key),key的内存地址，value的内存地址]，当取到的下标值已有数据，对比key的值，相同更新value，不相同散列冲突，往下走寻找空位插入；取到的下标没有数据，直接插入。默认生成的列表是稀疏的，就是为了冲突是更快的找到空位，当占用的位置达到列表长度的2/3，列表就是扩容，重新取模插入。

- 计算key的hash值【hash(key)】，再和mask做与操作【mask=字典最小长度（DictMinSize） - 1】，运算后会得到一个数字【index】，这个index就是要插入的enteies哈希表中的下标位置
- 若index下标位置已经被占用，则会判断enteies的key是否与要插入的key是否相等
- 如果key相等就表示key已存在，则更新value值
- 如果key不相等，就表示hash冲突，则会继续向下寻找空位置，一直到找到剩余空位为止。

2. python3.6 不讨论，3.6不是完全的有序

3. python 3.7后字典是有序的
- 申明字典时，初始两个数组
```python
indexies=[None,None,None,None,None,None,None,None] # 索引数组
metries=[] # 数据时间插入数组
```
- 插入{'name':'lqm'},通过key(name)的hash值运算出一个索引值，假设是2，metries为空，插入metries的0下标位置，在indexies中的下标2处改为0
```python
indexies=[None,None,0,None,None,None,None,None] # 索引数组
metries=[hask(key),'name','lqm'] # 数据时间插入数组
```
- 但是当通过key(name)的hash值运算出一个索引值已在indexies中时，冲突发生

> python3.7处理这个冲突的方法是开放地址法，数学递推公式可以理解为： H = (Hash(key)+d)%m，其中m代表散列表长度，d为增量序列，d一般有四种取法：线性探测法、平方探测法、再散列法、伪随机序列法。默认是线性探测法，即线性的增加d的值，然后重新计算索引值。


