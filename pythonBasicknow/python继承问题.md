# python继承问题
## 继承顺序（大体思路：经典类深度优先，新式类广度优先）
> 3C算法
* 我们把L[C]定义为类C的的linearization值(也就是MRO里的继承顺序，后面简称L值)，计算逻辑如下：

L[C] = C + merge of linearization of parents of C and list of parents of C in the order they are inherited from left to right.
即L[C]等于C的所有上一级父类的L值的融合

```python
class X(object):
    pass

class Y(object):
    pass


class Z(object):
    pass


class A(X, Y):
    pass


class B(Y, Z):
    pass


class M(A, B, Z):
    pass
```
```shell
L[x]=XO
L[Y]=YO
L[Z]=ZO
L[A]=A+M(L[X]+L[Y]+XY)
    =A+M(XO+YO+XY)--从左往右依次取出和内部类无被继承关系的类
    =A+X+M(O+YO+Y)
    =A+X+Y+O
    =AXYO

L[M]=M+M(L[A]+L[B]+L[Z]+ABZ)
    =M+M(AXYO+BYZO+ZO+ABZ)
    =M+A+M(XYO+BYZO+ZO+BZ)
    =M+A+X+M(YO+BYZO+ZO+BZ)
    =M+A+X+B+M(YO+YZO+ZO+Z)
    =M+A+X+B+Y+M(O+ZO+ZO+Z)
    =M+A+X+B+Y+Z+O
    =MAXBYZO
```

# super继承
* https://rhettinger.wordpress.com/2011/05/26/super-considered-super/

> super不是继承父类申明，super只是给当前类对象输出一条继承链，并指出下一步继承“调用”的是哪一个类。
super 其实干的是这件事：
```python
def super(cls, inst):
    mro = inst.__class__.mro()
    return mro[mro.index(cls) + 1]
```
两个参数 cls 和 inst 分别做了两件事：
1. inst 负责生成 MRO 的 list
2. 通过 cls 定位当前 MRO 中的 index, 并返回 mro[index + 1]
