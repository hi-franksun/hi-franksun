

abs(x)
返回数的绝对值，整数（integer）、小数（float）、复数（complex）
```python
abs(10) # 10
abs(-10) # 10
abs()
```


all(iterable)
如果 iterable 的所有元素均为真值（**或可迭代对象为空**）则返回 True 
等价于
```python
def all(iterable):
    for element in iterable: # 为空时候则不执行，直接返回了 True
        if not element:
            return False
    return True

```

```python
a = "test"
b = []
c = [0, 1]
print(all(a)) # True
print(any(b)) # True
print(all(c)) # False

```


any(iterable)
如果 iterable 的任一元素为真值则返回 True。 **如果可迭代对象为空，返回 False**。 等价于:
```python
def any(iterable):
    for element in iterable:
        if element:
            return True
    return False
```

```python
a = "test"
b = []
c = [0, 1]
print(any(a)) # True
print(any(b)) # False
print(any(c)) # True
```



bool(x)
返回一个布尔值，通过 [[逻辑值检测]] True 或者 False。

bool 类是 int 的子类（参见 数字类型 --- int, float, complex）：

```python
class bool(int):
    """
    bool(x) -> bool
	"""
```

其他类不能继承自它。它只有 False 和 True 两个实例；
```python
a = list()
b = []
c = 0
d = "a"
print(bool(a)) # False
print(bool(b)) # False
print(bool(c)) # False
print(bool(d)) # True
```


sum(iterable, start=0)
从 start 开始自左向右对 iterable 的项求和并返回总计值。 iterable 的项通常为数字，而 start 值则不允许为字符串。

注意：3.8 支持关键字参数，之前的版本不支持关键字参数

```python
a = [1, 2, 3]
result = sum(a, start=0) # 支持关键字参数
print(result)


a = [1, 2, 3]
result = sum(a, 0) # 不支持关键字参数
print(result)
```




#Python3 
