# loop notice

## loop order(循环顺序)

- 对任意序列进行迭代（例如列表或字符串），条目的迭代顺序与它们在序列中出现的顺序一致。

```python
words = ['cat', 'window', 'defenestrate']
result = {}
for w in words:
    result[w] = len(w)
print(result)
# {'cat': 3, 'window': 6, 'defenestrate': 12}
```

## copy or create itere/collection(复制/创建新的集合)

- 在遍历同一个集合时修改该集合的代码可能很难获得正确的结果。通常，更直接的做法是循环遍历该集合的副本或创建新集合：

Strategy:  Iterate over a copy

```python
for user, status in users.copy().items():
    if status == 'inactive':
        del users[user]
```

Strategy:  Create a new collection

```python
active_users = {}
for user, status in users.items():
    if status == 'active':
        active_users[user] = status
```

## loop immutable type

在遍历 immutable 类型时候，remove 和 insert 会导致一个内部计数器问题，计数器在达到 sequence 长度时候会终止，但是删除和插入会导致计数器发生变化，比如一下例子：

> 其他编程语言的 for 循环都是有一个计数器的，比如 i++，当 i > 10 的时候，终止这个循环，而 for 循环的计数器会在每次每次 loop 后+1，当前项/之前项删除后，计数器还是增加的，但是下一个项因为删除一个项，被提前了，所以会漏掉下一个项；

```python
a = [1, 2, 3, 4]
for i in a:
    if i > 2:
        a.remove(i)
print(a)  # [1, 2, 4]
# 在遍历的时候，遍历到 3 的时候，a 会移除 3，这个时候 a = [1, 2, 4]，len(a) 就是 3，再继续 i=4，这个时候，就是上边说的计数器终止了，不会执行下边的判断，直接打印了 a，其实就是原来 4 索引是 3，删除一个后变成了 2，那么自然会漏掉了索引 3，所以这个例子就会导致了最后除了严重的错判！
```

---

## 循环的技巧

### dict loop get key and value

一般的 dict 循环，只会取出 key

```python
a = {'city': 'nanjing', 'age': 26}
for k, v in a:
    print(k, v)
# ValueError: too many values to unpack (expected 2) ：unpack error
```

如果要取出 key 和 value，那么就要用到 item() method，会 return 一个 tuple；

```python
a = {'city': 'nanjing', 'age': 26}
for k, v in a.items():
    print((k, v))
# ('city', 'nanjing')
# ('age', 26)
```

### enumerate fuction get iterable index

enumerate will get iterable object index and value, finally return tuple with index and value

```python
b = ['beijing', 'shanghai', 'nanjing', 'guangzhou']
for i in enumerate(b):
    print(i)

# (0, 'beijing')
# (1, 'shanghai')
# (2, 'nanjing')
# (3, 'guangzhou')

c = ('beijing', 'shanghai', 'nanjing', 'guangzhou')
for i in enumerate(b):
    print(i)
# (0, 'beijing')
# (1, 'shanghai')
# (2, 'nanjing')
# (3, 'guangzhou')

d = '123'
for i in enumerate(d):
    print(i)
# (0, '1')
# (1, '2')
# (2, '3')
```

### zip function

zip() 只能匹配相同数量的 elements sequence，如果数量不相等，会按照最低数量进行匹配，高的 element 会舍弃，return tulpe；

```python
a = [1, 2, 3, 4]
b = ('a', 'b', 'c')

for i in zip(a, b):
    print(i)
# (1, 'a')
# (2, 'b')
# (3, 'c')
```
