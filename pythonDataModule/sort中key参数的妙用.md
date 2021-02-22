# list.sort、sorted、max 和 min 函数的 key 参数

```python
l = [28, 14, '28', 5, '9', '1', 0, 6, '23', 19]
l1 = sorted(l, key=int)
print(l1)
l2 = sorted(l, key=str)
print(l2)
```

> 输出
```shell
[0, '1', 5, 6, '9', 14, 19, '23', 28, '28']
[0, '1', 14, 19, '23', 28, '28', 5, 6, '9']
```
