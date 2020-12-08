

## enumerate
 
enumerate(iterable, start=0)

返回一个枚举对象。iterable 必须是一个序列，或 iterator，或其他支持迭代的对象。 enumerate() 返回的迭代器的 __next__() 方法返回一个元组，里面包含一个计数值（从 start 开始，默认为 0）和通过迭代 iterable 获得的值。

```python
seasons = ['Spring', 'Summer', 'Fall', 'Winter']
result = enumerate(seasons)
print(result)
print(type(result))
print(list(result))

# <enumerate object at 0x00000224ACC1FB88>
# <class 'enumerate'>
# [(0, 'Spring'), (1, 'Summer'), (2, 'Fall'), (3, 'Winter')]
```


作用：
> enumerate is useful for obtaining an indexed list（枚举在获取索引列表上非常有用）
        (0, seq[0]), (1, seq[1]), (2, seq[2]), ...

等价于：
```python
def enumerate(sequence, start=0):
    n = start
    for elem in sequence:
        yield n, elem
        n += 1
```


zip()
